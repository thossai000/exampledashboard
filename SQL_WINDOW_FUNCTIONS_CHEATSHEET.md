# 🪟 SQL WINDOW FUNCTIONS CHEAT SHEET

## Print this and tape to your wall during practice!

---

## THE UNIVERSAL PATTERN

```sql
SELECT 
    column1,
    column2,
    WINDOW_FUNCTION() OVER (
        PARTITION BY grouping_column   -- Optional: divides into groups
        ORDER BY ordering_column       -- Required for ranking/running calcs
        ROWS BETWEEN frame_start AND frame_end  -- Optional: defines window frame
    ) AS result_alias
FROM table_name;
```

---

## RANKING FUNCTIONS

### ROW_NUMBER() - Unique Sequential Numbers
```sql
-- Assigns 1, 2, 3, 4, 5... (no duplicates even with ties)
SELECT 
    employee_name,
    department,
    salary,
    ROW_NUMBER() OVER (
        PARTITION BY department 
        ORDER BY salary DESC
    ) AS row_num
FROM employees;
-- Result: Each department gets employees numbered 1, 2, 3...
```

**USE WHEN:** You need unique numbering, or "first" record per group

---

### RANK() - Rank WITH Gaps
```sql
-- Assigns 1, 2, 2, 4, 5... (skips numbers after ties)
SELECT 
    student_name,
    score,
    RANK() OVER (ORDER BY score DESC) AS rank
FROM exam_results;
-- If two students tie for 2nd, next is 4th (skips 3rd)
```

**USE WHEN:** Traditional competitive ranking (Olympics-style)

---

### DENSE_RANK() - Rank WITHOUT Gaps ⭐ MOST COMMON IN INTERVIEWS
```sql
-- Assigns 1, 2, 2, 3, 4... (no gaps after ties)
SELECT 
    product_name,
    category,
    revenue,
    DENSE_RANK() OVER (
        PARTITION BY category 
        ORDER BY revenue DESC
    ) AS rank
FROM products;
-- If two products tie for 2nd, next is 3rd (no gap)
```

**USE WHEN:** "Top N per group" questions (interview favorite!)

---

### NTILE(n) - Divide into Buckets
```sql
-- Divides rows into n equal groups
SELECT 
    customer_id,
    total_purchases,
    NTILE(4) OVER (ORDER BY total_purchases DESC) AS quartile
FROM customers;
-- Result: Top 25% = 1, Next 25% = 2, etc.
```

**USE WHEN:** Creating percentiles, quartiles, segments

---

## VALUE FUNCTIONS

### LAG(column, offset, default) - Previous Row Value
```sql
-- Access value from previous row
SELECT 
    month,
    revenue,
    LAG(revenue, 1) OVER (ORDER BY month) AS prev_month_revenue,
    revenue - LAG(revenue, 1) OVER (ORDER BY month) AS month_change
FROM monthly_sales;
```

**USE WHEN:** Month-over-month, day-over-day comparisons

---

### LEAD(column, offset, default) - Next Row Value
```sql
-- Access value from next row
SELECT 
    employee_id,
    hire_date,
    LEAD(hire_date, 1) OVER (ORDER BY hire_date) AS next_hire_date
FROM employees;
```

**USE WHEN:** Forward-looking analysis, gaps detection

---

### FIRST_VALUE() / LAST_VALUE()
```sql
-- Get first or last value in window
SELECT 
    product,
    sale_date,
    amount,
    FIRST_VALUE(amount) OVER (
        PARTITION BY product 
        ORDER BY sale_date
    ) AS first_sale_amount
FROM sales;
```

---

## AGGREGATE WINDOW FUNCTIONS

### SUM() OVER() - Running Totals ⭐ INTERVIEW FAVORITE
```sql
-- Cumulative sum
SELECT 
    sale_date,
    amount,
    SUM(amount) OVER (
        ORDER BY sale_date 
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS running_total
FROM daily_sales;
```

**USE WHEN:** Running/cumulative totals

---

### AVG() OVER() - Moving Average
```sql
-- 3-day moving average
SELECT 
    sale_date,
    amount,
    AVG(amount) OVER (
        ORDER BY sale_date 
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS moving_avg_3day
FROM daily_sales;
```

**USE WHEN:** Smoothing trends, reducing noise

---

### COUNT() OVER() - Running Count
```sql
-- Count within partition
SELECT 
    department,
    employee_name,
    COUNT(*) OVER (PARTITION BY department) AS dept_size
FROM employees;
```

---

## FRAME SPECIFICATIONS

```sql
ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW  -- Start to now (running total)
ROWS BETWEEN CURRENT ROW AND UNBOUNDED FOLLOWING  -- Now to end
ROWS BETWEEN 3 PRECEDING AND CURRENT ROW          -- Last 3 + current (moving avg)
ROWS BETWEEN 1 PRECEDING AND 1 FOLLOWING          -- Previous, current, next
```

---

## 🔥 TOP 5 INTERVIEW PATTERNS

### 1️⃣ Top N Per Group (MOST COMMON)
```sql
-- Find top 3 customers by revenue in each region
WITH ranked AS (
    SELECT 
        customer_id,
        region,
        SUM(revenue) AS total_revenue,
        DENSE_RANK() OVER (
            PARTITION BY region 
            ORDER BY SUM(revenue) DESC
        ) AS rnk
    FROM sales
    GROUP BY customer_id, region
)
SELECT region, customer_id, total_revenue
FROM ranked
WHERE rnk <= 3;
```

### 2️⃣ Running Total
```sql
SELECT 
    date,
    amount,
    SUM(amount) OVER (ORDER BY date) AS running_total
FROM transactions;
```

### 3️⃣ Month-over-Month Change
```sql
SELECT 
    month,
    revenue,
    revenue - LAG(revenue) OVER (ORDER BY month) AS change,
    ROUND((revenue - LAG(revenue) OVER (ORDER BY month)) * 100.0 / 
          LAG(revenue) OVER (ORDER BY month), 2) AS pct_change
FROM monthly_revenue;
```

### 4️⃣ Compare to Group Average
```sql
SELECT 
    employee_name,
    department,
    salary,
    AVG(salary) OVER (PARTITION BY department) AS dept_avg,
    salary - AVG(salary) OVER (PARTITION BY department) AS vs_dept_avg
FROM employees;
```

### 5️⃣ Find First/Last Record Per Group
```sql
-- First order for each customer
SELECT *
FROM (
    SELECT 
        *,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id 
            ORDER BY order_date
        ) AS rn
    FROM orders
) sub
WHERE rn = 1;
```

---

## ⚠️ COMMON MISTAKES TO AVOID

❌ **Forgetting ORDER BY in ranking functions**
```sql
-- WRONG: No ORDER BY
RANK() OVER (PARTITION BY dept)  -- ERROR or undefined behavior

-- RIGHT
RANK() OVER (PARTITION BY dept ORDER BY salary DESC)
```

❌ **Using WHERE instead of subquery for filtering**
```sql
-- WRONG: Can't filter window function result in WHERE
SELECT *, DENSE_RANK() OVER (...) AS rnk
FROM table
WHERE rnk <= 3;  -- ERROR!

-- RIGHT: Use subquery or CTE
SELECT * FROM (
    SELECT *, DENSE_RANK() OVER (...) AS rnk
    FROM table
) sub
WHERE rnk <= 3;
```

❌ **Confusing RANK vs DENSE_RANK**
```sql
-- Scores: 100, 95, 95, 90
RANK():       1, 2, 2, 4  -- Gaps after ties
DENSE_RANK(): 1, 2, 2, 3  -- No gaps
ROW_NUMBER(): 1, 2, 3, 4  -- Always unique
```

---

## MSSQL SPECIFIC NOTES

```sql
-- MSSQL uses TOP instead of LIMIT
SELECT TOP 10 * FROM table;

-- Current date
SELECT GETDATE();

-- Date arithmetic
SELECT DATEADD(day, -30, GETDATE());  -- 30 days ago
SELECT DATEDIFF(day, start_date, end_date);  -- Days between

-- Window functions work the same as PostgreSQL!
```

---

## QUICK SYNTAX REFERENCE

| Function | Purpose | Example |
|----------|---------|---------|
| ROW_NUMBER() | Unique sequential | 1, 2, 3, 4, 5 |
| RANK() | With gaps | 1, 2, 2, 4, 5 |
| DENSE_RANK() | No gaps | 1, 2, 2, 3, 4 |
| NTILE(n) | n buckets | 1, 1, 2, 2, 3, 3 |
| LAG(col, n) | Previous row | prev_month |
| LEAD(col, n) | Next row | next_month |
| SUM() OVER | Running total | cumulative |
| AVG() OVER | Moving average | 3-day avg |
| COUNT() OVER | Window count | group_size |

---

**Master these patterns and you'll ace the SQL portion! 🎯**

