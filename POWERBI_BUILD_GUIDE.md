# 🛠️ POWER BI BUILD GUIDE: Step-by-Step Instructions

## For Your Fiancée (or yourself) to Build the Dashboard

---

## 📋 PREREQUISITES

- [ ] Power BI Desktop installed (free from Microsoft)
- [ ] CSV files from `dashboard_data/` folder
- [ ] 2-3 hours to build

---

## STEP 1: Load the Data (15 minutes)

### 1.1 Open Power BI Desktop

### 1.2 Import CSV Files
1. Click **Home** → **Get Data** → **Text/CSV**
2. Navigate to `dashboard_data/` folder
3. Import these files in order:
   - `suppliers.csv`
   - `categories.csv`
   - `parts.csv`
   - `purchase_orders.csv`
   - `inventory.csv`
   - `date_table.csv`

4. For each file:
   - Click **Load** (not Transform Data for now)

### 1.3 Verify Data Loaded
- In the right **Fields** pane, you should see all 6 tables
- Click each to verify data looks correct

---

## STEP 2: Create Relationships (10 minutes)

### 2.1 Open Model View
- Click the **Model** icon on the left sidebar (looks like 3 connected boxes)

### 2.2 Create Relationships
Drag and drop to connect:

| From Table | From Column | To Table | To Column |
|------------|-------------|----------|-----------|
| purchase_orders | supplier_id | suppliers | supplier_id |
| purchase_orders | category_id | categories | category_id |
| purchase_orders | part_id | parts | part_id |
| inventory | part_id | parts | part_id |

### 2.3 Verify Relationships
- Lines should connect the tables
- All should be "1 to Many" (1:*)

---

## STEP 3: Create Calculated Measures (20 minutes)

### 3.1 Go to Report View
- Click the **Report** icon on left sidebar

### 3.2 Create a Measures Table (optional but cleaner)
1. **Home** → **Enter Data**
2. Just click **Load** with empty table
3. Rename to "Measures"

### 3.3 Create Each Measure
Right-click the Measures table → **New Measure**

**Copy each formula exactly:**

```
Total Spend YTD = 
SUM(purchase_orders[amount])
```

```
Order Count = 
COUNT(purchase_orders[po_id])
```

```
On-Time Delivery % = 
VAR DeliveredOnTime = CALCULATE(COUNT(purchase_orders[po_id]), purchase_orders[on_time_flag] = 1)
VAR TotalDelivered = CALCULATE(COUNT(purchase_orders[po_id]), purchase_orders[status] = "Delivered", NOT(ISBLANK(purchase_orders[on_time_flag])))
RETURN DIVIDE(DeliveredOnTime, TotalDelivered, 0) * 100
```

```
Average Quality Rating = 
AVERAGE(suppliers[quality_rating])
```

```
Supplier Count = 
DISTINCTCOUNT(purchase_orders[supplier_id])
```

```
Delivered Orders = 
CALCULATE(COUNT(purchase_orders[po_id]), purchase_orders[status] = "Delivered")
```

```
In Transit Orders = 
CALCULATE(COUNT(purchase_orders[po_id]), purchase_orders[status] = "In Transit")
```

```
Open Orders = 
CALCULATE(COUNT(purchase_orders[po_id]), purchase_orders[status] = "Open")
```

```
Delayed Orders = 
CALCULATE(COUNT(purchase_orders[po_id]), purchase_orders[status] = "Delayed")
```

```
Delayed % = 
DIVIDE([Delayed Orders], [Order Count], 0) * 100
```

```
At Risk Suppliers = 
CALCULATE(
    DISTINCTCOUNT(suppliers[supplier_id]),
    suppliers[quality_rating] < 88
)
```

---

## STEP 4: Build PAGE 1 - Executive Summary (40 minutes)

> **Page Size:** Keep default 16:9 (1280px x 720px) - do not change
> **Max Visualizations:** 4 charts/graphs per page (KPI cards don't count)

### 4.1 Rename Page
- Right-click page tab at bottom → **Rename** → "Executive Summary"

### 4.2 Add Title Bar
1. **Insert** → **Text Box**
2. Type: "GLOBAL SUPPLY CHAIN PERFORMANCE"
3. Font: Segoe UI Bold, 24pt
4. Position at top, full width
5. Background color: #1E3A5F (Navy Blue)
6. Font color: White

### 4.3 Add KPI Cards (4 across the top)

**Card 1: Total Spend YTD**
1. Click **Card** visual (single number icon)
2. Drag `Total Spend YTD` measure to **Fields**
3. Format:
   - **Callout value** → Display units: Millions
   - **Callout value** → Decimal places: 1
   - Category label: "Total Spend YTD"
4. Position: Top left under title
5. Expected value: **$39.7M**

**Card 2: On-Time Delivery %**
1. Add another Card
2. Drag `On-Time Delivery %` measure
3. Format as percentage, 2 decimals
4. Category label: "On-Time Delivery %"
5. Position: Next to Card 1
6. Expected value: **84.06%**

**Card 3: Order Count**
1. Add Card
2. Drag `Order Count` measure
3. Category label: "Order Count"
4. Position: Next to Card 2
5. Expected value: **100**

**Card 4: Delayed Orders**
1. Add Card
2. Drag `Delayed Orders` measure
3. Category label: "Delayed Orders"
4. Position: Next to Card 3
5. Expected value: **12**

### 4.4 VISUAL 1: Order Status Breakdown (Donut Chart)

1. Click **Donut Chart**
2. Configure:
   - **Legend:** purchase_orders → status
   - **Values:** Count of po_id
3. Format:
   - Title: "ORDER STATUS BREAKDOWN"
   - Colors: 
     - Delayed = Red
     - Delivered = Green
     - In Transit = Blue
     - Open = Yellow
4. Position: Left side, below KPIs
5. Expected breakdown: 69% Delivered, 16% In Transit, 12% Delayed, 3% Open

### 4.5 VISUAL 2: On-Time Delivery % by Month (Line Chart)

1. Click **Line Chart** visual
2. Configure:
   - **X-axis:** purchase_orders → order_date (set to Month)
   - **Y-axis:** `On-Time Delivery %`
3. Add constant line:
   - **Analytics** pane → Constant line
   - Value: 95 (target)
   - Style: Dashed Green
   - Label: "Target 95%"
4. Format:
   - Title: "ON-TIME DELIVERY % BY MONTH"
5. Position: Right side, below KPIs

### 4.6 VISUAL 3: Spend Trend by Month (Line Chart)

1. Click **Line Chart** visual
2. Configure:
   - **X-axis:** purchase_orders → order_date (set to Month)
   - **Y-axis:** `Total Spend YTD` (aggregated by month)
3. Format:
   - Title: "SPEND TREND BY MONTH"
   - Line color: Cyan/Teal
4. Position: Bottom left

### 4.7 VISUAL 4: Spend by Category (Pie Chart)

1. Click **Pie Chart**
2. Configure:
   - **Legend:** categories → category_name
   - **Values:** SUM of purchase_orders[amount]
3. Format:
   - Title: "SPEND BY CATEGORY"
   - Show data labels with values and percentages
4. Position: Bottom right
5. Categories shown: Mechanical Components (32.47%), Avionics Systems (26.89%), Electronics (21.94%), Composites (10.89%), Raw Materials (6.18%), Fasteners & Hardware (1.63%)

### 4.8 Add Date Slicer

1. Click **Slicer** visual
2. Drag purchase_orders → order_date
3. Format as date range slider
4. Position: Top right corner, next to KPIs

---

## STEP 5: Build PAGE 2 - Supplier Deep Dive (40 minutes)

> **Page Size:** Keep default 16:9 (1280px x 720px) - do not change
> **Max Visualizations:** 3 charts/tables per page (KPI cards don't count)

### 5.1 Add New Page
- Click the **+** at bottom of screen
- Right-click → Rename → "Supplier Deep Dive"

### 5.2 Add Title Bar
1. **Insert** → **Text Box**
2. Type: "SUPPLIER PERFORMANCE"
3. Same formatting as Page 1 (Navy blue background, white text)

### 5.3 Add KPI Cards (3 across top)

**Card 1: Supplier Count**
1. Add Card with `Supplier Count` measure
2. Category label: "Supplier Count"
3. Expected value: **14**

**Card 2: Average Quality Rating**
1. Add Card with `Average Quality Rating` measure
2. Format as percentage, 2 decimals
3. Category label: "Average Quality Rating"
4. Expected value: **91.23%**

**Card 3: At Risk Suppliers**
1. Add Card with `At Risk Suppliers` measure
2. Category label: "At Risk Suppliers"
3. Expected value: **4**

### 5.4 VISUAL 1: Top 10 Suppliers by Spend (Bar Chart)

1. Click **Clustered Bar Chart**
2. Configure:
   - **Y-axis:** suppliers → supplier_name
   - **X-axis:** SUM of purchase_orders[amount]
3. Filter to Top 10:
   - In **Filters** pane → supplier_name
   - Filter type: Top N
   - Show items: Top 10
   - By value: Sum of amount
4. Format:
   - Title: "TOP 10 SUPPLIERS BY SPEND"
   - Data labels: ON (show values in Millions)
   - X-axis title: "Total $ Spend (Millions)"
5. Position: Left side, below KPIs
6. Top suppliers: Structural Dynamics Inc ($6.7M), Aerospace Components Inc ($5.8M), Metal Fabrication Pro ($4.1M), etc.

### 5.5 VISUAL 2: Delayed Orders by Suppliers (Bar Chart) ⚠️ KEY VISUAL

1. Click **Clustered Bar Chart**
2. Configure:
   - **Y-axis:** suppliers → supplier_name
   - **X-axis:** `Delayed Orders` measure
3. Filter to show only suppliers with delays (Delayed Orders > 0)
4. Sort: Descending by Delayed Orders
5. Format:
   - Title: "DELAYED ORDERS BY SUPPLIERS"
   - Bar color: Default blue
   - Data labels: ON
6. Position: Right side, same row as Top 10
7. Expected results:
   - Metal Fabrication Pro: 3 delays
   - Quality Fasteners Inc: 3 delays
   - Structural Dynamics Inc: 3 delays
   - Advanced Materials Co: 2 delays
   - Defense Electronics LLC: 1 delay

### 5.6 VISUAL 3: Supplier Scorecard Table

1. Click **Table** visual
2. Add columns:
   - Supplier Name
   - Region
   - Average Quality Rating
   - Average On-Time Rating
   - Total Purchase Orders (sum of amount)
   - Delayed Orders
3. Sort by Average Quality Rating (ascending) to show worst performers first
4. Add Conditional Formatting on Quality Rating:
   - Format → Cell elements → Background color → fx
   - Rules-based formatting:
     - **< 88%** = Red (#DC3545)
     - **88% - 89.9%** = Yellow (#FFC107)
     - **≥ 90%** = Green (#28A745)
5. Format:
   - Title: "SUPPLIER SCORECARD QUALITY AT A GLANCE"
6. Position: Full width at bottom of page
7. Shows all suppliers sorted by quality rating with color-coded cells

### 5.7 Add Supplier Filter (Slicer)

1. Click **Slicer**
2. Drag suppliers → supplier_name
3. Format as Dropdown with "All" option
4. Position: Top right

---

## STEP 6: Format and Polish (20 minutes)

### 6.1 Apply Consistent Colors

**Theme colors used:**
- Primary: #1E3A5F (Navy Blue) - Title bars
- Positive: #28A745 (Green) - Good performance
- Warning: #FFC107 (Yellow) - Borderline
- Negative: #DC3545 (Red) - At risk/Problems
- Chart colors: Default Power BI palette

### 6.2 Format Each Visual
For each visual:
1. Add clear title (ALL CAPS for consistency)
2. Remove unnecessary borders
3. Ensure consistent font (Segoe UI)
4. Add data labels where helpful

### 6.3 Align Everything
1. Select multiple visuals
2. **Format** → **Align** → Distribute horizontally/vertically
3. Ensure clean, grid-like layout

---

## STEP 7: Test Interactions (10 minutes)

### 7.1 Test Slicers
- Change date range → all visuals should update
- Select supplier → all visuals should filter

### 7.2 Test Cross-Filtering
- Click a bar in Top Suppliers → other visuals highlight
- Click a status in donut → table filters

### 7.3 Verify Numbers Match

**Page 1 Expected Values:**
- Total Spend YTD: **$39.7M**
- On-Time Delivery %: **84.06%**
- Order Count: **100**
- Delayed Orders: **12**
- Status breakdown: 69% Delivered, 16% In Transit, 12% Delayed, 3% Open

**Page 2 Expected Values:**
- Supplier Count: **14**
- Average Quality Rating: **91.23%**
- At Risk Suppliers: **4**
- Total Delayed Orders: **12** (across 5 suppliers)

---

## STEP 8: Save and Export (5 minutes)

### 8.1 Save the File
- **File** → **Save As**
- Name: `GSC_Performance_Dashboard.pbix`
- Save to `ngDatabaseEng/` folder

### 8.2 Test Presentation Mode
- Click **View** → **Full Screen** (or press F11)
- This is how you'll present it!

---

# 📖 THE DATA STORY

## One Clear Narrative to Tell

> **"We have a supplier quality problem that's causing 12% of orders to be delayed, concentrated in 5 suppliers - with 4 flagged as at-risk due to quality ratings below 88%."**

---

### How to Walk Through It:

**PAGE 1 - Set the Stage (1-2 minutes)**

1. "This dashboard monitors our Global Supply Chain performance. We're managing **$39.7M in spend** across **100 purchase orders**."

2. "Looking at order status, 69% are delivered, 16% are in transit, but notice we have **12 orders delayed** - that's 12% of our pipeline shown in red."

3. "Our on-time delivery rate is at **84%**, and when we look at the trend, you can see we've been consistently **below our 95% target** throughout the year."

4. "The question is: what's causing these delays? Let me show you..." → Navigate to Page 2

**PAGE 2 - Reveal the Root Cause (2-3 minutes)**

5. "On this page, we drill into supplier performance. We have **14 active suppliers**, with an average quality rating of **91%**. But notice: **4 suppliers are flagged as at-risk**."

6. "This chart shows exactly where our delays are coming from. **5 suppliers account for ALL 12 delayed orders**:
   - Metal Fabrication Pro: 3 delays
   - Quality Fasteners Inc: 3 delays  
   - Structural Dynamics Inc: 3 delays
   - Advanced Materials Co: 2 delays
   - Defense Electronics LLC: 1 delay"

7. "Now look at the scorecard - I've sorted it by quality rating, lowest to highest. See how the **red cells** match our problem suppliers:
   - Quality Fasteners: **72.3%** quality, **68.5%** on-time
   - Metal Fabrication Pro: **76.8%** quality, **71.2%** on-time
   - Structural Dynamics: **81.5%** quality, **74.8%** on-time
   - Thermal Management: **85.1%** quality, **78.3%** on-time"

8. "There's a clear pattern: **low quality ratings directly correlate with delivery delays**. The suppliers in red are the ones causing our problems."

---

# 🎯 THE ACTIONABLE RECOMMENDATION

## Deliver This with Confidence

> **"Based on this analysis, I recommend we implement a Supplier Performance Improvement Program targeting our 4 at-risk suppliers."**

### Specific Actions:

1. **Immediate (This Week)**: 
   - Flag Quality Fasteners, Metal Fabrication Pro, Structural Dynamics, and Thermal Management for priority review
   - Schedule quality review meetings within 30 days

2. **Short-term (30-60 Days)**: 
   - Implement monthly scorecards with clear targets: minimum 88% quality rating, 85% on-time delivery
   - Establish bi-weekly check-ins with at-risk suppliers

3. **Mid-term (60-90 Days)**: 
   - Identify backup suppliers for Fasteners & Hardware, Raw Materials, and Structural Components categories
   - Begin qualification process for alternative suppliers

4. **Long-term (90+ Days)**: 
   - Modify contracts to include performance-based penalties and incentives
   - Consider supplier consolidation if improvements aren't achieved

### Business Impact Statement:
> "By improving these 4 suppliers to target levels, we could **reduce our delayed orders from 12% to near zero**, bringing our on-time delivery rate from 84% back above our 95% target. This protects our production schedules and strengthens our supply chain reliability."

---

## ✅ FINAL CHECKLIST

Before the interview:

- [x] Dashboard loads without errors
- [x] Page 1: All 4 KPIs display correctly ($39.7M, 84.06%, 100, 12)
- [x] Page 1: Donut shows 4 status categories with correct %
- [x] Page 1: On-Time trend shows performance below 95% target
- [x] Page 1: Spend trend and category breakdown display correctly
- [x] Page 2: All 3 KPIs display (14 suppliers, 91.23%, 4 at-risk)
- [x] Page 2: Top 10 suppliers bar chart shows spend values
- [x] Page 2: Delayed orders chart shows 5 suppliers with delays
- [x] Page 2: Scorecard table has red/yellow/green conditional formatting
- [x] Slicers filter correctly on both pages
- [ ] Practiced the 5-minute walkthrough
- [ ] Memorized the data story and recommendation
- [ ] File saved and backed up
- [ ] Know how to screen share

---

## 🎤 PRESENTATION FLOW (5 Minutes Total)

| Time | Page | Action | Key Points |
|------|------|--------|------------|
| 0:00-0:30 | 1 | Introduce dashboard | "Managing $39.7M in supply chain spend" |
| 0:30-1:00 | 1 | KPIs & Status donut | "12 orders delayed - 12% of pipeline" |
| 1:00-1:30 | 1 | On-Time trend | "Consistently below our 95% target" |
| 1:30-2:00 | 1 | Transition | "What's causing this? Let me show you..." |
| 2:00-2:30 | 2 | KPIs | "4 suppliers flagged as at-risk" |
| 2:30-3:15 | 2 | Delayed Orders chart | "5 suppliers cause ALL 12 delays" |
| 3:15-4:00 | 2 | Scorecard table | "Red = low quality = delays. Clear pattern." |
| 4:00-4:30 | 2 | The insight | "Low quality ratings correlate with delays" |
| 4:30-5:00 | 2 | **RECOMMENDATION** | "Supplier Performance Improvement Program" |

---

## ⚠️ TROUBLESHOOTING

**Visual is blank:**
- Check that the right fields are in the right wells
- Verify data loaded (check Fields pane)

**Numbers look wrong:**
- Check measure formulas for typos
- Verify relationships in Model view

**Slicers don't filter:**
- Check relationships exist
- Ensure slicer field connects to fact table

**Conditional formatting not showing:**
- Ensure you're using Rules-based (not Gradient)
- Check threshold values match your data ranges

**Can't find a field:**
- Make sure all CSV files loaded
- Check field names match exactly

---

## 📐 FINAL LAYOUT REFERENCE (16:9 @ 1280x720)

### Page 1: Executive Summary
```
┌──────────────────────────────────────────────────────────────────────┐
│  GLOBAL SUPPLY CHAIN PERFORMANCE                    [Order Date ▼]  │
├──────────────────────────────────────────────────────────────────────┤
│  ┌─────────┐  ┌─────────┐  ┌─────────┐  ┌─────────┐                  │
│  │ 39.7M   │  │ 84.06%  │  │   100   │  │   12    │  ← 4 KPI CARDS   │
│  │Total    │  │On-Time  │  │ Order   │  │Delayed  │                  │
│  │Spend YTD│  │Delivery%│  │ Count   │  │Orders   │                  │
│  └─────────┘  └─────────┘  └─────────┘  └─────────┘                  │
├──────────────────────────────────────────────────────────────────────┤
│  ┌─────────────────────┐    ┌────────────────────────────────────┐   │
│  │ ORDER STATUS        │    │ ON-TIME DELIVERY % BY MONTH        │   │
│  │ BREAKDOWN           │    │                    --- Target 95%  │   │
│  │    ◐ 69% Delivered  │    │  📈 ~~~~~~~~~~~~~~~~~~~~~~         │   │
│  │    16% In Transit   │    │                                    │   │
│  │    12% Delayed      │    │  Jan Feb Mar Apr May Jun Jul ...   │   │
│  │    3% Open          │    │                                    │   │
│  └─────────────────────┘    └────────────────────────────────────┘   │
│  ┌─────────────────────┐    ┌────────────────────────────────────┐   │
│  │ SPEND TREND BY      │    │ SPEND BY CATEGORY                  │   │
│  │ MONTH               │    │    ◕ Mechanical (32%)              │   │
│  │  📈                 │    │    ◔ Avionics (27%)                │   │
│  │     ~~~~~           │    │    ◔ Electronics (22%)             │   │
│  │ Jan ... Nov         │    │    ◔ Others...                     │   │
│  └─────────────────────┘    └────────────────────────────────────┘   │
└──────────────────────────────────────────────────────────────────────┘
```

### Page 2: Supplier Deep Dive
```
┌──────────────────────────────────────────────────────────────────────┐
│  SUPPLIER PERFORMANCE                          [Supplier Name ▼]    │
├──────────────────────────────────────────────────────────────────────┤
│  ┌─────────┐  ┌─────────┐  ┌─────────┐                               │
│  │   14    │  │ 91.23%  │  │    4    │            ← 3 KPI CARDS      │
│  │Supplier │  │Avg Qual │  │At Risk  │                               │
│  │ Count   │  │ Rating  │  │Suppliers│                               │
│  └─────────┘  └─────────┘  └─────────┘                               │
├──────────────────────────────────────────────────────────────────────┤
│  ┌───────────────────────────┐  ┌───────────────────────────────┐    │
│  │ TOP 10 SUPPLIERS BY SPEND │  │ DELAYED ORDERS BY SUPPLIERS   │    │
│  │                           │  │                               │    │
│  │ Structural Dyn ████ 6.7M  │  │ Metal Fab Pro    ████ 3      │    │
│  │ Aerospace Comp ███ 5.8M   │  │ Quality Fastener ████ 3      │    │
│  │ Metal Fab Pro  ██ 4.1M    │  │ Structural Dyn   ████ 3      │    │
│  │ Precision Part ██ 3.5M    │  │ Advanced Mat     ██ 2        │    │
│  │ ...                       │  │ Defense Elec     █ 1         │    │
│  └───────────────────────────┘  └───────────────────────────────┘    │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ SUPPLIER SCORECARD QUALITY AT A GLANCE                         │  │
│  │ Supplier Name      │ Region │ Avg Quality │ Avg On-Time │Delays│  │
│  │ Quality Fasteners  │ Ohio   │ 🔴 72.30%   │    68.50%   │  3   │  │
│  │ Metal Fabrication  │ Texas  │ 🔴 76.80%   │    71.20%   │  3   │  │
│  │ Structural Dyn     │Florida │ 🔴 81.50%   │    74.80%   │  3   │  │
│  │ Thermal Mgmt       │ Nevada │ 🔴 85.10%   │    78.30%   │  -   │  │
│  │ Advanced Materials │Califor │ 🟡 88.40%   │    79.20%   │  2   │  │
│  │ Defense Electronics│ Texas  │ 🟢 91.20%   │    84.50%   │  1   │  │
│  │ ...more suppliers (all green)                                  │  │
│  └────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────┘
```

---

## 🔑 KEY TALKING POINTS TO MEMORIZE

### The Problem Statement:
> "12% of our orders are delayed, driven by suppliers with quality ratings below 88%."

### The Pattern:
> "Low quality ratings correlate directly with delivery delays."

### The Numbers That Matter:
- **$39.7M** total spend
- **84%** on-time (below 95% target)
- **12 orders** delayed (12%)
- **4 suppliers** at-risk (quality < 88%)
- **5 suppliers** account for ALL delays

### The Recommendation:
> "Implement a Supplier Performance Improvement Program for our 4 at-risk suppliers."

---

**You've got this! Your dashboard looks great - now deliver the story with confidence! 🎯**

