# 🎯 NORTHROP GRUMMAN DATABASE ENGINEER INTERVIEW BATTLE PLAN

## ⚡ MISSION CRITICAL: 72-HOUR SPRINT TO SUCCESS

**TODAY:** Friday, November 29, 2024  
**INTERVIEW:** Monday, December 2, 2024  
**INTERVIEWER:** Analytics Manager 3  
**POSITION:** Engineer Database (Level 2/3) - Global Supply Chain Operations  

---

## 🔥 YOUR SUPERPOWER: SAY THIS IN THE FIRST 5 MINUTES

> "I have an **active DoD Secret clearance** that's currently in-scope, and I'm ready to pursue SAP access immediately."

This eliminates 6-12 months of hiring delay. **MASSIVE competitive advantage.**

---

# 📅 3-DAY HOUR-BY-HOUR BATTLE PLAN

---

## DAY 1: FRIDAY, NOVEMBER 29 (6 hours - evening)

### 18:00-19:30 | SQL JOINs Refresher [VIDEO + PRACTICE] 🔗

**OBJECTIVE:** Go from "rusty" to "confident" on all JOIN types

#### Video Resources (Choose one based on your time):

📹 **Primary: "SQL Joins Explained |¦| Joins in SQL |¦| SQL Tutorial"**
- **Channel:** Socratica
- **Link:** https://www.youtube.com/watch?v=9yeOJ0ZMUYw
- **Duration:** 7 minutes (quick refresher)
- **Why:** Clear Venn diagram explanations

📹 **Extended: "SQL Joins Tutorial for Beginners - Inner Join, Left Join, Right Join, Full Outer Join"**
- **Channel:** Programming with Mosh
- **Link:** https://www.youtube.com/watch?v=Yh4CrPHVBdE
- **Duration:** 13 minutes
- **Why:** More examples, beginner-friendly

**CONCEPTS TO MASTER:**
```
✓ INNER JOIN  - Returns only rows with matches in BOTH tables
✓ LEFT JOIN   - ALL rows from left table + matching from right (NULLs if no match)
✓ RIGHT JOIN  - ALL rows from right table + matching from left
✓ FULL OUTER  - ALL rows from BOTH tables (NULLs where no match)
✓ CROSS JOIN  - Cartesian product (every combination)
✓ SELF JOIN   - Table joined to itself (employee → manager)
```

#### Practice Platform: SQLZoo (FREE, Interactive)
🔨 **Link:** https://sqlzoo.net/wiki/The_JOIN_operation
- Complete ALL exercises (15-20 problems)
- Time yourself: aim for 3-5 minutes per problem

#### Additional Practice: LeetCode SQL
🔨 **Link:** https://leetcode.com/problemset/database/

**MUST COMPLETE TODAY:**
| # | Problem | Difficulty | Concept |
|---|---------|------------|---------|
| 175 | Combine Two Tables | Easy | LEFT JOIN basics |
| 181 | Employees Earning More Than Managers | Easy | SELF JOIN |
| 183 | Customers Who Never Order | Easy | LEFT JOIN + NULL check |
| 184 | Department Highest Salary | Medium | JOIN + subquery |

**✅ CHECKPOINT:** Can you write a 3-table JOIN query from memory?

---

### 19:30-19:45 | BREAK 🧘
- Stretch, hydrate, clear your mind
- Quick snack for energy

---

### 19:45-21:30 | Window Functions DEEP DIVE [CRITICAL] 🪟

**OBJECTIVE:** Go from "zero" to "can write on whiteboard"

#### Video Resources (Watch in order):

📹 **Video 1: "SQL Window Functions Tutorial"**
- **Channel:** techTFQ (Thoufiq Mohammed)
- **Link:** https://www.youtube.com/watch?v=Ww71knvhQ-s
- **Duration:** 47 minutes
- **Why:** THE most comprehensive window functions tutorial on YouTube
- **Blog companion:** https://techtfq.com/blog/sql-window-function-part1-sql-queries-tutorial

📹 **Video 2: "Advanced SQL Tutorial | ROW_NUMBER RANK DENSE_RANK"**
- **Channel:** Alex The Analyst
- **Link:** https://www.youtube.com/watch?v=9UgLXRrmwz0
- **Duration:** 13 minutes
- **Why:** Quick reinforcement of ranking functions

**CONCEPTS TO MASTER:**

```sql
-- THE WINDOW FUNCTION PATTERN (memorize this!)
SELECT 
    column,
    FUNCTION() OVER (
        PARTITION BY grouping_column  -- Optional: divides into groups
        ORDER BY ordering_column      -- Required for ranking functions
    ) AS window_result
FROM table;
```

**KEY FUNCTIONS (with examples):**

```sql
-- 1. ROW_NUMBER() - Assigns sequential numbers (no ties)
ROW_NUMBER() OVER (PARTITION BY region ORDER BY revenue DESC)
-- Result: 1, 2, 3, 4, 5... (unique numbers even with ties)

-- 2. RANK() - Assigns rank WITH gaps for ties
RANK() OVER (PARTITION BY region ORDER BY revenue DESC)
-- Result: 1, 2, 2, 4, 5... (skips 3 if two items tie for 2nd)

-- 3. DENSE_RANK() - Assigns rank WITHOUT gaps
DENSE_RANK() OVER (PARTITION BY region ORDER BY revenue DESC)
-- Result: 1, 2, 2, 3, 4... (no gap after tie)

-- 4. LAG(column, offset) - Access PREVIOUS row's value
LAG(revenue, 1) OVER (ORDER BY date)
-- Use case: Month-over-month comparison

-- 5. LEAD(column, offset) - Access NEXT row's value
LEAD(revenue, 1) OVER (ORDER BY date)
-- Use case: Forward-looking analysis

-- 6. SUM() OVER() - Running totals
SUM(revenue) OVER (ORDER BY date ROWS UNBOUNDED PRECEDING)
-- Use case: Cumulative sales

-- 7. AVG() OVER() - Moving averages
AVG(revenue) OVER (PARTITION BY region ORDER BY date ROWS 3 PRECEDING)
-- Use case: 3-day rolling average
```

#### Practice: Write These ON PAPER (Whiteboard Simulation)

**Problem 1: Top 3 Customers per Region**
```sql
-- YOUR TASK: Find top 3 customers by revenue in each region
-- Tables: sales (customer_id, region, revenue)

WITH ranked AS (
    SELECT 
        customer_id,
        region,
        SUM(revenue) as total_revenue,
        DENSE_RANK() OVER (
            PARTITION BY region 
            ORDER BY SUM(revenue) DESC
        ) as rnk
    FROM sales
    GROUP BY customer_id, region
)
SELECT customer_id, region, total_revenue
FROM ranked
WHERE rnk <= 3
ORDER BY region, rnk;
```

**Problem 2: Running Total**
```sql
-- YOUR TASK: Calculate running total of daily sales
-- Tables: daily_sales (sale_date, amount)

SELECT 
    sale_date,
    amount,
    SUM(amount) OVER (
        ORDER BY sale_date 
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) as running_total
FROM daily_sales
ORDER BY sale_date;
```

**Problem 3: Month-over-Month Change**
```sql
-- YOUR TASK: Calculate revenue change vs previous month
-- Tables: monthly_revenue (month, revenue)

SELECT 
    month,
    revenue,
    LAG(revenue, 1) OVER (ORDER BY month) as prev_month_revenue,
    revenue - LAG(revenue, 1) OVER (ORDER BY month) as month_change,
    ROUND(
        (revenue - LAG(revenue, 1) OVER (ORDER BY month)) * 100.0 / 
        LAG(revenue, 1) OVER (ORDER BY month), 2
    ) as pct_change
FROM monthly_revenue;
```

**✅ CHECKPOINT:** Can you write a DENSE_RANK() PARTITION BY query without looking?

---

### 21:30-22:30 | Dashboard Review Session 1 📊

**OBJECTIVE:** Know your dashboard INSIDE AND OUT

#### With Your Fiancée, Document:

**📋 DASHBOARD DOCUMENTATION SHEET**

```
BUSINESS PROBLEM:
_________________________________________________
_________________________________________________

TARGET STAKEHOLDER:
_________________________________________________

DATA SOURCES:
| Source Name | Type | Refresh Frequency | Key Fields |
|-------------|------|-------------------|------------|
|             |      |                   |            |
|             |      |                   |            |

KPIs DISPLAYED:
| KPI | Formula/Calculation | Why This Matters | Target/Benchmark |
|-----|---------------------|------------------|------------------|
|     |                     |                  |                  |
|     |                     |                  |                  |
|     |                     |                  |                  |

VISUALIZATIONS:
| Chart Type | Data Shown | Why This Chart Type |
|------------|------------|---------------------|
|            |            |                     |
|            |            |                     |

KEY INSIGHTS (3-5):
1. _________________________________________________
2. _________________________________________________
3. _________________________________________________

RECOMMENDATIONS/ACTIONS:
_________________________________________________
```

#### Practice: 3-Minute Dashboard Walkthrough OUT LOUD

**SCRIPT TEMPLATE:**
> "This dashboard addresses [BUSINESS PROBLEM] for [STAKEHOLDER]. 
> The key metrics are [KPI 1], [KPI 2], and [KPI 3] because [BUSINESS VALUE].
> The data comes from [SOURCES] and refreshes [FREQUENCY].
> 
> [Point to first visual] This shows [INSIGHT]. Notice how [TREND/PATTERN].
> This is important because [BUSINESS IMPACT].
> 
> Based on this analysis, I recommend [ACTION]."

**✅ CHECKPOINT:** Can you explain every single element of your dashboard?

---

### 22:30-24:00 | Python ETL Concepts [VIDEO] 🐍

**OBJECTIVE:** Connect your Python skills to ETL terminology

#### Video Resources:

📹 **"ETL Pipeline Explained | ETL Process Overview"**
- **Channel:** GeeksforGeeks
- **Link:** https://www.youtube.com/watch?v=oF_2uDb7DvQ
- **Duration:** 10 minutes
- **Why:** Quick conceptual overview

📹 **"Python for Data Engineering - Build ETL Pipelines"**
- **Link:** Search "Python ETL pipeline tutorial pandas sqlalchemy"
- **Duration:** 30-45 minutes
- **Why:** Hands-on Python implementation

**ETL VOCABULARY CHEAT SHEET:**

```
EXTRACT (E)
├── Reading from databases (SQL queries)
├── API calls (requests library)
├── File ingestion (CSV, JSON, XML)
├── Web scraping (BeautifulSoup)
└── Your experience: "Pulling telemetry from 2,000+ nodes"

TRANSFORM (T)
├── Data cleaning (handling nulls, duplicates)
├── Type conversion (string → datetime)
├── Aggregation (grouping, summarizing)
├── Joining/merging datasets
├── Validation (Pydantic!)
└── Your experience: "Pydantic schema validation in microservices"

LOAD (L)
├── Writing to database (INSERT, UPSERT)
├── Bulk loading (COPY command)
├── Data warehouse staging
├── Incremental vs full loads
└── Your experience: "Aggregated intelligence fed to analytics system"
```

**PYTHON LIBRARIES TO MENTION:**

| Library | Purpose | Your Experience Level |
|---------|---------|----------------------|
| pandas | Data manipulation | ✓ Strong |
| sqlalchemy | Database connections | ✓ Have used |
| pyodbc | MSSQL connections | ○ Can learn |
| psycopg2 | PostgreSQL | ✓ Have used |
| pydantic | Data validation | ✓ Strong (resume!) |
| schedule | Job scheduling | ○ Can learn |

**✅ CHECKPOINT:** Can you describe your AI collective intelligence project as an ETL pipeline?

---

## DAY 2: SATURDAY, NOVEMBER 30 (8 hours)

### 09:00-09:30 | Morning Warmup ☕
- Review yesterday's notes
- Quick 5 JOIN problems (timed: 3 min each)

---

### 09:30-12:00 | Window Functions MASTERY [PRACTICE] 🪟

**OBJECTIVE:** Solve problems until window functions feel natural

#### LeetCode SQL - Window Function Problems

**MUST COMPLETE (in order of difficulty):**

| # | Problem Name | Difficulty | Key Concept | Link |
|---|--------------|------------|-------------|------|
| 176 | Second Highest Salary | Medium | Subquery/DENSE_RANK | leetcode.com/problems/second-highest-salary |
| 178 | Rank Scores | Medium | DENSE_RANK() | leetcode.com/problems/rank-scores |
| 180 | Consecutive Numbers | Medium | LAG()/LEAD() | leetcode.com/problems/consecutive-numbers |
| 185 | Department Top Three Salaries | Hard | DENSE_RANK() + PARTITION BY | leetcode.com/problems/department-top-three-salaries |
| 197 | Rising Temperature | Easy | LAG() for comparison | leetcode.com/problems/rising-temperature |

#### HackerRank SQL Practice
🔨 **Link:** https://www.hackerrank.com/domains/sql

Focus on "Advanced Select" section

#### DataLemur (Real Interview Questions)
🔨 **Link:** https://datalemur.com/
- Filter by "Window Functions"
- Real questions from tech companies

#### StrataScratch (Analytics-focused)
🔨 **Link:** https://www.stratascratch.com/
- Filter by "Window Functions"
- Focus on business analytics scenarios

**WHITEBOARD PRACTICE:**
Write these queries on PAPER without looking:

1. **Find employees with salary above their department average**
```sql
SELECT employee_name, salary, department
FROM (
    SELECT 
        employee_name,
        salary,
        department,
        AVG(salary) OVER (PARTITION BY department) as dept_avg
    FROM employees
) sub
WHERE salary > dept_avg;
```

2. **Find the first order for each customer**
```sql
SELECT customer_id, order_date, order_amount
FROM (
    SELECT 
        customer_id,
        order_date,
        order_amount,
        ROW_NUMBER() OVER (
            PARTITION BY customer_id 
            ORDER BY order_date
        ) as rn
    FROM orders
) sub
WHERE rn = 1;
```

---

### 12:00-12:30 | LUNCH BREAK 🍽️

---

### 12:30-14:30 | Dashboard Presentation MASTERY 📊

**OBJECTIVE:** Present like you've done this 100 times

#### Video Resource:

📹 **"How to Present Data to Executives"**
- **Channel:** Storytelling with Data (Cole Nussbaumer Knaflic)
- **Link:** https://www.youtube.com/watch?v=hB6ZfWVqpM0
- **Duration:** 15 minutes
- **Why:** THE authority on data storytelling

📹 **"How to Present to Senior Leadership"**
- Search: "presenting data to stakeholders tips"
- **Duration:** 10-15 minutes

**DATA STORYTELLING FRAMEWORK:**

```
1. CONTEXT (30 seconds)
   "This dashboard addresses [PROBLEM] for [AUDIENCE]..."

2. FINDINGS (2-3 minutes)
   "The data reveals three key insights..."
   [Walk through each visualization with insight]

3. IMPLICATIONS (30 seconds)
   "This means [BUSINESS IMPACT]..."

4. RECOMMENDATION (30 seconds)
   "Based on this, I recommend [ACTION]..."

5. INVITATION (15 seconds)
   "What questions do you have about the methodology?"
```

**PRACTICE SESSION 1: Full Walkthrough with Fiancée**
- Time yourself: 5-7 minutes
- Get feedback on: clarity, confidence, eye contact, pacing
- Ask: "Did I explain the 'why' behind each visualization?"

**PRACTICE SESSION 2: Q&A Simulation**

Have fiancée ask these questions:
1. "Why did you choose [KPI] over [alternative]?"
2. "What does this spike/dip in the data represent?"
3. "How would you improve this dashboard?"
4. "What data quality issues did you encounter?"
5. "How would this integrate with existing reporting?"
6. "What would you do if this metric suddenly dropped 20%?"

**HANDLING THE "DID SOMEONE HELP YOU?" QUESTION:**

> "I collaborated with my fiancée, who has experience in analytics at P&G. I focused on **[YOUR CONTRIBUTION: data requirements, KPI definitions, insight generation, SQL queries if any]**, and we leveraged her visualization expertise for **[HER CONTRIBUTION: dashboard design, tool knowledge]**. This mirrors real-world analytics work – it's often a team effort combining data engineering and visualization skills. I can walk you through every design decision and the reasoning behind it."

**Why this works:**
- Honest (integrity matters at Northrop)
- Shows collaboration skills
- Demonstrates you understand the content, not just the aesthetics
- Reframes teamwork as a strength

---

### 14:30-14:45 | BREAK 🧘

---

### 14:45-16:45 | SQL Interview Patterns [CRITICAL] 📝

**OBJECTIVE:** Master the most common SQL interview question types

#### Pattern 1: Finding Duplicates

```sql
-- Find duplicate records
SELECT email, COUNT(*) as count
FROM users
GROUP BY email
HAVING COUNT(*) > 1;

-- Delete duplicates (keep one)
WITH duplicates AS (
    SELECT id, 
           ROW_NUMBER() OVER (PARTITION BY email ORDER BY id) as rn
    FROM users
)
DELETE FROM users WHERE id IN (
    SELECT id FROM duplicates WHERE rn > 1
);
```

#### Pattern 2: Second Highest Value

```sql
-- Method 1: Subquery
SELECT MAX(salary) as second_highest
FROM employees
WHERE salary < (SELECT MAX(salary) FROM employees);

-- Method 2: DENSE_RANK (preferred for interviews)
SELECT salary as second_highest
FROM (
    SELECT salary, DENSE_RANK() OVER (ORDER BY salary DESC) as rnk
    FROM employees
) ranked
WHERE rnk = 2;
```

#### Pattern 3: Self-Join (Employee-Manager)

```sql
-- Find employees earning more than their manager
SELECT e.name as employee, e.salary, m.name as manager, m.salary as manager_salary
FROM employees e
JOIN employees m ON e.manager_id = m.id
WHERE e.salary > m.salary;
```

#### Pattern 4: CTEs for Readability

```sql
-- Using CTE instead of nested subqueries
WITH regional_sales AS (
    SELECT region, SUM(revenue) as total_revenue
    FROM sales
    GROUP BY region
),
top_regions AS (
    SELECT region
    FROM regional_sales
    WHERE total_revenue > (SELECT AVG(total_revenue) FROM regional_sales)
)
SELECT s.product, s.region, SUM(s.revenue)
FROM sales s
JOIN top_regions t ON s.region = t.region
GROUP BY s.product, s.region;
```

#### Pattern 5: Date Calculations (MSSQL Specific)

```sql
-- MSSQL uses different syntax!
-- Get records from last 30 days
SELECT * FROM orders
WHERE order_date >= DATEADD(day, -30, GETDATE());

-- Calculate days between dates
SELECT DATEDIFF(day, start_date, end_date) as days_elapsed
FROM projects;

-- Extract year/month
SELECT YEAR(order_date), MONTH(order_date)
FROM orders;
```

**⚠️ MSSQL vs PostgreSQL Differences:**

| Concept | PostgreSQL | MSSQL |
|---------|------------|-------|
| Limit rows | `LIMIT 10` | `TOP 10` |
| Current date | `CURRENT_DATE` | `GETDATE()` |
| String concat | `\|\|` | `+` or `CONCAT()` |
| Date diff | `date1 - date2` | `DATEDIFF(day, date1, date2)` |
| Substring | `SUBSTRING(col, 1, 5)` | Same |
| ILIKE | `ILIKE '%pattern%'` | `LIKE '%pattern%'` (case-insensitive by default) |

---

### 16:45-17:45 | STAR Story Transformation 🌟

**OBJECTIVE:** Reframe your projects with data engineering terminology

#### STAR STORY 1: Data Pipeline at Scale (AI Collective Intelligence)

**Situation:**
> "At Northrop Grumman, I worked on an AI collective intelligence system that required processing telemetry data from over 2,000 distributed nodes in real-time."

**Task:**
> "I needed to build a data pipeline that could handle 1,000+ packets per second while maintaining data quality for decision-making algorithms."

**Action:**
> "I built an ETL pipeline in Python:
> - **EXTRACT:** Pulled telemetry data from nodes via network protocol using asyncio for concurrent processing
> - **TRANSFORM:** Validated packet formats, applied decision tree models, and processed Q-learning algorithms
> - **LOAD:** Aggregated intelligence data and fed it back to the network for coordinated decision-making
> 
> I implemented error handling for network failures and logging for debugging anomalies."

**Result:**
> "The pipeline improved network coordination efficiency by 90% and enabled real-time collective decision-making across thousands of nodes. This experience directly translates to building supply chain analytics pipelines."

---

#### STAR STORY 2: Data Quality Improvement (Pydantic)

**Situation:**
> "Our microservices at Northrop Grumman were experiencing data quality issues—schema mismatches and type errors were causing downstream failures in analytics pipelines."

**Task:**
> "I was tasked with improving data validation to ensure clean data reached our databases and reporting systems."

**Action:**
> "I integrated Pydantic for schema validation in our Python microservices. This enforced strict type checking during the TRANSFORM step:
> - Defined schemas for each data model
> - Added validation rules for acceptable value ranges
> - Set up logging to catch validation errors early
> - Worked with QA to test edge cases"

**Result:**
> "Reduced testing time by 30% and improved code quality by 15%. More importantly, we eliminated a class of data quality bugs reaching production. This made our analytics dashboards more reliable for stakeholders."

---

#### STAR STORY 3: Learning New Technology (Android/Java)

**Situation:**
> "When I joined Northrop Grumman, I was assigned to debug Java-based Android applications, but my background was primarily Python."

**Task:**
> "I needed to ramp up on Java OOP, Android Studio, and the codebase quickly to contribute within weeks."

**Action:**
> "I took a structured approach:
> - Completed a Java refresher focusing on OOP differences from Python
> - Studied the existing codebase—reading code, adding comments to understand architecture
> - Paired with senior engineers to learn debugging workflows
> - Started with small bugs, gradually increasing complexity"

**Result:**
> "Within my first quarter, I reduced QA-reported bugs by 40% and improved API reliability by 25%. This demonstrates my ability to rapidly acquire new skills—I'd apply this same learning agility to Tableau, Power BI, or any tool your team uses."

---

#### STAR STORY 4: Stakeholder Communication (HIL Testing)

**Situation:**
> "I worked on Hardware-in-the-Loop testing for physical radios—highly technical work with complex RF metrics. I needed to present results to program managers who weren't RF engineers."

**Task:**
> "Convey whether radios met reliability standards in a way that enabled decision-making, without overwhelming them with technical jargon."

**Action:**
> "I created visualizations showing latency, jitter, and packet loss over time with clear red/yellow/green thresholds. I translated RF metrics into business terms—instead of 'jitter,' I said 'communication reliability.' I prepared a one-page summary with key findings and recommendations."

**Result:**
> "Program managers understood findings clearly and approved moving to the next phase. They specifically thanked me for making technical details accessible. This improved communication reliability by 30% and ensured we met defense guidelines."

---

### 17:45-18:00 | BREAK 🧘

---

### 18:00-19:00 | Supply Chain Crash Course 📦

**OBJECTIVE:** Don't sound clueless about the domain

#### Video Resource:

📹 **"Supply Chain Management In 6 Minutes"**
- **Channel:** Eye on Tech
- **Link:** https://www.youtube.com/watch?v=Mi1QBxVjZAw
- **Duration:** 6 minutes

📹 **"Supply Chain KPIs and Metrics"**
- Search: "supply chain KPIs explained"
- **Duration:** 10-15 minutes

**SUPPLY CHAIN VOCABULARY:**

```
SUPPLY CHAIN FLOW:
Supplier → Procurement → Manufacturing → Distribution → Customer

KEY METRICS TO KNOW:
┌─────────────────────────────────────────────────────────────┐
│ KPI                    │ What It Measures                   │
├─────────────────────────────────────────────────────────────┤
│ Inventory Turnover     │ How quickly inventory sells/uses   │
│ Fill Rate              │ % of orders fulfilled completely   │
│ On-Time Delivery (OTD) │ % shipments arriving on schedule   │
│ Lead Time              │ Days from order to delivery        │
│ Perfect Order Rate     │ Orders with no errors              │
│ Stockout Rate          │ Frequency of running out of items  │
│ Carrying Cost          │ Cost to hold inventory             │
│ Supplier Quality       │ Defect rate from suppliers         │
└─────────────────────────────────────────────────────────────┘
```

**NORTHROP GRUMMAN CONTEXT:**
- Defense contractor supply chain = complex, highly regulated
- Critical parts for aircraft (F-35, B-21, etc.)
- Supplier quality and security are paramount
- Global operations with compliance requirements
- Long lead times for specialized components

**INTERVIEW ANSWER:**

> "What do you know about supply chain analytics?"

> "I understand supply chain involves managing the flow of materials from suppliers through production to customers. Key analytics track inventory turnover, fill rates, lead times, and supplier performance to optimize cost, speed, and reliability. In defense, there's added complexity around security and compliance. While I haven't worked in supply chain specifically, I'm excited to apply my data engineering skills to this domain—the analytics principles are universal, and I'd enjoy learning the domain-specific nuances."

---

### 19:00-20:00 | Dashboard Rehearsal Session 2 📊

**PRACTICE SESSION 3: Record Yourself**
- Present to camera on your phone
- Watch recording—identify filler words, nervous habits, unclear explanations
- Re-record until confident

**PRACTICE SESSION 4: Whiteboard Sketch**
- Can you sketch the dashboard from memory?
- Can you explain the data pipeline that feeds it?
- Practice: "If asked to add a new metric, how would you approach it?"

---

### 20:00-21:00 | Mock Interview Questions (Technical) 💻

**Practice these OUT LOUD with fiancée or record yourself:**

**Q: "Write a query to find the top 3 customers by revenue in each region."**

*Talk through your approach:*
> "I'd use a window function with DENSE_RANK partitioned by region, ordered by revenue descending. Then filter where rank is 3 or less."

*Write the query:*
```sql
WITH ranked AS (
    SELECT 
        customer_id,
        region,
        SUM(revenue) as total_revenue,
        DENSE_RANK() OVER (
            PARTITION BY region 
            ORDER BY SUM(revenue) DESC
        ) as rnk
    FROM sales
    GROUP BY customer_id, region
)
SELECT region, customer_id, total_revenue
FROM ranked
WHERE rnk <= 3
ORDER BY region, rnk;
```

---

## DAY 3: SUNDAY, DECEMBER 1 (8 hours)

### 09:00-12:00 | SQL Speed Drills & Refinement ⚡

**OBJECTIVE:** Build muscle memory for writing SQL under pressure

#### Morning Drill Routine:

**Round 1 (30 min): JOIN Speed Round**
- 10 JOIN problems on LeetCode/HackerRank
- Time limit: 3 minutes each
- Write on PAPER first, then type

**Round 2 (60 min): Window Function Marathon**
- 15 window function problems
- Mix of ranking, running totals, LAG/LEAD
- Focus on problems you got wrong yesterday

**Round 3 (60 min): Interview Simulation**
- Have someone give you problems verbally
- Write solutions on paper/whiteboard
- Explain your thought process OUT LOUD

**Round 4 (30 min): Weak Spot Practice**
- Review any concepts still fuzzy
- Practice MSSQL-specific syntax

---

### 12:00-12:30 | LUNCH BREAK 🍽️

---

### 12:30-15:00 | Dashboard Final Rehearsal 📊

**THE DRESS REHEARSAL**

**Session 1 (45 min): Full Run-Through**
- Present dashboard to fiancée (or friend)
- Time yourself: 5-7 minutes
- Get honest feedback

**Session 2 (45 min): Tough Q&A**
- Fiancée plays skeptical interviewer
- Answer rapid-fire questions
- Practice recovering when you don't know something

**Session 3 (45 min): Polish & Perfect**
- Incorporate feedback
- One final run-through
- Record for confidence boost

**BODY LANGUAGE CHECKLIST:**
- [ ] Sitting/standing straight, open posture
- [ ] Eye contact with "interviewer"
- [ ] Gestures to emphasize points (not excessive)
- [ ] Speaking pace: slow enough to understand
- [ ] Showing enthusiasm about insights
- [ ] Pausing for effect and before answering

---

### 15:00-15:15 | BREAK 🧘

---

### 15:15-17:15 | Mock Interview: Full Simulation 🎭

**Run a complete mock interview (2 hours):**

**Part 1: SQL Technical (45 min)**
- 3-4 problems, increasing difficulty
- Write on paper/whiteboard
- Explain thought process

**Part 2: Dashboard Presentation (20 min)**
- Full presentation
- Q&A about methodology

**Part 3: Behavioral (30 min)**
- 4-5 STAR questions
- Focus on data engineering framing

**Part 4: Your Questions (10 min)**
- Practice asking your prepared questions

**Debrief (15 min):**
- What went well?
- What needs improvement?
- Action items for tomorrow morning

---

### 17:15-18:15 | STAR Stories Final Review 🌟

**Review and rehearse your 5-7 STAR stories:**

1. **Data Pipeline at Scale** (AI Collective Intelligence)
2. **Data Quality Improvement** (Pydantic)
3. **Learning New Technology** (Android/Java)
4. **Stakeholder Communication** (HIL Testing)
5. **Cross-Functional Collaboration** (Any team project)
6. **Handling Ambiguity** (IoT Pothole Detection)
7. **Automation & Efficiency** (SonarQube/Jenkins)

**For each story, ensure you can articulate:**
- Clear Situation (30 seconds)
- Specific Task (15 seconds)
- Detailed Action with DATA ENGINEERING terms (60-90 seconds)
- Quantified Result (30 seconds)

---

### 18:15-19:15 | Final Concept Review 📚

**CREATE YOUR 1-PAGE CHEAT SHEET:**

```
SQL QUICK REFERENCE:
┌────────────────────────────────────────┐
│ Window Functions                       │
│ ROW_NUMBER() - unique sequential       │
│ RANK() - gaps for ties                 │
│ DENSE_RANK() - no gaps                 │
│ LAG(col, n) - previous row             │
│ LEAD(col, n) - next row                │
│ SUM() OVER(ORDER BY) - running total   │
└────────────────────────────────────────┘

┌────────────────────────────────────────┐
│ JOIN Types                             │
│ INNER - both match                     │
│ LEFT - all left + match right          │
│ RIGHT - all right + match left         │
│ FULL OUTER - all from both             │
│ CROSS - cartesian product              │
└────────────────────────────────────────┘

┌────────────────────────────────────────┐
│ MSSQL Differences                      │
│ TOP n (not LIMIT)                      │
│ GETDATE() (not CURRENT_DATE)           │
│ DATEADD, DATEDIFF functions            │
└────────────────────────────────────────┘

┌────────────────────────────────────────┐
│ ETL = Extract, Transform, Load         │
│ Libraries: pandas, sqlalchemy, pyodbc  │
│ Data quality: validation, dedup, nulls │
└────────────────────────────────────────┘

┌────────────────────────────────────────┐
│ Supply Chain KPIs                      │
│ Inventory Turnover, Fill Rate, OTD     │
│ Lead Time, Perfect Order Rate          │
└────────────────────────────────────────┘
```

---

### 19:15-20:00 | Logistics & Preparation 📋

**PREPARE EVERYTHING FOR TOMORROW:**

- [ ] Professional attire ready (business casual for defense)
- [ ] Resume printed (3 copies)
- [ ] Questions for interviewer printed/memorized
- [ ] Dashboard file ready to share screen
- [ ] Test screen sharing if virtual interview
- [ ] Notepad and pen for taking notes
- [ ] Water bottle
- [ ] Know exactly where you're going / meeting link

**LOCATION PREFERENCE STATEMENT:**

> "I'm particularly interested in the California location due to [personal reason - fiancée's work, family, etc.], though I'm absolutely open to Melbourne as well. I'm flexible and committed to the opportunity regardless of location."

---

### 20:00-21:00 | Light Review & Relaxation 🧘

- Skim through notes (DON'T CRAM)
- Light review of window function syntax
- Visualization: See yourself succeeding
- Early to bed - sleep is crucial!

**CONFIDENCE MANTRAS:**
- "I have an active Secret clearance - that's a massive advantage."
- "I know SQL, I know Python, I have a great dashboard."
- "I've built real data pipelines at Northrop Grumman."
- "I'm a fast learner - my resume proves it."

---

## DAY 4: MONDAY, DECEMBER 2 (Interview Day)

### 08:00-08:30 | Light Warmup ☀️

**DO NOT CRAM - Just gentle review:**
- Skim window function examples (don't solve)
- Look at JOIN Venn diagrams
- Read through 1-2 practice problems

### 08:30-09:00 | Dashboard Mental Review

- Review KPI definitions
- Review key insights
- Visualize presenting confidently

### 09:00-09:30 | Mental Preparation 🧠

**Review your talking points:**
1. Clearance advantage (mention early)
2. 3 strongest STAR stories
3. Location preference (tactful)

**Power poses and deep breathing:**
- Stand tall, arms wide for 2 minutes
- Deep breaths: 4 count in, 4 count hold, 4 count out

### 09:30-10:00 | Final Logistics

- [ ] Professional attire on
- [ ] All materials ready
- [ ] Test screen sharing ONE MORE TIME
- [ ] Join meeting 5 minutes early

---

# 📝 MOCK INTERVIEW QUESTION BANK

## SQL Technical Questions (40% of Interview)

### Question 1: Top N Per Group (VERY LIKELY)
**"Write a query to find the top 3 customers by revenue in each region."**

```sql
WITH ranked_customers AS (
    SELECT 
        region,
        customer_id,
        SUM(revenue) as total_revenue,
        DENSE_RANK() OVER (
            PARTITION BY region 
            ORDER BY SUM(revenue) DESC
        ) as rnk
    FROM sales
    GROUP BY region, customer_id
)
SELECT region, customer_id, total_revenue
FROM ranked_customers
WHERE rnk <= 3
ORDER BY region, rnk;
```

**Explanation (say this out loud):**
> "I use a CTE to first calculate total revenue per customer, grouped by region. Then I apply DENSE_RANK() partitioned by region and ordered by revenue descending. This assigns ranks 1, 2, 3 within each region. Finally, I filter for ranks 1-3. I use DENSE_RANK instead of RANK to handle ties consistently without gaps."

---

### Question 2: JOIN Explanation (VERY LIKELY)
**"Explain the difference between INNER JOIN and LEFT JOIN with an example."**

> "INNER JOIN returns only rows where there's a match in BOTH tables. If a row in table A doesn't have a corresponding row in table B, it's excluded.
>
> LEFT JOIN returns ALL rows from the left table, plus matching rows from the right table. If there's no match, the right table columns are NULL.
>
> **Example:** If I'm joining Customers to Orders with LEFT JOIN, I get all customers even if they've never placed an order—their order columns would be NULL. With INNER JOIN, I'd only get customers who have placed at least one order.
>
> For analytics, LEFT JOIN is useful when you want the full picture—like finding customers who HAVEN'T ordered recently."

---

### Question 3: Running Totals (LIKELY)
**"How would you calculate a running total of sales by date?"**

```sql
SELECT 
    sale_date,
    daily_sales,
    SUM(daily_sales) OVER (
        ORDER BY sale_date 
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) as running_total
FROM daily_sales_summary
ORDER BY sale_date;
```

**Explanation:**
> "I use SUM() as a window function with OVER clause. ORDER BY ensures chronological processing. ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW specifies the frame—sum from the first row up to current. This creates a cumulative total that grows with each date."

---

### Question 4: Finding Duplicates (COMMON)
**"How would you find duplicate records in a table?"**

```sql
-- Find duplicates
SELECT email, COUNT(*) as duplicate_count
FROM users
GROUP BY email
HAVING COUNT(*) > 1;

-- See all duplicate rows
SELECT *
FROM users
WHERE email IN (
    SELECT email FROM users
    GROUP BY email
    HAVING COUNT(*) > 1
);
```

---

### Question 5: WHERE vs HAVING (COMMON)
**"What's the difference between WHERE and HAVING?"**

> "WHERE filters rows BEFORE grouping—it operates on individual rows. HAVING filters AFTER grouping—it operates on aggregated results.
>
> **Example:** If I want to find regions with total sales over $1M, I use HAVING SUM(sales) > 1000000 because I'm filtering on the aggregated sum. If I want to exclude returns before calculating, I use WHERE order_type != 'return'.
>
> A query can have both: WHERE to filter raw data, then GROUP BY, then HAVING to filter aggregated results."

---

### Question 6: Query Optimization (POSSIBLE)
**"How do you optimize a slow query?"**

> "I follow a systematic approach:
> 1. **EXPLAIN ANALYZE** - Check the execution plan to identify bottlenecks
> 2. **Indexes** - Ensure columns in WHERE, JOIN, ORDER BY have appropriate indexes
> 3. **Reduce data early** - Filter with WHERE before JOINs to reduce row count
> 4. **Avoid SELECT \*** - Only select columns actually needed
> 5. **Review JOINs** - Ensure JOIN conditions use indexed columns
> 6. **Subqueries vs JOINs** - Sometimes restructuring is faster
> 7. **Partitioning** - For very large tables, partition by date or region
> 8. **Statistics** - Ensure database statistics are up to date"

---

### Question 7: LAG/LEAD Usage (POSSIBLE)
**"Find products whose sales decreased compared to the previous month."**

```sql
WITH monthly_sales AS (
    SELECT 
        product_id,
        DATE_TRUNC('month', sale_date) as month,
        SUM(amount) as monthly_total
    FROM sales
    GROUP BY product_id, DATE_TRUNC('month', sale_date)
),
with_comparison AS (
    SELECT 
        product_id,
        month,
        monthly_total,
        LAG(monthly_total, 1) OVER (
            PARTITION BY product_id 
            ORDER BY month
        ) as prev_month_total
    FROM monthly_sales
)
SELECT product_id, month, monthly_total, prev_month_total
FROM with_comparison
WHERE monthly_total < prev_month_total;
```

---

### Question 8: Self-Join (POSSIBLE)
**"Find employees who earn more than their manager."**

```sql
SELECT 
    e.name as employee_name,
    e.salary as employee_salary,
    m.name as manager_name,
    m.salary as manager_salary
FROM employees e
JOIN employees m ON e.manager_id = m.employee_id
WHERE e.salary > m.salary;
```

---

### Question 9: CTE vs Subquery (POSSIBLE)
**"When would you use a CTE instead of a subquery?"**

> "CTEs (Common Table Expressions) improve readability, especially for complex queries with multiple levels. They're named and can be referenced multiple times, unlike subqueries which must be repeated.
>
> **Use CTE when:**
> - Query logic is complex and needs to be readable
> - You need to reference the same derived table multiple times
> - Recursive queries are needed
>
> **Use subquery when:**
> - Simple, one-time use
> - Subquery result is small and efficient
>
> Performance is usually similar, so I default to CTEs for maintainability."

---

### Question 10: NULL Handling (POSSIBLE)
**"How do you handle NULL values in calculations?"**

> "NULLs propagate through calculations—any operation with NULL returns NULL. I handle them with:
> - **COALESCE(column, default)** - Replace NULL with default value
> - **NULLIF(a, b)** - Returns NULL if a equals b
> - **ISNULL(column, default)** - MSSQL-specific alternative to COALESCE
> - **IS NULL / IS NOT NULL** in WHERE clauses
> - **Aggregations ignore NULLs** except COUNT(*) - be aware of this
>
> For averages, I consider whether NULLs should be treated as zero or excluded entirely—it depends on business logic."

---

## Dashboard Presentation Questions (30% of Interview)

### Question 11: Dashboard Walkthrough
**"Walk me through this dashboard. What insights does it show?"**

**Use this structure:**
> "[BUSINESS CONTEXT] This dashboard addresses [PROBLEM] for [STAKEHOLDER]. The goal is to provide visibility into [PROCESS] to enable [DECISION/ACTION].
>
> [PANEL BY PANEL] The top panel shows [KPI] over [TIME]. We can see [TREND/INSIGHT]. This matters because [BUSINESS IMPACT].
>
> [DATA & METHOD] The data comes from [SOURCE] and refreshes [FREQUENCY]. I calculated [KPI] using [LOGIC]. I chose [CHART TYPE] because it best shows [COMPARISON/TREND].
>
> [CONCLUSION] Based on this, I recommend [ACTION]. This dashboard empowers [STAKEHOLDER] to make data-driven decisions about [PROCESS]."

---

### Question 12: KPI Selection
**"Why did you choose these specific KPIs?"**

> "I selected these KPIs based on [BUSINESS OBJECTIVE]. For example, [KPI 1] measures [WHAT] which directly impacts [BUSINESS OUTCOME]. I validated these choices by [STAKEHOLDER DISCUSSION / INDUSTRY STANDARDS / BUSINESS REQUIREMENTS]. These metrics are actionable—if [KPI] drops, we know to investigate [SPECIFIC AREA]."

---

### Question 13: Dashboard Improvements
**"How would you improve this dashboard?"**

> "I'd add three enhancements:
> 1. **Drill-down capability** - Click a region to see store-level detail
> 2. **Interactive filters** - Let users customize date ranges and segments
> 3. **Automated alerts** - Email stakeholders when KPIs fall outside acceptable range
>
> This would make the dashboard more proactive rather than just reactive."

---

### Question 14: Data Quality
**"How did you ensure data quality?"**

> "I used a multi-layer approach:
> 1. **Schema validation** - Checked data types and formats
> 2. **Range checks** - Flagged impossible values (negative quantities, future dates)
> 3. **Null handling** - Identified and addressed missing data appropriately
> 4. **Deduplication** - Removed or flagged duplicate records
> 5. **Cross-validation** - Compared totals against known control figures
>
> This is similar to my work with Pydantic at Northrop—catching issues early before they corrupt downstream analytics."

---

### Question 15: Tool Choice
**"What tool did you use and why?"**

> "We used [ACTUAL TOOL]. While I haven't used Tableau or Power BI professionally, I understand data visualization principles—choosing the right chart type for the data, minimizing clutter, focusing on actionable insights. The principles transfer across tools. I'm eager to learn your team's preferred tools and can ramp up quickly, as I've demonstrated with other technologies at Northrop."

---

## Python/ETL Questions (10-15% of Interview)

### Question 16: ETL Pipeline Experience
**"Describe an ETL pipeline you've built."**

> "In my AI collective intelligence project, I built a pipeline handling 1,000 packets per second from 2,000+ nodes.
>
> **EXTRACT:** Python pulled telemetry data via network protocol using asyncio for concurrent processing.
>
> **TRANSFORM:** Validated packet formats, applied decision tree models, and processed Q-learning algorithms. I ensured data quality by rejecting malformed packets and logging anomalies.
>
> **LOAD:** Aggregated intelligence data and fed it back to the network for coordinated decision-making.
>
> I implemented retry logic for network failures, comprehensive logging, and made the pipeline idempotent to handle restarts safely. The system improved coordination efficiency by 90%."

---

### Question 17: Data Quality Handling
**"How do you handle data quality issues in pipelines?"**

> "I use validation at multiple stages:
> 1. **Schema validation** with libraries like Pydantic—catches type errors and missing fields early
> 2. **Business rule checks** during transformation—flag values outside acceptable ranges
> 3. **Null and duplicate handling** - appropriate strategy based on context
> 4. **Logging and monitoring** - track quality metrics over time, alert on anomalies
>
> The key is failing fast—better to catch bad data early than let it corrupt downstream analytics. At Northrop, integrating Pydantic reduced testing time by 30% and eliminated a class of production bugs."

---

### Question 18: Automation
**"How would you automate a daily data refresh?"**

> "I'd set up a scheduled Python ETL script:
> 1. **Scheduler:** Use cron (Linux) or Task Scheduler (Windows), or Python's APScheduler for more control
> 2. **Extract:** SQL query or API call to pull new data
> 3. **Transform:** Process with pandas—clean, validate, aggregate
> 4. **Load:** Write to database using sqlalchemy or pyodbc for MSSQL
> 5. **Error handling:** Try-except blocks, retry logic, and alerting via email/Slack on failure
> 6. **Idempotency:** Design so reruns don't create duplicates (use MERGE/UPSERT or delete-then-insert)
> 7. **Logging:** Track success/failure, row counts, execution time"

---

## Behavioral Questions (20-30% of Interview)

### Question 19: Learning New Technology
**"Tell me about a time you had to learn a new technology quickly."**

**[Use Android/Java Story]**

> **Situation:** When I joined Northrop, I was assigned to debug Java Android apps, but my background was primarily Python.
>
> **Task:** Ramp up on Java OOP, Android Studio, and the codebase to contribute within weeks.
>
> **Action:** Structured learning approach: Java refresher course, studied existing codebase with detailed notes, paired with senior engineers, started with small bugs and increased complexity.
>
> **Result:** Within first quarter, reduced QA bugs by 40% and improved API reliability by 25%. Demonstrates my ability to rapidly acquire new skills—I'd apply this same approach to Tableau, Power BI, or any tool your team uses.

---

### Question 20: Stakeholder Communication
**"Describe a time you had to communicate technical concepts to non-technical stakeholders."**

**[Use HIL Testing Story]**

> **Situation:** Worked on HIL testing for radios with complex RF metrics. Needed to present results to program managers who weren't RF engineers.
>
> **Task:** Convey reliability standards in a way that enabled decision-making without jargon overload.
>
> **Action:** Created visualizations with red/yellow/green thresholds. Translated RF metrics to business terms—'jitter' became 'communication reliability.' Prepared one-page summary with key findings and recommendations. Started presentations with the bottom line, then backed up with data.
>
> **Result:** Managers understood clearly and approved next phase. Specifically thanked me for making technical details accessible. Improved communication reliability by 30%.

---

### Question 21: Data Quality Problem
**"Tell me about a time you solved a data quality issue."**

**[Use Pydantic Story - abbreviated]**

> **Situation:** Schema mismatches causing downstream failures in analytics.
>
> **Task:** Improve data validation in ETL process.
>
> **Action:** Integrated Pydantic for schema validation, defined strict type checking, added validation rules, set up early error logging.
>
> **Result:** Reduced testing time 30%, improved code quality 15%, eliminated a class of production bugs.

---

### Question 22: Handling Pressure
**"Describe a time you delivered under tight deadlines."**

> **Situation:** [Use a relevant project with deadline pressure]
>
> **Task:** [Specific deliverable with timeline]
>
> **Action:** Prioritized ruthlessly—identified critical path items. Communicated proactively with stakeholders about tradeoffs. Focused on MVP first, then enhancements. Asked for help when needed.
>
> **Result:** [Delivered on time, quantified impact]

---

### Question 23: Cross-Functional Collaboration
**"Tell me about working with a cross-functional team."**

> **Situation:** IoT Pothole Detection project required collaboration across hardware (Raspberry Pi), software (CNN model), and cloud (ThingSpeak) disciplines.
>
> **Task:** Build end-to-end system from sensor to visualization.
>
> **Action:** Coordinated with team members handling different components. Defined clear interfaces between systems. Regular syncs to align on integration points. Documentation for handoffs.
>
> **Result:** Achieved 89.82% field accuracy. Demonstrated ability to work across technical domains—similar to analytics role that spans data engineering and business stakeholders.

---

## Questions TO ASK the Interviewer (HAVE 5-7 READY)

1. **"What does the data landscape look like for the supply chain team—what are the main data sources and systems you work with?"**

2. **"What are the biggest data challenges the Global Supply Chain Operations Performance team is facing right now?"**

3. **"How do you balance ad-hoc analysis requests with building long-term data infrastructure?"**

4. **"What does success look like in this role in the first 6 months?"**

5. **"What opportunities are there for growth in data engineering or analytics leadership?"**

6. **"How does the team approach data quality and governance in a regulated defense environment?"**

7. **"What's your experience been transitioning from Secret to SAP clearance for team members?"**

---

# ✅ FINAL PRE-INTERVIEW CHECKLIST

## Sunday Evening (Complete by 9 PM)
- [ ] 10 window function problems solved (timed)
- [ ] 5 JOIN problems solved (timed)
- [ ] Full dashboard presentation rehearsed
- [ ] Q&A simulation completed
- [ ] Cheat sheets reviewed
- [ ] 3 STAR stories practiced out loud
- [ ] Clothes laid out
- [ ] Resume printed (3 copies)
- [ ] Questions for interviewer memorized

## Monday Morning
- [ ] Light SQL review (no cramming)
- [ ] Dashboard silent walkthrough
- [ ] Deep breathing exercises
- [ ] Power pose (2 minutes)
- [ ] Join meeting 5 minutes early

## During Interview
- [ ] Mention clearance in first 5 minutes
- [ ] Express location preference tactfully
- [ ] Talk through SQL approach BEFORE writing
- [ ] Present dashboard with business context first
- [ ] Use STAR format for behavioral questions
- [ ] Ask thoughtful questions
- [ ] Express genuine enthusiasm

## After Interview
- [ ] Send thank-you email within 24 hours
- [ ] Mention specific discussion points
- [ ] Reiterate enthusiasm and fit

---

# 🏆 SUCCESS MANTRAS

**Before entering:**
> "I have an active Secret clearance. I know SQL. I know Python. I have a great dashboard. I've built real data pipelines. I'm a fast learner. I belong here."

**If you get stuck on SQL:**
> "Let me talk through my approach..." [verbalize thought process]

**If asked something you don't know:**
> "I haven't worked with that specifically, but here's how I'd approach learning it..." [show learning agility]

**Closing strong:**
> "I'm genuinely excited about this opportunity. My combination of clearance, Python data engineering experience, and analytics background makes me a strong fit. I'm eager to contribute to the supply chain team's data capabilities."

---

**YOU'VE GOT THIS. 🎯**

The clearance alone puts you ahead of 90% of candidates. Your Python skills are transferable. Your dashboard shows you can deliver. Now go demonstrate your value.

**Good luck on Monday!**

