# 📊 SQL QUERIES FOR SUPPLY CHAIN DASHBOARD

## Demonstrate your SQL skills when discussing the dashboard!

---

## 🎯 KEY QUERIES TO KNOW

These are the SQL queries that would power your dashboard. Be ready to write these on a whiteboard or explain the logic.

---

## 1. Total Spend by Category with Year-over-Year Comparison

```sql
-- This query shows spend by category with YoY change
-- Uses window function for comparison!

WITH current_year AS (
    SELECT 
        c.category_name,
        SUM(po.amount) AS current_spend
    FROM purchase_orders po
    JOIN categories c ON po.category_id = c.category_id
    WHERE YEAR(po.order_date) = 2024
    GROUP BY c.category_name
),
prior_year AS (
    SELECT 
        c.category_name,
        SUM(po.amount) AS prior_spend
    FROM purchase_orders po
    JOIN categories c ON po.category_id = c.category_id
    WHERE YEAR(po.order_date) = 2023
    GROUP BY c.category_name
)
SELECT 
    cy.category_name,
    cy.current_spend,
    COALESCE(py.prior_spend, 0) AS prior_spend,
    cy.current_spend - COALESCE(py.prior_spend, 0) AS spend_change,
    CASE 
        WHEN py.prior_spend > 0 
        THEN ROUND((cy.current_spend - py.prior_spend) * 100.0 / py.prior_spend, 2)
        ELSE NULL 
    END AS pct_change
FROM current_year cy
LEFT JOIN prior_year py ON cy.category_name = py.category_name
ORDER BY cy.current_spend DESC;
```

**Interview talking point:**
> "This query uses CTEs to calculate year-over-year spend comparison. I used a LEFT JOIN to handle categories that didn't exist last year, and COALESCE to handle nulls in the percentage calculation."

---

## 2. Top 10 Suppliers by Spend with Performance Metrics

```sql
-- Combines spend with on-time and quality metrics
-- Uses multiple aggregations in single query

SELECT TOP 10
    s.supplier_name,
    s.region,
    COUNT(po.po_id) AS total_orders,
    SUM(po.amount) AS total_spend,
    ROUND(AVG(CAST(po.on_time_flag AS FLOAT)) * 100, 1) AS on_time_pct,
    s.quality_rating,
    -- Supplier score (weighted average)
    ROUND(
        (AVG(CAST(po.on_time_flag AS FLOAT)) * 0.4 + s.quality_rating/100 * 0.6) * 100, 
        1
    ) AS supplier_score
FROM purchase_orders po
JOIN suppliers s ON po.supplier_id = s.supplier_id
WHERE po.status = 'Delivered'
GROUP BY s.supplier_id, s.supplier_name, s.region, s.quality_rating
ORDER BY total_spend DESC;
```

**Interview talking point:**
> "This query ranks suppliers by spend but also includes performance metrics. The supplier score is a weighted composite of on-time delivery and quality rating - this helps identify suppliers who are spending leaders but may have quality issues."

---

## 3. On-Time Delivery Trend by Month (Window Function!)

```sql
-- Monthly on-time delivery percentage with running average
-- DEMONSTRATES WINDOW FUNCTIONS!

WITH monthly_delivery AS (
    SELECT 
        FORMAT(actual_delivery, 'yyyy-MM') AS delivery_month,
        COUNT(*) AS total_deliveries,
        SUM(CAST(on_time_flag AS INT)) AS on_time_deliveries,
        ROUND(
            SUM(CAST(on_time_flag AS INT)) * 100.0 / COUNT(*), 
            1
        ) AS on_time_pct
    FROM purchase_orders
    WHERE status = 'Delivered' AND actual_delivery IS NOT NULL
    GROUP BY FORMAT(actual_delivery, 'yyyy-MM')
)
SELECT 
    delivery_month,
    total_deliveries,
    on_time_deliveries,
    on_time_pct,
    -- 3-month moving average using window function
    ROUND(
        AVG(on_time_pct) OVER (
            ORDER BY delivery_month 
            ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
        ), 
        1
    ) AS moving_avg_3mo,
    -- Compare to previous month using LAG
    LAG(on_time_pct, 1) OVER (ORDER BY delivery_month) AS prev_month_pct,
    on_time_pct - LAG(on_time_pct, 1) OVER (ORDER BY delivery_month) AS month_change
FROM monthly_delivery
ORDER BY delivery_month;
```

**Interview talking point:**
> "This query demonstrates window functions - I use AVG() OVER with ROWS BETWEEN for a 3-month moving average, and LAG() to compare each month to the previous. This helps identify trends and whether we're improving or declining."

---

## 4. At-Risk Parts (Lead Time Analysis)

```sql
-- Identifies parts where remaining time < lead time
-- Critical for proactive supply chain management

WITH upcoming_needs AS (
    SELECT 
        p.part_id,
        p.part_number,
        p.description,
        p.lead_time_days,
        p.criticality,
        po.expected_delivery,
        DATEDIFF(DAY, GETDATE(), po.expected_delivery) AS days_until_need,
        po.po_id,
        po.status
    FROM purchase_orders po
    JOIN parts p ON po.part_id = p.part_id
    WHERE po.status IN ('Open', 'In Transit')
)
SELECT 
    part_number,
    description,
    lead_time_days,
    days_until_need,
    criticality,
    CASE 
        WHEN days_until_need < lead_time_days * 0.3 THEN 'CRITICAL'
        WHEN days_until_need < lead_time_days * 0.5 THEN 'HIGH'
        WHEN days_until_need < lead_time_days * 0.8 THEN 'MEDIUM'
        ELSE 'LOW'
    END AS risk_level,
    status AS order_status
FROM upcoming_needs
WHERE days_until_need < lead_time_days
ORDER BY 
    CASE 
        WHEN criticality = 'Critical' THEN 1
        WHEN criticality = 'High' THEN 2
        ELSE 3
    END,
    days_until_need ASC;
```

**Interview talking point:**
> "This query identifies at-risk items by comparing remaining time until we need a part versus its lead time. The risk level is calculated using CASE statements with percentage thresholds. This enables proactive intervention before stockouts occur."

---

## 5. Spend by Region with Ranking

```sql
-- Regional spend analysis with dense ranking
-- Shows geographic concentration risk

SELECT 
    s.region,
    COUNT(DISTINCT s.supplier_id) AS supplier_count,
    COUNT(po.po_id) AS order_count,
    SUM(po.amount) AS total_spend,
    ROUND(
        SUM(po.amount) * 100.0 / SUM(SUM(po.amount)) OVER (), 
        1
    ) AS pct_of_total,
    DENSE_RANK() OVER (ORDER BY SUM(po.amount) DESC) AS spend_rank
FROM purchase_orders po
JOIN suppliers s ON po.supplier_id = s.supplier_id
WHERE YEAR(po.order_date) = 2024
GROUP BY s.region
ORDER BY total_spend DESC;
```

**Interview talking point:**
> "This uses DENSE_RANK to rank regions by spend, and a window function to calculate percentage of total spend without a separate subquery. If one region has too high a percentage, it could indicate geographic concentration risk."

---

## 6. Purchase Order Status Summary

```sql
-- Status breakdown with percentage distribution
-- Good for the stacked bar visual

SELECT 
    status,
    COUNT(*) AS order_count,
    SUM(amount) AS total_amount,
    ROUND(COUNT(*) * 100.0 / SUM(COUNT(*)) OVER (), 1) AS pct_of_orders,
    ROUND(SUM(amount) * 100.0 / SUM(SUM(amount)) OVER (), 1) AS pct_of_value
FROM purchase_orders
WHERE YEAR(order_date) = 2024
GROUP BY status
ORDER BY 
    CASE status
        WHEN 'Delivered' THEN 1
        WHEN 'In Transit' THEN 2
        WHEN 'Open' THEN 3
        WHEN 'Delayed' THEN 4
        ELSE 5
    END;
```

---

## 7. Inventory Turns Calculation

```sql
-- Inventory turnover ratio by category
-- Standard supply chain efficiency metric

WITH category_cogs AS (
    SELECT 
        c.category_id,
        c.category_name,
        SUM(po.amount) * 0.7 AS estimated_cogs  -- Assuming 70% of spend is COGS
    FROM purchase_orders po
    JOIN categories c ON po.category_id = c.category_id
    WHERE po.status = 'Delivered'
      AND YEAR(po.order_date) = 2024
    GROUP BY c.category_id, c.category_name
),
avg_inventory AS (
    SELECT 
        p.category_id,
        AVG(i.quantity_on_hand * p.unit_cost) AS avg_inventory_value
    FROM inventory i
    JOIN parts p ON i.part_id = p.part_id
    GROUP BY p.category_id
)
SELECT 
    cogs.category_name,
    cogs.estimated_cogs,
    inv.avg_inventory_value,
    ROUND(cogs.estimated_cogs / NULLIF(inv.avg_inventory_value, 0), 2) AS inventory_turns,
    CASE 
        WHEN cogs.estimated_cogs / NULLIF(inv.avg_inventory_value, 0) >= 12 THEN 'Excellent'
        WHEN cogs.estimated_cogs / NULLIF(inv.avg_inventory_value, 0) >= 8 THEN 'Good'
        WHEN cogs.estimated_cogs / NULLIF(inv.avg_inventory_value, 0) >= 4 THEN 'Fair'
        ELSE 'Needs Improvement'
    END AS performance
FROM category_cogs cogs
LEFT JOIN avg_inventory inv ON cogs.category_id = inv.category_id
ORDER BY inventory_turns DESC;
```

---

## 8. Supplier Scorecard (Composite Metrics)

```sql
-- Comprehensive supplier performance view
-- Could be a separate dashboard page

WITH supplier_metrics AS (
    SELECT 
        s.supplier_id,
        s.supplier_name,
        s.region,
        COUNT(po.po_id) AS total_orders,
        SUM(po.amount) AS total_spend,
        AVG(CAST(po.on_time_flag AS FLOAT)) AS on_time_rate,
        s.quality_rating / 100.0 AS quality_rate,
        AVG(
            DATEDIFF(DAY, po.order_date, 
                     COALESCE(po.actual_delivery, po.expected_delivery))
        ) AS avg_lead_time,
        s.contract_end
    FROM suppliers s
    LEFT JOIN purchase_orders po ON s.supplier_id = po.supplier_id
    WHERE po.status = 'Delivered' OR po.status IS NULL
    GROUP BY s.supplier_id, s.supplier_name, s.region, 
             s.quality_rating, s.contract_end
)
SELECT 
    supplier_name,
    region,
    total_orders,
    total_spend,
    ROUND(on_time_rate * 100, 1) AS on_time_pct,
    ROUND(quality_rate * 100, 1) AS quality_pct,
    avg_lead_time,
    -- Composite score (weighted)
    ROUND(
        (COALESCE(on_time_rate, 0.9) * 0.35 + 
         quality_rate * 0.35 + 
         CASE WHEN avg_lead_time < 30 THEN 0.3 ELSE 0.15 END) * 100,
        1
    ) AS composite_score,
    -- Contract status
    CASE 
        WHEN contract_end < GETDATE() THEN 'EXPIRED'
        WHEN contract_end < DATEADD(MONTH, 3, GETDATE()) THEN 'EXPIRING SOON'
        ELSE 'Active'
    END AS contract_status,
    -- Rank by composite score
    DENSE_RANK() OVER (ORDER BY 
        (COALESCE(on_time_rate, 0.9) * 0.35 + quality_rate * 0.35 + 
         CASE WHEN avg_lead_time < 30 THEN 0.3 ELSE 0.15 END) DESC
    ) AS performance_rank
FROM supplier_metrics
WHERE total_orders > 0
ORDER BY composite_score DESC;
```

---

## 9. Monthly Spend Trend with Running Total

```sql
-- For the spend trend line chart
-- Includes running total using window function

SELECT 
    FORMAT(order_date, 'yyyy-MM') AS order_month,
    COUNT(*) AS order_count,
    SUM(amount) AS monthly_spend,
    SUM(SUM(amount)) OVER (
        ORDER BY FORMAT(order_date, 'yyyy-MM')
        ROWS UNBOUNDED PRECEDING
    ) AS running_total_ytd,
    AVG(SUM(amount)) OVER (
        ORDER BY FORMAT(order_date, 'yyyy-MM')
        ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
    ) AS moving_avg_3mo
FROM purchase_orders
WHERE YEAR(order_date) = 2024
GROUP BY FORMAT(order_date, 'yyyy-MM')
ORDER BY order_month;
```

---

## 10. Duplicate Detection (Data Quality)

```sql
-- Data quality check - find potential duplicate POs
-- Shows data validation skills

SELECT 
    supplier_id,
    part_id,
    order_date,
    amount,
    COUNT(*) AS duplicate_count
FROM purchase_orders
GROUP BY supplier_id, part_id, order_date, amount
HAVING COUNT(*) > 1
ORDER BY duplicate_count DESC;
```

**Interview talking point:**
> "Data quality is critical for accurate analytics. This query identifies potential duplicate purchase orders - same supplier, part, date, and amount. In the ETL process, I'd flag these for review before loading into the analytics layer."

---

## 🎤 HOW TO DISCUSS SQL IN THE INTERVIEW

### When they ask about the dashboard's data:

> "The dashboard is powered by SQL queries that I wrote to extract and transform data from the procurement system. For example, the Top Suppliers view uses a query with multiple JOINs across purchase orders, suppliers, and categories, with aggregations for spend and on-time percentage. I used window functions for calculations like running totals and rankings."

### When asked to write SQL on whiteboard:

**Start by clarifying:**
> "So you want me to find the top 3 suppliers by spend in each region? Let me think about the approach..."

**Talk through your logic:**
> "I'll use DENSE_RANK with PARTITION BY region, ordered by total spend descending. Then filter where rank is 3 or less."

**Write the query:**
```sql
WITH ranked_suppliers AS (
    SELECT 
        s.region,
        s.supplier_name,
        SUM(po.amount) AS total_spend,
        DENSE_RANK() OVER (
            PARTITION BY s.region 
            ORDER BY SUM(po.amount) DESC
        ) AS spend_rank
    FROM purchase_orders po
    JOIN suppliers s ON po.supplier_id = s.supplier_id
    GROUP BY s.region, s.supplier_name
)
SELECT region, supplier_name, total_spend, spend_rank
FROM ranked_suppliers
WHERE spend_rank <= 3
ORDER BY region, spend_rank;
```

---

## ✅ SQL CONCEPTS DEMONSTRATED

- [x] JOINs (INNER, LEFT)
- [x] GROUP BY with multiple aggregations
- [x] CTEs (Common Table Expressions)
- [x] Window functions: RANK, DENSE_RANK, ROW_NUMBER
- [x] Window functions: LAG, running totals
- [x] CASE statements
- [x] Date functions (DATEDIFF, FORMAT, DATEADD)
- [x] NULL handling (COALESCE, NULLIF)
- [x] Percentage calculations
- [x] TOP N queries

---

**Know these queries and you'll crush the SQL portion! 🎯**

