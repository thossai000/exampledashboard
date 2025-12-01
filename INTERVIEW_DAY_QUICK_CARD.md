# 📋 INTERVIEW DAY QUICK REFERENCE CARD

## Print this! Keep it nearby until you walk in.

---

## ⚡ SAY THIS IN FIRST 5 MINUTES

> "I have an **active DoD Secret clearance** that's currently in-scope, and I'm ready to pursue SAP access immediately."

---

## 🎯 THE BIG 3 MESSAGES

1. **I have clearance** → Eliminates 6-12 month hiring delay
2. **I'm a fast learner** → Evidence: Android/Java ramp-up, 40% bug reduction
3. **I've built data pipelines** → AI collective intelligence, Pydantic validation

---

## 📊 DASHBOARD KEY NUMBERS

### Page 1 - Executive Summary:
| KPI | Value | Story |
|-----|-------|-------|
| Total Spend YTD | **$39.7M** | Total procurement volume |
| On-Time Delivery | **84.06%** | Below 95% target - problem! |
| Order Count | **100** | Total orders tracked |
| Delayed Orders | **12** | 12% of pipeline - unacceptable |

### Page 2 - Supplier Performance:
| KPI | Value | Story |
|-----|-------|-------|
| Supplier Count | **14** | Active suppliers |
| Avg Quality Rating | **91.23%** | Overall quality score |
| At Risk Suppliers | **4** | Quality < 88% |

### Status Breakdown:
- Delivered: **69%**
- In Transit: **16%**
- Delayed: **12%**
- Open: **3%**

---

## 📖 THE DATA STORY (30-Second Version)

> "12% of orders are delayed, all caused by 5 suppliers. 4 of these suppliers have quality ratings below 88% - they're flagged as at-risk. **Low quality = delays.** My recommendation: Implement a Supplier Performance Improvement Program for these 4 suppliers."

---

## 🎯 THE PROBLEM SUPPLIERS

| Supplier | Quality | Delays |
|----------|---------|--------|
| Quality Fasteners Inc | 72.3% 🔴 | 3 |
| Metal Fabrication Pro | 76.8% 🔴 | 3 |
| Structural Dynamics Inc | 81.5% 🔴 | 3 |
| Thermal Management Inc | 85.1% 🔴 | - |
| Advanced Materials Co | 88.4% 🟡 | 2 |
| Defense Electronics LLC | 91.2% 🟢 | 1 |

---

## 🔧 SQL QUICK REMINDERS

### Window Function Pattern:
```sql
FUNCTION() OVER (
    PARTITION BY group_col 
    ORDER BY sort_col
)
```

### Top N Per Group:
```sql
WITH ranked AS (
    SELECT *, DENSE_RANK() OVER (
        PARTITION BY region 
        ORDER BY revenue DESC
    ) AS rnk
    FROM sales
)
SELECT * FROM ranked WHERE rnk <= 3;
```

### MSSQL Differences:
- `TOP 10` not `LIMIT 10`
- `GETDATE()` not `CURRENT_DATE`
- `DATEADD(day, -30, GETDATE())`

---

## 📊 DASHBOARD WALKTHROUGH FLOW

| Time | Page | What to Say |
|------|------|-------------|
| 0:00-0:30 | 1 | "Managing $39.7M in supply chain spend" |
| 0:30-1:00 | 1 | "12 delayed orders - 12% of pipeline" |
| 1:00-1:30 | 1 | "On-time below 95% target all year" |
| 1:30-2:00 | → | "Let me show you what's causing this..." |
| 2:00-2:30 | 2 | "4 suppliers flagged as at-risk" |
| 2:30-3:30 | 2 | "5 suppliers cause ALL 12 delays" |
| 3:30-4:00 | 2 | "Red = low quality = delays" |
| 4:00-4:30 | 2 | "Clear pattern: quality correlates with delays" |
| 4:30-5:00 | 2 | **"Recommend: Supplier Improvement Program"** |

---

## 🌟 YOUR STRONGEST STORIES

1. **ETL Pipeline** → AI collective intelligence, 1,000 packets/sec, 90% efficiency gain
2. **Data Quality** → Pydantic validation, 30% testing reduction, eliminated bugs
3. **Learning Agility** → Android/Java ramp-up, 40% bug reduction in first quarter
4. **Stakeholder Comms** → HIL testing, RF metrics → business language

---

## ❓ QUESTIONS TO ASK THEM

1. "What does the **data landscape** look like for the supply chain team?"
2. "What are the **biggest data challenges** the team is facing?"
3. "What does **success look like** in the first 6 months?"
4. "How does the team approach **data quality** in a defense environment?"
5. "What opportunities exist for **growth** in analytics leadership?"

---

## 💬 IF YOU DON'T KNOW SOMETHING

> "I haven't worked with [X] specifically, but here's how I'd approach learning it..."

> "That's outside my direct experience, but based on my work with [similar thing], I would..."

---

## 🧘 CONFIDENCE MANTRAS

- I have clearance (massive advantage)
- I know SQL and Python
- I've built real data pipelines
- My dashboard tells a clear story
- I'm a proven fast learner
- I belong here

---

## ✅ LAST-MINUTE CHECKLIST

- [ ] Resume (3 copies)
- [ ] Questions printed
- [ ] Dashboard file ready (GSC_Performance_Dashboard.pbix)
- [ ] Screen share tested
- [ ] Water bottle
- [ ] Deep breath
- [ ] Smile 😊

---

## 🏆 CLOSING STRONG

> "I'm genuinely excited about this opportunity. My combination of **clearance**, **Python data engineering experience**, and **proven analytics skills** - as demonstrated by this dashboard - makes me a strong fit. I'm eager to contribute to the supply chain team's data capabilities."

---

**YOU'VE GOT THIS! 🎯**

