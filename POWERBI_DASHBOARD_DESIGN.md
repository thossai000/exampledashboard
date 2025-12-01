# 📊 POWER BI DASHBOARD DESIGN: Global Supply Chain Performance

## For Northrop Grumman Database Engineer Interview

---

## 🎯 DASHBOARD CONCEPT

**Title:** "Global Supply Chain Operations Performance Dashboard"

**Business Problem:** Supply chain visibility across procurement, inventory, and supplier performance to enable data-driven decision-making for GSC leadership.

**Target Audience:** Analytics Manager / Supply Chain Leadership

**Why This Will Impress:**
1. Directly addresses "Global Supply Chain Operations Performance" team needs
2. Shows SQL + Python ETL skills through data model
3. Demonstrates data visualization best practices
4. Includes actionable KPIs, not just vanity metrics
5. Shows understanding of defense contractor supply chain context

---

## 📐 DASHBOARD LAYOUT (Single Page)

```
┌──────────────────────────────────────────────────────────────────────────────┐
│  🏭 GLOBAL SUPPLY CHAIN PERFORMANCE DASHBOARD          [Date Filter ▼]       │
│  Last Refreshed: Nov 29, 2024 | Data Through: Q4 2024                        │
├──────────────────────────────────────────────────────────────────────────────┤
│                                                                              │
│  ┌─────────────┐ ┌─────────────┐ ┌─────────────┐ ┌─────────────┐            │
│  │   $42.5M    │ │   94.2%     │ │   97.8%     │ │   12.3      │            │
│  │ Total Spend │ │ On-Time     │ │ Quality     │ │ Inventory   │            │
│  │  YTD        │ │ Delivery    │ │ Rate        │ │ Turns       │            │
│  │  ▲ 8.2%     │ │  ▲ 2.1%     │ │  ▲ 0.5%     │ │  ▲ 1.2      │            │
│  └─────────────┘ └─────────────┘ └─────────────┘ └─────────────┘            │
│                                                                              │
│  ┌────────────────────────────────────┐ ┌────────────────────────────────┐  │
│  │  📈 SPEND TREND BY CATEGORY        │ │  🏆 TOP 10 SUPPLIERS BY SPEND  │  │
│  │                                    │ │                                │  │
│  │    $M                              │ │  Supplier A  ████████████ $8.2M│  │
│  │   15│      ___                     │ │  Supplier B  █████████   $6.1M│  │
│  │   10│   __/   \__                  │ │  Supplier C  ███████     $4.8M│  │
│  │    5│__/         \___              │ │  Supplier D  █████       $3.2M│  │
│  │     └──────────────────            │ │  Supplier E  ████        $2.9M│  │
│  │      Q1  Q2  Q3  Q4                │ │  ...                          │  │
│  │  — Electronics  — Raw Materials    │ │                                │  │
│  │  — Components   — Services         │ │  [Click supplier for details] │  │
│  └────────────────────────────────────┘ └────────────────────────────────┘  │
│                                                                              │
│  ┌────────────────────────────────────┐ ┌────────────────────────────────┐  │
│  │  🚚 DELIVERY PERFORMANCE TREND     │ │  ⚠️ AT-RISK ITEMS              │  │
│  │                                    │ │                                │  │
│  │   %                                │ │  Part #    Lead   Days  Risk   │  │
│  │  100│  ___________                 │ │  ──────────────────────────────│  │
│  │   95│_/           \                │ │  AV-2847   45d    3d   🔴 HIGH │  │
│  │   90│              \___            │ │  EL-9921   30d    7d   🟡 MED  │  │
│  │     └──────────────────            │ │  MC-4455   60d   12d   🟡 MED  │  │
│  │      Jan Feb Mar Apr May Jun Jul   │ │  RF-7733   21d   15d   🟢 LOW  │  │
│  │  — On-Time %  ··· Target (95%)     │ │                                │  │
│  │                                    │ │  Total At-Risk: 23 items       │  │
│  └────────────────────────────────────┘ └────────────────────────────────┘  │
│                                                                              │
│  ┌────────────────────────────────────┐ ┌────────────────────────────────┐  │
│  │  📍 SPEND BY REGION (MAP)          │ │  📊 SUPPLIER SCORECARD         │  │
│  │                                    │ │                                │  │
│  │       [US Map with bubbles]        │ │  Metric        Score  Trend    │  │
│  │                                    │ │  ──────────────────────────────│  │
│  │    ● California: $12.4M            │ │  On-Time       94.2%   ▲       │  │
│  │    ● Texas: $8.7M                  │ │  Quality       97.8%   ▲       │  │
│  │    ● Florida: $6.2M                │ │  Cost Savings   4.2%   ▼       │  │
│  │    ● Other: $15.2M                 │ │  Lead Time     -3.2d   ▲       │  │
│  │                                    │ │  Response       92.1%  →       │  │
│  └────────────────────────────────────┘ └────────────────────────────────┘  │
│                                                                              │
│  ┌───────────────────────────────────────────────────────────────────────┐  │
│  │  📋 PURCHASE ORDER STATUS BREAKDOWN                                   │  │
│  │                                                                       │  │
│  │  [═══════════════════════════════════════════════════════]           │  │
│  │  ████████████████████  ██████████  ████  ██                          │  │
│  │  Delivered (68%)       In Transit  Open  Delayed                     │  │
│  │       1,847               412       156    43                         │  │
│  └───────────────────────────────────────────────────────────────────────┘  │
│                                                                              │
└──────────────────────────────────────────────────────────────────────────────┘
```

---

## 📊 KPIs AND CALCULATIONS

### Header KPIs (Big Numbers)

| KPI | Formula | Target | Why It Matters |
|-----|---------|--------|----------------|
| **Total Spend YTD** | `SUM(PO_Amount) WHERE Year = CurrentYear` | N/A | Overall procurement volume |
| **On-Time Delivery %** | `COUNT(Delivered_OnTime) / COUNT(All_Deliveries) * 100` | ≥95% | Supplier reliability |
| **Quality Rate %** | `1 - (Defective_Units / Total_Units) * 100` | ≥98% | Supplier quality |
| **Inventory Turns** | `COGS / Average_Inventory` | ≥10 | Inventory efficiency |

### Chart Calculations

**Spend Trend by Category:**
```sql
SELECT 
    Category,
    DATE_TRUNC('quarter', order_date) AS Quarter,
    SUM(amount) AS Total_Spend
FROM purchase_orders
GROUP BY Category, Quarter
ORDER BY Quarter
```

**Top 10 Suppliers by Spend:**
```sql
SELECT TOP 10
    supplier_name,
    SUM(amount) AS Total_Spend,
    COUNT(*) AS PO_Count,
    AVG(on_time_flag) * 100 AS OnTime_Pct
FROM purchase_orders po
JOIN suppliers s ON po.supplier_id = s.supplier_id
GROUP BY supplier_name
ORDER BY Total_Spend DESC
```

**At-Risk Items:**
```sql
SELECT 
    part_number,
    lead_time_days,
    DATEDIFF(day, GETDATE(), required_date) AS Days_Until_Need,
    CASE 
        WHEN Days_Until_Need < lead_time_days * 0.5 THEN 'HIGH'
        WHEN Days_Until_Need < lead_time_days * 0.8 THEN 'MEDIUM'
        ELSE 'LOW'
    END AS Risk_Level
FROM inventory_requirements ir
JOIN parts p ON ir.part_id = p.part_id
WHERE Days_Until_Need < lead_time_days
ORDER BY Risk_Level DESC, Days_Until_Need ASC
```

---

## 🗄️ DATA MODEL

### Tables Required:

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   SUPPLIERS     │     │ PURCHASE_ORDERS │     │   CATEGORIES    │
├─────────────────┤     ├─────────────────┤     ├─────────────────┤
│ supplier_id (PK)│◄────│ supplier_id (FK)│     │ category_id (PK)│
│ supplier_name   │     │ po_id (PK)      │     │ category_name   │
│ region          │     │ category_id (FK)│────►│                 │
│ quality_rating  │     │ order_date      │     └─────────────────┘
│ on_time_rating  │     │ delivery_date   │
│ contract_start  │     │ amount          │     ┌─────────────────┐
│ contract_end    │     │ status          │     │     PARTS       │
└─────────────────┘     │ on_time_flag    │     ├─────────────────┤
                        │ part_id (FK)    │────►│ part_id (PK)    │
                        └─────────────────┘     │ part_number     │
                                                │ description     │
┌─────────────────┐     ┌─────────────────┐     │ lead_time_days  │
│   INVENTORY     │     │   DELIVERIES    │     │ unit_cost       │
├─────────────────┤     ├─────────────────┤     └─────────────────┘
│ inventory_id(PK)│     │ delivery_id (PK)│
│ part_id (FK)    │     │ po_id (FK)      │     ┌─────────────────┐
│ quantity_on_hand│     │ scheduled_date  │     │  DATE_TABLE     │
│ reorder_point   │     │ actual_date     │     ├─────────────────┤
│ location        │     │ quantity        │     │ date (PK)       │
│ last_updated    │     │ quality_status  │     │ year            │
└─────────────────┘     │ defect_count    │     │ quarter         │
                        └─────────────────┘     │ month           │
                                                │ week            │
                                                └─────────────────┘
```

### Relationships:
- `Suppliers` 1:N `Purchase_Orders`
- `Categories` 1:N `Purchase_Orders`
- `Parts` 1:N `Purchase_Orders`
- `Purchase_Orders` 1:N `Deliveries`
- `Parts` 1:N `Inventory`
- `Date_Table` 1:N all fact tables (for time intelligence)

---

## 🎨 DESIGN SPECIFICATIONS

### Color Palette (Professional/Defense):

```
PRIMARY:    #1E3A5F (Navy Blue - Northrop Grumman brand adjacent)
SECONDARY:  #4A90A4 (Steel Blue)
ACCENT:     #F4A460 (Sandy Brown - for highlights)
SUCCESS:    #28A745 (Green)
WARNING:    #FFC107 (Amber)
DANGER:     #DC3545 (Red)
BACKGROUND: #F8F9FA (Light Gray)
TEXT:       #212529 (Dark Gray)
```

### Typography:
- **Title:** Segoe UI Bold, 24pt, Navy Blue
- **KPI Numbers:** Segoe UI Light, 36pt, Navy Blue
- **KPI Labels:** Segoe UI, 12pt, Dark Gray
- **Chart Titles:** Segoe UI Semibold, 14pt, Navy Blue
- **Axis Labels:** Segoe UI, 10pt, Dark Gray

### Visual Best Practices:
- ✅ Left-to-right reading flow (most important KPIs at top)
- ✅ Consistent chart heights for visual balance
- ✅ Minimal gridlines (reduce clutter)
- ✅ Data labels on key points only
- ✅ Target/benchmark lines for context
- ✅ Color-coded status indicators (red/yellow/green)
- ✅ Clear "last refreshed" timestamp

---

## 💾 SAMPLE DATA (Copy to Excel/CSV)

### suppliers.csv
```csv
supplier_id,supplier_name,region,quality_rating,on_time_rating,contract_start,contract_end
S001,Aerospace Components Inc,California,98.5,96.2,2022-01-01,2025-12-31
S002,Defense Electronics LLC,Texas,97.8,94.1,2021-06-15,2024-06-14
S003,Precision Parts Corp,Florida,99.1,97.5,2023-03-01,2026-02-28
S004,Advanced Materials Co,California,96.2,92.8,2022-09-01,2025-08-31
S005,Tech Systems Group,Arizona,98.0,95.5,2021-01-01,2024-12-31
S006,Quality Fasteners Inc,Ohio,97.5,93.2,2023-01-15,2026-01-14
S007,Composite Solutions,Washington,99.3,96.8,2022-04-01,2025-03-31
S008,Avionics Plus,Georgia,98.8,94.9,2021-11-01,2024-10-31
S009,Metal Fabrication Pro,Texas,97.1,91.5,2023-06-01,2026-05-31
S010,Electronic Assembly Corp,California,98.6,95.8,2022-02-15,2025-02-14
```

### categories.csv
```csv
category_id,category_name
C001,Electronics
C002,Raw Materials
C003,Mechanical Components
C004,Services
C005,Fasteners & Hardware
C006,Composites
C007,Avionics
C008,Tooling
```

### purchase_orders.csv
```csv
po_id,supplier_id,category_id,order_date,delivery_date,amount,status,on_time_flag,part_id
PO-001,S001,C001,2024-01-15,2024-02-10,125000,Delivered,1,P001
PO-002,S002,C007,2024-01-18,2024-02-15,287000,Delivered,1,P002
PO-003,S003,C003,2024-01-22,2024-02-20,45000,Delivered,0,P003
PO-004,S004,C002,2024-02-01,2024-03-01,198000,Delivered,1,P004
PO-005,S005,C001,2024-02-05,2024-03-05,156000,Delivered,1,P005
PO-006,S001,C007,2024-02-10,2024-03-15,312000,Delivered,1,P006
PO-007,S006,C005,2024-02-15,2024-03-10,23000,Delivered,0,P007
PO-008,S007,C006,2024-02-20,2024-03-25,445000,Delivered,1,P008
PO-009,S008,C007,2024-03-01,2024-04-01,267000,Delivered,1,P009
PO-010,S002,C001,2024-03-05,2024-04-05,189000,Delivered,1,P010
PO-011,S009,C002,2024-03-10,2024-04-15,78000,Delivered,0,P011
PO-012,S010,C001,2024-03-15,2024-04-10,234000,Delivered,1,P012
PO-013,S003,C003,2024-03-20,2024-04-20,67000,In Transit,NULL,P013
PO-014,S001,C007,2024-03-25,2024-04-25,356000,In Transit,NULL,P014
PO-015,S004,C002,2024-04-01,2024-05-01,145000,Open,NULL,P015
PO-016,S005,C001,2024-04-05,2024-05-10,198000,Open,NULL,P016
PO-017,S002,C007,2024-04-10,2024-05-15,423000,Delayed,NULL,P017
PO-018,S006,C005,2024-04-15,2024-05-20,34000,Open,NULL,P018
PO-019,S008,C007,2024-04-20,2024-06-01,512000,In Transit,NULL,P019
PO-020,S001,C001,2024-04-25,2024-05-30,278000,In Transit,NULL,P020
```

### parts.csv
```csv
part_id,part_number,description,lead_time_days,unit_cost,category_id
P001,AV-2847,Avionics Control Module,45,12500,C007
P002,EL-9921,Electronic Sensor Array,30,8700,C001
P003,MC-4455,Mechanical Actuator,60,4500,C003
P004,RF-7733,RF Amplifier Unit,21,15600,C001
P005,CM-1122,Composite Panel,35,23400,C006
P006,FH-3344,Fastener Kit Assembly,14,340,C005
P007,AV-5566,Flight Computer Board,50,45000,C007
P008,RM-7788,Raw Aluminum Stock,10,1200,C002
P009,EL-8899,Power Distribution Unit,28,7800,C001
P010,MC-2233,Hydraulic Valve,42,5600,C003
```

---

## 🔧 DAX MEASURES (For Power BI)

```dax
// Total Spend YTD
Total Spend YTD = 
CALCULATE(
    SUM(purchase_orders[amount]),
    YEAR(purchase_orders[order_date]) = YEAR(TODAY())
)

// On-Time Delivery %
On-Time Delivery % = 
DIVIDE(
    CALCULATE(COUNT(purchase_orders[po_id]), purchase_orders[on_time_flag] = 1),
    CALCULATE(COUNT(purchase_orders[po_id]), purchase_orders[status] = "Delivered"),
    0
) * 100

// YoY Spend Change
YoY Spend Change = 
VAR CurrentYearSpend = [Total Spend YTD]
VAR PriorYearSpend = 
    CALCULATE(
        SUM(purchase_orders[amount]),
        YEAR(purchase_orders[order_date]) = YEAR(TODAY()) - 1
    )
RETURN
    DIVIDE(CurrentYearSpend - PriorYearSpend, PriorYearSpend, 0) * 100

// Inventory Turns (simplified)
Inventory Turns = 
DIVIDE(
    SUM(purchase_orders[amount]) * 0.7,  -- Approximate COGS
    AVERAGE(inventory[quantity_on_hand]) * AVERAGE(parts[unit_cost]),
    0
)

// At-Risk Item Count
At-Risk Items = 
CALCULATE(
    COUNT(parts[part_id]),
    FILTER(
        parts,
        parts[lead_time_days] > 
            DATEDIFF(TODAY(), RELATED(purchase_orders[delivery_date]), DAY)
    )
)

// Supplier Quality Score
Supplier Quality Score = 
AVERAGE(suppliers[quality_rating])

// Spend by Region
Spend by Region = 
SUMMARIZE(
    purchase_orders,
    suppliers[region],
    "Total", SUM(purchase_orders[amount])
)
```

---

## 🎤 PRESENTATION SCRIPT

### Opening (30 seconds):
> "This dashboard provides visibility into Global Supply Chain Operations performance. The goal is to enable data-driven decision-making for procurement, inventory management, and supplier relationships. I designed it with the GSC Operations Performance team in mind."

### Header KPIs (60 seconds):
> "At the top, we have four key performance indicators:
> 
> **Total Spend YTD** of $42.5 million, up 8.2% from last year - indicating increased procurement activity.
> 
> **On-Time Delivery** at 94.2% - we're close to our 95% target, trending upward.
> 
> **Quality Rate** at 97.8% - exceeding our 98% target, showing strong supplier quality.
> 
> **Inventory Turns** at 12.3 - healthy turnover indicating efficient inventory management.
> 
> These metrics give leadership an immediate health check of supply chain operations."

### Spend Analysis (45 seconds):
> "The Spend Trend chart breaks down procurement by category over time. Electronics and Avionics are our largest spend categories, which makes sense for an aerospace organization.
> 
> The Top 10 Suppliers view shows spend concentration - our top supplier represents about 19% of total spend. This is actionable - we might want to evaluate supplier diversification to reduce risk."

### Delivery Performance (45 seconds):
> "The Delivery Performance trend shows on-time percentage over the past 7 months. We had a dip in March but recovered. The dotted line is our 95% target.
> 
> The At-Risk Items table is particularly valuable - it flags parts where lead time exceeds time until we need them. The red indicators require immediate action. This enables proactive rather than reactive supply chain management."

### Supplier Scorecard (30 seconds):
> "The Supplier Scorecard aggregates key metrics across our supplier base. This helps identify systemic issues - for example, if Cost Savings is trending down, we might need to renegotiate contracts or find alternative suppliers."

### Technical Details (if asked):
> "The data comes from our procurement system, refreshed daily. I used SQL to extract and transform the data - calculating derived metrics like on-time percentage and risk scores. The ETL process validates data quality before loading into Power BI."

### Closing/Recommendations:
> "Based on this analysis, I'd recommend:
> 1. Focus on the 23 at-risk items - especially the 3 high-risk parts
> 2. Review the March delivery performance dip to prevent recurrence
> 3. Consider supplier diversification for our top 3 vendors
> 
> The dashboard enables GSC leadership to make these data-driven decisions quickly."

---

## ❓ ANTICIPATED Q&A

**Q: "Why did you choose these specific KPIs?"**
> "I selected KPIs that represent the four pillars of supply chain performance: Cost (Total Spend), Reliability (On-Time Delivery), Quality (Quality Rate), and Efficiency (Inventory Turns). These are industry-standard metrics that give a complete picture of supply chain health."

**Q: "How would this help the supply chain team make decisions?"**
> "Three main ways: First, the At-Risk Items table enables proactive intervention before stockouts occur. Second, the Supplier Scorecard helps identify underperforming suppliers for contract renegotiation or replacement. Third, the trend views help predict future needs and identify seasonal patterns."

**Q: "What data quality issues might you encounter?"**
> "Common issues include missing delivery dates for in-transit orders, inconsistent supplier naming, and duplicate records. I'd address these in the ETL process - validating data formats, standardizing names, and deduplicating before loading. I have experience with Pydantic for schema validation which catches these issues early."

**Q: "How would you improve this dashboard?"**
> "I'd add several features: 
> 1. Drill-through capability to see individual PO details
> 2. Email alerts when KPIs fall below thresholds
> 3. Forecasting based on historical trends
> 4. Integration with contract end dates for proactive renewal
> 5. Benchmark comparisons to industry standards"

**Q: "What tool did you use and why?"**
> "Power BI because it's an industry-standard tool for business intelligence, and I understand Northrop Grumman uses Microsoft technologies. The DAX language enables complex calculations, and the visualization options are robust. While I'm still learning Power BI's advanced features, I understand data visualization principles and can apply them in any tool."

---

## 🛠️ BUILD INSTRUCTIONS FOR FIANCÉE

### Step 1: Create Sample Data
1. Create Excel workbook with sheets for each CSV above
2. Save as `SupplyChainData.xlsx`

### Step 2: Power BI Setup
1. Open Power BI Desktop
2. Get Data → Excel → Select workbook
3. Load all tables

### Step 3: Create Relationships
1. Go to Model view
2. Drag `supplier_id` from `purchase_orders` to `suppliers`
3. Drag `category_id` from `purchase_orders` to `categories`
4. Drag `part_id` from `purchase_orders` to `parts`

### Step 4: Create Measures
1. Create new measures using DAX formulas above
2. Add to appropriate visuals

### Step 5: Build Visuals
1. Follow layout diagram above
2. Use card visuals for header KPIs
3. Line chart for trends
4. Bar chart for top suppliers
5. Table for at-risk items
6. Stacked bar for PO status

### Step 6: Format
1. Apply color palette
2. Add title and last-refreshed timestamp
3. Add date slicer filter
4. Format numbers (currency, percentages)

---

## ✅ DASHBOARD CHECKLIST

Before interview, ensure:

- [ ] Dashboard loads without errors
- [ ] All KPIs show reasonable values
- [ ] Date filter works correctly
- [ ] Color coding is consistent
- [ ] Can explain every visualization
- [ ] Have practiced 5-minute walkthrough
- [ ] Know the DAX/SQL behind each metric
- [ ] Prepared answers for Q&A

---

**This dashboard demonstrates SQL skills, ETL understanding, visualization expertise, and supply chain domain knowledge! 🎯**

