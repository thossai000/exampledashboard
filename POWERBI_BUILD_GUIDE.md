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
2. Type: "🏭 GLOBAL SUPPLY CHAIN PERFORMANCE - EXECUTIVE SUMMARY"
3. Font: Segoe UI Bold, 24pt
4. Position at top, full width
5. Background color: #1E3A5F (Navy Blue)
6. Font color: White

### 4.3 Add KPI Cards (4 across the top)

**Card 1: Total Spend**
1. Click **Card** visual (single number icon)
2. Drag `Total Spend YTD` measure to **Fields**
3. Format:
   - **Callout value** → Display units: Millions
   - **Callout value** → Decimal places: 1
   - Add "YTD" to category label
4. Size: ~200px wide, 100px tall
5. Position: Top left under title

**Card 2: On-Time Delivery**
1. Add another Card
2. Drag `On-Time Delivery %` measure
3. Format as percentage, 1 decimal
4. Position: Next to Card 1

**Card 3: Order Count**
1. Add Card
2. Drag `Order Count` measure
3. Position: Next to Card 2

**Card 4: Delayed Orders** ⚠️
1. Add Card
2. Drag `Delayed Orders` measure
3. Format: Red text color (#DC3545)
4. Position: Next to Card 3
5. **This is key for the data story!**

### 4.4 VISUAL 1: Order Status Breakdown (Donut Chart)

1. Click **Donut Chart**
2. Configure:
   - **Legend:** purchase_orders → status
   - **Values:** Count of po_id
3. Format:
   - Title: "📋 ORDER STATUS BREAKDOWN"
   - Colors: 
     - Delivered = #28A745 (Green)
     - In Transit = #17A2B8 (Blue)
     - Open = #FFC107 (Yellow)
     - Delayed = #DC3545 (Red)
4. Size: ~350px x 300px
5. Position: Left side, below KPIs

### 4.5 VISUAL 2: Spend Trend Chart (Line Chart)

1. Click **Line Chart** visual
2. Configure:
   - **X-axis:** purchase_orders → order_date (set to Month)
   - **Y-axis:** `Total Spend YTD` (it will aggregate by month)
3. Format:
   - Title: "📈 MONTHLY SPEND TREND"
   - Line color: #4A90A4
4. Size: ~450px wide, 280px tall
5. Position: Center, below KPIs

### 4.6 VISUAL 3: Delivery Performance Trend (Line Chart)

1. Click **Line Chart**
2. Configure:
   - **X-axis:** purchase_orders → order_date (Month)
   - **Y-axis:** `On-Time Delivery %`
3. Add constant line:
   - **Analytics** pane → Constant line
   - Value: 95 (target)
   - Style: Dashed Red
   - Label: "Target 95%"
4. Format:
   - Title: "🚚 ON-TIME DELIVERY TREND"
5. Position: Right side, same row as Spend Trend

### 4.7 VISUAL 4: Spend by Category (Donut Chart)

1. Click **Donut Chart**
2. Configure:
   - **Legend:** categories → category_name
   - **Values:** SUM of purchase_orders[amount]
3. Format:
   - Title: "💰 SPEND BY CATEGORY"
   - Show legend at bottom
4. Size: ~350px x 280px
5. Position: Bottom left

### 4.8 Add Date Slicer

1. Click **Slicer** visual
2. Drag purchase_orders → order_date
3. Format as dropdown or date range
4. Position: Top right corner, next to KPIs

### 4.9 Add Navigation Button to Page 2

1. **Insert** → **Buttons** → **Blank**
2. Text: "🔍 VIEW SUPPLIER DETAILS →"
3. Format:
   - Background: #4A90A4
   - Font: White, Bold
4. Action:
   - Turn ON **Action**
   - Type: **Page navigation**
   - Destination: Supplier Deep Dive
5. Position: Bottom right corner

---

## STEP 5: Build PAGE 2 - Supplier Deep Dive (40 minutes)

> **Page Size:** Keep default 16:9 (1280px x 720px) - do not change
> **Max Visualizations:** 4 charts/graphs per page (KPI cards don't count)

### 5.1 Add New Page
- Click the **+** at bottom of screen
- Right-click → Rename → "Supplier Deep Dive"

### 5.2 Add Title Bar
1. **Insert** → **Text Box**
2. Type: "🔍 SUPPLIER PERFORMANCE DEEP DIVE"
3. Same formatting as Page 1 (Navy blue background, white text)

### 5.3 Add KPI Cards (3 across top)

**Card 1: Supplier Count**
1. Add Card with `Supplier Count` measure

**Card 2: Average Quality Rating**
1. Add Card with `Average Quality Rating` measure
2. Add "%" suffix

**Card 3: At Risk Suppliers** ⚠️
1. Add Card with `At Risk Suppliers` measure
2. Format: Red text (#DC3545)
3. **Key story element!**

### 5.4 VISUAL 1: Top 10 Suppliers Bar Chart

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
   - Title: "🏆 TOP 10 SUPPLIERS BY SPEND"
   - Data labels: ON
5. Size: ~450px x 350px
6. Position: Left side, below KPIs

### 5.5 VISUAL 2: Delayed Orders by Supplier (Bar Chart) ⚠️ KEY VISUAL

1. Click **Clustered Bar Chart**
2. Configure:
   - **Y-axis:** suppliers → supplier_name
   - **X-axis:** `Delayed Orders` measure
3. Sort: Descending by Delayed Orders
4. Filter: Top 10 by Delayed Orders (or just suppliers with delays)
5. Format:
   - Title: "⚠️ DELAYED ORDERS BY SUPPLIER"
   - Bar color: #DC3545 (Red)
   - Data labels: ON
6. Size: ~450px x 350px
7. Position: Right side, same row as Top 10

### 5.6 VISUAL 3: Supplier Scorecard Table

1. Click **Table** visual
2. Add columns:
   - supplier_name
   - region
   - quality_rating
   - on_time_rating
   - Sum of amount (from purchase_orders)
   - `Delayed Orders` measure
3. Sort by quality_rating (ascending) to show worst first
4. Add Conditional Formatting:
   - Select quality_rating column
   - Format → Conditional formatting → Background color
   - Rules: <85 = Red, 85-90 = Yellow, >90 = Green
   - Repeat for on_time_rating
5. Format:
   - Title: "📊 SUPPLIER SCORECARD - QUALITY AT A GLANCE"
6. Size: Full width, ~250px tall
7. Position: Bottom of page

### 5.7 Add Supplier Filter (Slicer)

1. Click **Slicer**
2. Drag suppliers → supplier_name
3. Format as Dropdown
4. Position: Top right

### 5.8 Add Navigation Button Back to Page 1

1. **Insert** → **Buttons** → **Blank**
2. Text: "← BACK TO SUMMARY"
3. Same formatting as before
4. Action: Page navigation → Executive Summary
5. Position: Bottom left

---

## STEP 6: Format and Polish (20 minutes)

### 6.1 Apply Consistent Colors

**Theme colors to use:**
- Primary: #1E3A5F (Navy Blue)
- Secondary: #4A90A4 (Steel Blue)
- Accent: #F4A460 (Sandy Brown)
- Positive: #28A745 (Green)
- Warning: #FFC107 (Yellow)
- Negative: #DC3545 (Red)
- Background: #F8F9FA (Light Gray)

### 6.2 Format Each Visual
For each visual:
1. Add clear title
2. Remove unnecessary borders
3. Ensure consistent font (Segoe UI)
4. Add tooltips where helpful

### 6.3 Add "Last Refreshed" Text (Both Pages)
1. **Insert** → **Text Box**
2. Type: "Last Refreshed: Dec 2024 | Data Through: Nov 2024"
3. Position: Under title bar, small font

### 6.4 Align Everything
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
- Click a category in donut → table filters

### 7.3 Test Page Navigation
- Click "VIEW SUPPLIER DETAILS" button → should go to Page 2
- Click "BACK TO SUMMARY" → should return to Page 1

### 7.4 Verify Numbers
- Total spend should be ~$42M
- On-time % should be around 54% (for delivered orders)
- 12 orders should be Delayed
- 4 suppliers should show as "At Risk"

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

> **"We have a supplier quality problem that's causing 12% of orders to be delayed, concentrated in just 4 suppliers."**

### How to Walk Through It:

**PAGE 1 - Set the Stage (1-2 minutes)**

1. "This dashboard monitors our Global Supply Chain performance across $42M in spend."

2. "Looking at order status, 69% are delivered, 16% in transit, but notice we have **12 orders delayed** - that's 12% of our pipeline."

3. "Our on-time delivery has been trending down - you can see we're now **below our 95% target**."

4. "Let me show you what's driving this..." → Click to Page 2

**PAGE 2 - Reveal the Problem (2-3 minutes)**

5. "When we drill into suppliers, we see **4 suppliers are flagged as at-risk** with quality ratings below 88%."

6. "This chart shows exactly where our delays are coming from - **S006, S009, S011, and S004** account for ALL 12 delayed orders."

7. "Look at the scorecard: these same suppliers have the **lowest quality and on-time ratings**:
   - S006 Quality Fasteners: 72% quality, 68% on-time
   - S009 Metal Fabrication Pro: 77% quality, 71% on-time
   - S004 Advanced Materials: 76% quality, 73% on-time"

8. "There's a clear pattern: **low quality ratings correlate directly with delivery delays.**"

---

# 🎯 THE ACTIONABLE RECOMMENDATION

## Deliver This with Confidence

> **"Based on this analysis, I recommend we implement a Supplier Performance Improvement Program for S006, S009, S011, and S004."**

### Specific Actions:

1. **Immediate**: Schedule quality review meetings with the 4 at-risk suppliers within 30 days

2. **Short-term**: Implement monthly scorecards with clear quality targets (minimum 90%)

3. **Mid-term**: Identify backup suppliers for the categories these 4 serve (Fasteners, Raw Materials, Structural Components)

4. **Long-term**: Consider contract modifications to include performance penalties/incentives

### Business Impact Statement:
> "If we can improve these 4 suppliers to our target levels, we could eliminate up to **12% of order delays**, improving our overall on-time delivery from ~54% back above 95%."

---

## ✅ FINAL CHECKLIST

Before the interview:

- [ ] Dashboard loads without errors
- [ ] Page 1: All 4 KPIs display correctly
- [ ] Page 1: Donut shows 4 status categories
- [ ] Page 1: Both line charts show trends
- [ ] Page 2: Supplier bar charts show data
- [ ] Page 2: Scorecard table has conditional colors
- [ ] Navigation buttons work between pages
- [ ] Slicers filter correctly on both pages
- [ ] Practiced the 5-minute walkthrough
- [ ] Memorized the data story and recommendation
- [ ] File saved and backed up
- [ ] Know how to screen share

---

## 🎤 PRESENTATION FLOW (5 Minutes Total)

| Time | Action | Key Points |
|------|--------|------------|
| 0:00-0:30 | Introduce Page 1 | "This monitors $42M in supply chain spend" |
| 0:30-1:30 | Walk through KPIs & Status | "Notice 12 delayed orders - 12% of pipeline" |
| 1:30-2:00 | Show Trend Lines | "On-time delivery is below our 95% target" |
| 2:00-2:30 | Navigate to Page 2 | "Let me show you what's causing this" |
| 2:30-3:30 | Delayed Orders Chart | "4 suppliers cause ALL delays" |
| 3:30-4:00 | Scorecard Table | "Same suppliers have lowest ratings" |
| 4:00-4:30 | The Pattern | "Low quality = delivery delays" |
| 4:30-5:00 | **RECOMMENDATION** | "Implement Supplier Improvement Program" |

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

**Navigation buttons don't work:**
- Ensure Action is turned ON
- Verify destination page name matches exactly

**Can't find a field:**
- Make sure all CSV files loaded
- Check field names match exactly

---

## 📐 FINAL LAYOUT REFERENCE (16:9 @ 1280x720)

### Page 1: Executive Summary (4 Visuals + 4 Cards)
```
┌──────────────────────────────────────────────────────────────────┐
│  🏭 GLOBAL SUPPLY CHAIN - EXECUTIVE SUMMARY        [Date ▼]     │
├──────────────────────────────────────────────────────────────────┤
│  ┌────────┐  ┌────────┐  ┌────────┐  ┌────────┐                  │
│  │$42.5M  │  │ 54.1%  │  │  100   │  │  12⚠️  │    ← 4 KPI CARDS │
│  │ Spend  │  │On-Time │  │Orders  │  │Delayed │                  │
│  └────────┘  └────────┘  └────────┘  └────────┘                  │
├──────────────────────────────────────────────────────────────────┤
│  ┌──────────────┐  ┌──────────────┐  ┌──────────────┐            │
│  │ 1. STATUS    │  │ 2. SPEND     │  │ 3. ON-TIME   │            │
│  │    DONUT     │  │    TREND     │  │    TREND     │            │
│  │   ○ 69%      │  │      📈      │  │  --- Target  │            │
│  │  Delivered   │  │              │  │      📈      │            │
│  └──────────────┘  └──────────────┘  └──────────────┘            │
│  ┌──────────────────────────────┐  ┌───────────────────────────┐ │
│  │ 4. SPEND BY CATEGORY         │  │  [VIEW SUPPLIER DETAILS→] │ │
│  │          DONUT               │  │         (button)          │ │
│  └──────────────────────────────┘  └───────────────────────────┘ │
└──────────────────────────────────────────────────────────────────┘
```

### Page 2: Supplier Deep Dive (3 Visuals + 3 Cards)
```
┌──────────────────────────────────────────────────────────────────┐
│  🔍 SUPPLIER PERFORMANCE DEEP DIVE           [Supplier ▼]       │
├──────────────────────────────────────────────────────────────────┤
│  ┌────────┐  ┌────────┐  ┌────────┐                              │
│  │   15   │  │ 89.7%  │  │  4⚠️   │            ← 3 KPI CARDS     │
│  │Supplier│  │Avg Qual│  │At Risk │                              │
│  └────────┘  └────────┘  └────────┘                              │
├──────────────────────────────────────────────────────────────────┤
│  ┌────────────────────────┐  ┌────────────────────────┐          │
│  │ 1. TOP 10 SUPPLIERS    │  │ 2. ⚠️ DELAYED ORDERS   │          │
│  │    BY SPEND            │  │    BY SUPPLIER         │          │
│  │ S001 ████████████      │  │ S006 ████              │          │
│  │ S007 ██████████        │  │ S009 ████              │          │
│  │ S003 ████████          │  │ S011 ███               │          │
│  └────────────────────────┘  └────────────────────────┘          │
│  ┌────────────────────────────────────────────────────────────┐  │
│  │ 3. 📊 SUPPLIER SCORECARD                                   │  │
│  │ Supplier         │ Region │ Quality │ On-Time │ Delays     │  │
│  │ S006 Quality...  │ Ohio   │  72% 🔴 │  68% 🔴 │   4        │  │
│  │ S009 Metal...    │ Texas  │  77% 🔴 │  71% 🔴 │   4        │  │
│  └────────────────────────────────────────────────────────────┘  │
│  ┌───────────────────┐                                           │
│  │ ← BACK TO SUMMARY │                                           │
│  └───────────────────┘                                           │
└──────────────────────────────────────────────────────────────────┘
```

---

**You've got this! Build it, practice the story, deliver the recommendation with confidence! 🎯**

