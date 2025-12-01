# 💻 SQL PRACTICE PROBLEMS

## Work through these problems on paper (whiteboard simulation)!

---

## SETUP: Sample Tables

Imagine these tables exist (you'll use them for problems below):

```sql
-- employees table
CREATE TABLE employees (
    employee_id INT,
    name VARCHAR(100),
    department VARCHAR(50),
    salary DECIMAL(10,2),
    manager_id INT,
    hire_date DATE
);

-- sales table
CREATE TABLE sales (
    sale_id INT,
    customer_id INT,
    region VARCHAR(50),
    product VARCHAR(100),
    amount DECIMAL(10,2),
    sale_date DATE
);

-- orders table
CREATE TABLE orders (
    order_id INT,
    customer_id INT,
    order_date DATE,
    total_amount DECIMAL(10,2),
    status VARCHAR(20)
);

-- customers table
CREATE TABLE customers (
    customer_id INT,
    name VARCHAR(100),
    email VARCHAR(100),
    join_date DATE
);
```

---

# SECTION 1: JOIN PROBLEMS 🔗

## Problem 1.1: Basic LEFT JOIN
**Find all customers and their orders. Include customers who haven't placed any orders.**

<details>
<summary>Click for Solution</summary>

```sql
SELECT 
    c.customer_id,
    c.name,
    o.order_id,
    o.order_date,
    o.total_amount
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
ORDER BY c.customer_id;
```

**Key Insight:** LEFT JOIN ensures we keep ALL customers, even those with no orders (their order columns will be NULL).

</details>

---

## Problem 1.2: Customers Who Never Ordered
**Find customers who have NEVER placed an order.**

<details>
<summary>Click for Solution</summary>

```sql
-- Method 1: LEFT JOIN + NULL check
SELECT c.customer_id, c.name
FROM customers c
LEFT JOIN orders o ON c.customer_id = o.customer_id
WHERE o.order_id IS NULL;

-- Method 2: NOT EXISTS
SELECT customer_id, name
FROM customers c
WHERE NOT EXISTS (
    SELECT 1 FROM orders o 
    WHERE o.customer_id = c.customer_id
);

-- Method 3: NOT IN
SELECT customer_id, name
FROM customers
WHERE customer_id NOT IN (
    SELECT DISTINCT customer_id FROM orders
);
```

**Key Insight:** After LEFT JOIN, rows without matches have NULL in the right table columns. Filter for those NULLs.

</details>

---

## Problem 1.3: Self-Join - Employees & Managers
**Find employees who earn MORE than their manager.**

<details>
<summary>Click for Solution</summary>

```sql
SELECT 
    e.name AS employee_name,
    e.salary AS employee_salary,
    m.name AS manager_name,
    m.salary AS manager_salary
FROM employees e
JOIN employees m ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;
```

**Key Insight:** SELF JOIN - joining a table to itself using different aliases (e for employee, m for manager).

</details>

---

## Problem 1.4: Multi-Table JOIN
**Find the total sales amount per customer, showing customer name and region.**

<details>
<summary>Click for Solution</summary>

```sql
SELECT 
    c.name AS customer_name,
    s.region,
    SUM(s.amount) AS total_sales
FROM customers c
JOIN sales s ON c.customer_id = s.customer_id
GROUP BY c.name, s.region
ORDER BY total_sales DESC;
```

</details>

---

## Problem 1.5: FULL OUTER JOIN
**Show all customers and all orders, including unmatched records from both sides.**

<details>
<summary>Click for Solution</summary>

```sql
SELECT 
    c.customer_id AS cust_id,
    c.name,
    o.order_id,
    o.total_amount
FROM customers c
FULL OUTER JOIN orders o ON c.customer_id = o.customer_id;
```

**Key Insight:** FULL OUTER shows:
- Customers with orders (matched)
- Customers without orders (customer cols filled, order cols NULL)
- Orders without customers (order cols filled, customer cols NULL) - orphan records

</details>

---

# SECTION 2: WINDOW FUNCTIONS - RANKING 🏆

## Problem 2.1: Top 3 Salaries Per Department
**Find the top 3 highest-paid employees in each department.**

<details>
<summary>Click for Solution</summary>

```sql
WITH ranked_employees AS (
    SELECT 
        name,
        department,
        salary,
        DENSE_RANK() OVER (
            PARTITION BY department 
            ORDER BY salary DESC
        ) AS salary_rank
    FROM employees
)
SELECT name, department, salary, salary_rank
FROM ranked_employees
WHERE salary_rank <= 3
ORDER BY department, salary_rank;
```

**Key Insight:** 
- DENSE_RANK handles ties without gaps
- PARTITION BY creates separate rankings per department
- Must use subquery/CTE to filter on window function result

</details>

---

## Problem 2.2: Second Highest Salary
**Find the second highest salary in the company.**

<details>
<summary>Click for Solution</summary>

```sql
-- Method 1: DENSE_RANK
SELECT salary AS second_highest
FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) AS rnk
    FROM employees
) ranked
WHERE rnk = 2;

-- Method 2: Subquery (no window function)
SELECT MAX(salary) AS second_highest
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);

-- Method 3: DISTINCT + OFFSET (PostgreSQL)
SELECT DISTINCT salary
FROM employees
ORDER BY salary DESC
OFFSET 1 LIMIT 1;
```

</details>

---

## Problem 2.3: Rank vs Dense_Rank vs Row_Number
**Show the difference between RANK, DENSE_RANK, and ROW_NUMBER for employee salaries.**

<details>
<summary>Click for Solution</summary>

```sql
SELECT 
    name,
    salary,
    ROW_NUMBER() OVER (ORDER BY salary DESC) AS row_num,
    RANK() OVER (ORDER BY salary DESC) AS rank,
    DENSE_RANK() OVER (ORDER BY salary DESC) AS dense_rank
FROM employees;

-- Example output if salaries are: 100, 95, 95, 90, 85
-- name     salary  row_num  rank  dense_rank
-- Alice    100     1        1     1
-- Bob      95      2        2     2
-- Charlie  95      3        2     2      <-- Bob and Charlie tie
-- David    90      4        4     3      <-- RANK skips 3, DENSE_RANK doesn't
-- Eve      85      5        5     4
```

**Key Insight:**
- ROW_NUMBER: Always unique (1, 2, 3, 4, 5)
- RANK: Gaps after ties (1, 2, 2, 4, 5)
- DENSE_RANK: No gaps (1, 2, 2, 3, 4)

</details>

---

## Problem 2.4: Top Customer Per Region
**Find THE top customer (by total sales) in each region. If tie, show all tied customers.**

<details>
<summary>Click for Solution</summary>

```sql
WITH customer_sales AS (
    SELECT 
        customer_id,
        region,
        SUM(amount) AS total_sales,
        DENSE_RANK() OVER (
            PARTITION BY region 
            ORDER BY SUM(amount) DESC
        ) AS rnk
    FROM sales
    GROUP BY customer_id, region
)
SELECT region, customer_id, total_sales
FROM customer_sales
WHERE rnk = 1
ORDER BY region;
```

</details>

---

## Problem 2.5: Percentile/Quartile Assignment
**Assign each customer to a quartile based on their total order amount.**

<details>
<summary>Click for Solution</summary>

```sql
SELECT 
    customer_id,
    SUM(total_amount) AS total_orders,
    NTILE(4) OVER (ORDER BY SUM(total_amount) DESC) AS quartile
FROM orders
GROUP BY customer_id;

-- Quartile 1 = Top 25% (highest spenders)
-- Quartile 4 = Bottom 25%
```

</details>

---

# SECTION 3: WINDOW FUNCTIONS - RUNNING CALCULATIONS 📈

## Problem 3.1: Running Total of Sales
**Calculate the running total of daily sales.**

<details>
<summary>Click for Solution</summary>

```sql
SELECT 
    sale_date,
    amount,
    SUM(amount) OVER (
        ORDER BY sale_date
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS running_total
FROM sales
ORDER BY sale_date;
```

**Key Insight:** 
- SUM() OVER with ORDER BY creates cumulative sum
- ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW is often implicit but explicit is clearer

</details>

---

## Problem 3.2: Running Total Per Region
**Calculate running total of sales within each region separately.**

<details>
<summary>Click for Solution</summary>

```sql
SELECT 
    region,
    sale_date,
    amount,
    SUM(amount) OVER (
        PARTITION BY region
        ORDER BY sale_date
    ) AS regional_running_total
FROM sales
ORDER BY region, sale_date;
```

**Key Insight:** PARTITION BY resets the running total for each region.

</details>

---

## Problem 3.3: 3-Day Moving Average
**Calculate a 3-day moving average of sales.**

<details>
<summary>Click for Solution</summary>

```sql
SELECT 
    sale_date,
    amount,
    AVG(amount) OVER (
        ORDER BY sale_date
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS moving_avg_3day
FROM (
    SELECT sale_date, SUM(amount) AS amount
    FROM sales
    GROUP BY sale_date
) daily_sales
ORDER BY sale_date;
```

**Key Insight:** ROWS BETWEEN 2 PRECEDING AND CURRENT ROW = current row + 2 previous rows = 3 days.

</details>

---

## Problem 3.4: Month-over-Month Change (LAG)
**Calculate the month-over-month revenue change and percentage change.**

<details>
<summary>Click for Solution</summary>

```sql
WITH monthly_revenue AS (
    SELECT 
        DATE_TRUNC('month', sale_date) AS month,
        SUM(amount) AS revenue
    FROM sales
    GROUP BY DATE_TRUNC('month', sale_date)
)
SELECT 
    month,
    revenue,
    LAG(revenue, 1) OVER (ORDER BY month) AS prev_month,
    revenue - LAG(revenue, 1) OVER (ORDER BY month) AS change,
    ROUND(
        (revenue - LAG(revenue, 1) OVER (ORDER BY month)) * 100.0 / 
        LAG(revenue, 1) OVER (ORDER BY month), 2
    ) AS pct_change
FROM monthly_revenue
ORDER BY month;
```

**Key Insight:** LAG(column, 1) gets the previous row's value. First row will have NULL for prev_month.

</details>

---

## Problem 3.5: Days Since Last Order (LAG)
**For each order, calculate days since the customer's previous order.**

<details>
<summary>Click for Solution</summary>

```sql
SELECT 
    customer_id,
    order_id,
    order_date,
    LAG(order_date, 1) OVER (
        PARTITION BY customer_id 
        ORDER BY order_date
    ) AS prev_order_date,
    order_date - LAG(order_date, 1) OVER (
        PARTITION BY customer_id 
        ORDER BY order_date
    ) AS days_since_last_order
FROM orders
ORDER BY customer_id, order_date;
```

**Key Insight:** PARTITION BY customer_id ensures we compare orders within same customer.

</details>

---

## Problem 3.6: Identify Consecutive Values (LAG/LEAD)
**Find products that had increasing sales for 3 consecutive months.**

<details>
<summary>Click for Solution</summary>

```sql
WITH monthly_sales AS (
    SELECT 
        product,
        DATE_TRUNC('month', sale_date) AS month,
        SUM(amount) AS revenue
    FROM sales
    GROUP BY product, DATE_TRUNC('month', sale_date)
),
with_comparisons AS (
    SELECT 
        product,
        month,
        revenue,
        LAG(revenue, 1) OVER (PARTITION BY product ORDER BY month) AS prev_month_rev,
        LAG(revenue, 2) OVER (PARTITION BY product ORDER BY month) AS two_months_ago_rev
    FROM monthly_sales
)
SELECT DISTINCT product
FROM with_comparisons
WHERE revenue > prev_month_rev 
  AND prev_month_rev > two_months_ago_rev;
```

</details>

---

# SECTION 4: AGGREGATION & GROUPING 📊

## Problem 4.1: GROUP BY with HAVING
**Find departments with average salary greater than $75,000.**

<details>
<summary>Click for Solution</summary>

```sql
SELECT 
    department,
    AVG(salary) AS avg_salary,
    COUNT(*) AS employee_count
FROM employees
GROUP BY department
HAVING AVG(salary) > 75000
ORDER BY avg_salary DESC;
```

**Key Insight:** 
- WHERE filters rows BEFORE grouping
- HAVING filters groups AFTER aggregation

</details>

---

## Problem 4.2: WHERE vs HAVING
**Find departments where the average salary of employees hired after 2020 exceeds $80,000.**

<details>
<summary>Click for Solution</summary>

```sql
SELECT 
    department,
    AVG(salary) AS avg_salary
FROM employees
WHERE hire_date > '2020-01-01'    -- Filter rows first
GROUP BY department
HAVING AVG(salary) > 80000        -- Then filter groups
ORDER BY avg_salary DESC;
```

</details>

---

## Problem 4.3: Finding Duplicates
**Find email addresses that appear more than once in the customers table.**

<details>
<summary>Click for Solution</summary>

```sql
-- Find duplicate emails
SELECT email, COUNT(*) AS occurrence_count
FROM customers
GROUP BY email
HAVING COUNT(*) > 1;

-- See all rows with duplicate emails
SELECT *
FROM customers
WHERE email IN (
    SELECT email 
    FROM customers 
    GROUP BY email 
    HAVING COUNT(*) > 1
);
```

</details>

---

## Problem 4.4: Count with Conditions
**Count orders by status, showing completed, pending, and cancelled counts per customer.**

<details>
<summary>Click for Solution</summary>

```sql
SELECT 
    customer_id,
    COUNT(*) AS total_orders,
    SUM(CASE WHEN status = 'completed' THEN 1 ELSE 0 END) AS completed,
    SUM(CASE WHEN status = 'pending' THEN 1 ELSE 0 END) AS pending,
    SUM(CASE WHEN status = 'cancelled' THEN 1 ELSE 0 END) AS cancelled
FROM orders
GROUP BY customer_id;
```

**Key Insight:** CASE inside SUM is a powerful pattern for conditional counting/aggregation.

</details>

---

# SECTION 5: SUBQUERIES & CTEs 🔄

## Problem 5.1: Correlated Subquery
**Find employees who earn above their department's average salary.**

<details>
<summary>Click for Solution</summary>

```sql
-- Method 1: Correlated subquery
SELECT name, department, salary
FROM employees e1
WHERE salary > (
    SELECT AVG(salary) 
    FROM employees e2 
    WHERE e2.department = e1.department
);

-- Method 2: Window function (more efficient)
SELECT name, department, salary
FROM (
    SELECT 
        name, 
        department, 
        salary,
        AVG(salary) OVER (PARTITION BY department) AS dept_avg
    FROM employees
) sub
WHERE salary > dept_avg;
```

</details>

---

## Problem 5.2: CTE for Readability
**Find customers who spent more than the average customer spend.**

<details>
<summary>Click for Solution</summary>

```sql
WITH customer_totals AS (
    SELECT 
        customer_id,
        SUM(total_amount) AS total_spent
    FROM orders
    GROUP BY customer_id
),
avg_spend AS (
    SELECT AVG(total_spent) AS avg_spent
    FROM customer_totals
)
SELECT 
    ct.customer_id,
    ct.total_spent,
    a.avg_spent
FROM customer_totals ct
CROSS JOIN avg_spend a
WHERE ct.total_spent > a.avg_spent
ORDER BY ct.total_spent DESC;
```

**Key Insight:** CTEs improve readability by naming intermediate results.

</details>

---

## Problem 5.3: Multiple CTEs
**Find the top-performing region and list its top 5 customers.**

<details>
<summary>Click for Solution</summary>

```sql
WITH regional_totals AS (
    SELECT 
        region,
        SUM(amount) AS total_sales
    FROM sales
    GROUP BY region
),
top_region AS (
    SELECT region
    FROM regional_totals
    ORDER BY total_sales DESC
    LIMIT 1
),
customer_sales_in_top_region AS (
    SELECT 
        s.customer_id,
        SUM(s.amount) AS total_sales,
        ROW_NUMBER() OVER (ORDER BY SUM(s.amount) DESC) AS rnk
    FROM sales s
    WHERE s.region = (SELECT region FROM top_region)
    GROUP BY s.customer_id
)
SELECT customer_id, total_sales
FROM customer_sales_in_top_region
WHERE rnk <= 5;
```

</details>

---

# SECTION 6: MSSQL-SPECIFIC PRACTICE 🔷

## Problem 6.1: TOP vs LIMIT
**Get the top 10 highest-paid employees (MSSQL syntax).**

<details>
<summary>Click for Solution</summary>

```sql
-- MSSQL syntax
SELECT TOP 10 name, salary
FROM employees
ORDER BY salary DESC;

-- PostgreSQL equivalent
SELECT name, salary
FROM employees
ORDER BY salary DESC
LIMIT 10;
```

</details>

---

## Problem 6.2: Date Functions (MSSQL)
**Find all orders from the last 30 days.**

<details>
<summary>Click for Solution</summary>

```sql
-- MSSQL
SELECT *
FROM orders
WHERE order_date >= DATEADD(day, -30, GETDATE());

-- PostgreSQL equivalent
SELECT *
FROM orders
WHERE order_date >= CURRENT_DATE - INTERVAL '30 days';
```

</details>

---

## Problem 6.3: DATEDIFF (MSSQL)
**Calculate the number of days since each employee was hired.**

<details>
<summary>Click for Solution</summary>

```sql
-- MSSQL
SELECT 
    name,
    hire_date,
    DATEDIFF(day, hire_date, GETDATE()) AS days_employed
FROM employees;

-- PostgreSQL equivalent
SELECT 
    name,
    hire_date,
    CURRENT_DATE - hire_date AS days_employed
FROM employees;
```

</details>

---

# SECTION 7: INTERVIEW SIMULATION 🎯

## Warm-Up (5 problems, 15 minutes total)

1. Find all unique regions from the sales table
2. Count employees per department
3. Find the maximum salary
4. List customers ordered by join date (newest first)
5. Find orders with total_amount > 1000

---

## Full Interview Simulation (30 minutes)

**Timer: Give yourself 5-7 minutes per problem. Write on PAPER first.**

### Interview Problem 1:
**"Write a query to find the top 2 customers by total spending in each region."**

<details>
<summary>Click for Solution</summary>

```sql
WITH customer_spending AS (
    SELECT 
        customer_id,
        region,
        SUM(amount) AS total_spent,
        DENSE_RANK() OVER (
            PARTITION BY region 
            ORDER BY SUM(amount) DESC
        ) AS spending_rank
    FROM sales
    GROUP BY customer_id, region
)
SELECT region, customer_id, total_spent
FROM customer_spending
WHERE spending_rank <= 2
ORDER BY region, spending_rank;
```

</details>

---

### Interview Problem 2:
**"Calculate the running total of orders for each customer, ordered by date."**

<details>
<summary>Click for Solution</summary>

```sql
SELECT 
    customer_id,
    order_date,
    total_amount,
    SUM(total_amount) OVER (
        PARTITION BY customer_id 
        ORDER BY order_date
    ) AS running_total
FROM orders
ORDER BY customer_id, order_date;
```

</details>

---

### Interview Problem 3:
**"Find customers whose latest order was more than 90 days ago (inactive customers)."**

<details>
<summary>Click for Solution</summary>

```sql
-- MSSQL version
SELECT customer_id, MAX(order_date) AS last_order_date
FROM orders
GROUP BY customer_id
HAVING DATEDIFF(day, MAX(order_date), GETDATE()) > 90;

-- PostgreSQL version
SELECT customer_id, MAX(order_date) AS last_order_date
FROM orders
GROUP BY customer_id
HAVING MAX(order_date) < CURRENT_DATE - INTERVAL '90 days';
```

</details>

---

### Interview Problem 4:
**"For each product, show the month-over-month sales change."**

<details>
<summary>Click for Solution</summary>

```sql
WITH monthly_product_sales AS (
    SELECT 
        product,
        DATE_TRUNC('month', sale_date) AS month,
        SUM(amount) AS monthly_revenue
    FROM sales
    GROUP BY product, DATE_TRUNC('month', sale_date)
)
SELECT 
    product,
    month,
    monthly_revenue,
    LAG(monthly_revenue) OVER (PARTITION BY product ORDER BY month) AS prev_month,
    monthly_revenue - LAG(monthly_revenue) OVER (PARTITION BY product ORDER BY month) AS change
FROM monthly_product_sales
ORDER BY product, month;
```

</details>

---

### Interview Problem 5:
**"Find employees who are the highest paid in their department."**

<details>
<summary>Click for Solution</summary>

```sql
WITH ranked AS (
    SELECT 
        name,
        department,
        salary,
        RANK() OVER (PARTITION BY department ORDER BY salary DESC) AS rnk
    FROM employees
)
SELECT name, department, salary
FROM ranked
WHERE rnk = 1;
```

</details>

---

## 📝 SCORING YOURSELF

After each problem, check:
- [ ] Correct result?
- [ ] Efficient approach?
- [ ] Clean, readable code?
- [ ] Completed in time?

**Target: 4/5 correct = Ready for interview!**

---

**Practice until these patterns are automatic! 🎯**

