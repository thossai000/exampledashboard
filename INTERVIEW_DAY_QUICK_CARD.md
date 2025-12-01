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

## 📍 LOCATION STATEMENT

> "I'm particularly interested in the **California** location due to [personal reason], though I'm absolutely open to **Melbourne** as well. I'm flexible and committed to the opportunity regardless of location."

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

### Running Total:
```sql
SUM(amount) OVER (ORDER BY date)
```

### MSSQL Differences:
- `TOP 10` not `LIMIT 10`
- `GETDATE()` not `CURRENT_DATE`
- `DATEADD(day, -30, GETDATE())`

---

## 📊 DASHBOARD STRUCTURE

1. **Context** (30 sec): Business problem + stakeholder
2. **Data** (30 sec): Sources + refresh frequency
3. **Walkthrough** (2-3 min): Each visual + insight
4. **Conclusion** (30 sec): Key insights + recommendations

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
- I'm a proven fast learner
- I belong here

---

## ⏰ INTERVIEW FLOW

| Section | Estimated Time | Your Prep |
|---------|----------------|-----------|
| SQL Technical | 30-45 min | Window functions, JOINs |
| Dashboard | 15-20 min | Full walkthrough + Q&A |
| Behavioral | 20-30 min | STAR stories |
| Your Questions | 5-10 min | 5 prepared questions |

---

## ✅ LAST-MINUTE CHECKLIST

- [ ] Resume (3 copies)
- [ ] Questions printed
- [ ] Dashboard file ready
- [ ] Screen share tested
- [ ] Water bottle
- [ ] Deep breath
- [ ] Smile 😊

---

## 🏆 CLOSING STRONG

> "I'm genuinely excited about this opportunity. My combination of **clearance**, **Python data engineering experience**, and **analytics background** makes me a strong fit. I'm eager to contribute to the supply chain team's data capabilities."

---

**YOU'VE GOT THIS! 🎯**

