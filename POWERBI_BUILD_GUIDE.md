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

---

## STEP 4: Build the Dashboard Layout (60 minutes)

### 4.1 Set Page Size
1. Click empty space on canvas
2. In **Visualizations** → **Format** (paint roller icon)
3. **Page information** → **Type** = Custom
4. Width: 1920, Height: 1080 (or 1280 x 720)

### 4.2 Add Title Bar
1. **Insert** → **Text Box**
2. Type: "🏭 GLOBAL SUPPLY CHAIN PERFORMANCE DASHBOARD"
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
5. Position: Top left

**Card 2: On-Time Delivery**
1. Add another Card
2. Drag `On-Time Delivery %` measure
3. Format as percentage, 1 decimal
4. Position: Next to Card 1

**Card 3: Quality Rating**
1. Add Card
2. Drag `Average Quality Rating` measure
3. Format: add "%" suffix in formatting
4. Position: Next to Card 2

**Card 4: Order Count**
1. Add Card
2. Drag `Order Count` measure
3. Position: Next to Card 3

### 4.4 Add Spend Trend Chart (Line Chart)

1. Click **Line Chart** visual
2. Configure:
   - **X-axis:** purchase_orders → order_date (set to Month)
   - **Y-axis:** `Total Spend YTD` (it will aggregate by month)
3. Format:
   - Title: "📈 SPEND TREND BY MONTH"
   - Line color: #4A90A4
4. Size: ~400px wide, 250px tall
5. Position: Left side, below KPIs

### 4.5 Add Top Suppliers Bar Chart

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
5. Position: Right of Spend Trend

### 4.6 Add Delivery Performance Trend

1. Click **Line Chart**
2. Configure:
   - **X-axis:** purchase_orders → order_date (Month)
   - **Y-axis:** `On-Time Delivery %`
3. Add constant line:
   - **Analytics** pane → Constant line
   - Value: 95 (target)
   - Style: Dashed
4. Format:
   - Title: "🚚 DELIVERY PERFORMANCE TREND"
5. Position: Below Spend Trend

### 4.7 Add Status Breakdown (Stacked Bar)

1. Click **Stacked Bar Chart**
2. Configure:
   - **Y-axis:** purchase_orders → status
   - **X-axis:** Count of po_id
3. Format:
   - Title: "📋 ORDER STATUS BREAKDOWN"
   - Colors: Green (Delivered), Blue (In Transit), Yellow (Open), Red (Delayed)
4. Position: Bottom of dashboard, full width

### 4.8 Add Spend by Category (Donut or Pie)

1. Click **Donut Chart**
2. Configure:
   - **Legend:** categories → category_name
   - **Values:** SUM of purchase_orders[amount]
3. Format:
   - Title: "💰 SPEND BY CATEGORY"
   - Show legend at bottom
4. Position: Empty space

### 4.9 Add Supplier Table (for scorecard)

1. Click **Table** visual
2. Add columns:
   - supplier_name
   - region
   - Sum of amount
   - quality_rating
   - on_time_rating
3. Format:
   - Title: "📊 SUPPLIER SCORECARD"
   - Conditional formatting on ratings (green = high, red = low)
4. Position: Right side

---

## STEP 5: Add Filters (10 minutes)

### 5.1 Date Slicer
1. Click **Slicer** visual
2. Drag purchase_orders → order_date
3. Format as dropdown or date range
4. Position: Top right corner

### 5.2 Category Slicer (optional)
1. Add another Slicer
2. Drag categories → category_name
3. Set to dropdown
4. Position: Near date slicer

---

## STEP 6: Format and Polish (20 minutes)

### 6.1 Apply Consistent Colors

**Theme colors to use:**
- Primary: #1E3A5F (Navy Blue)
- Secondary: #4A90A4 (Steel Blue)
- Accent: #F4A460 (Sandy Brown)
- Positive: #28A745 (Green)
- Negative: #DC3545 (Red)
- Background: #F8F9FA (Light Gray)

### 6.2 Format Each Visual
For each visual:
1. Add clear title
2. Remove unnecessary borders
3. Ensure consistent font (Segoe UI)
4. Add tooltips where helpful

### 6.3 Add "Last Refreshed" Text
1. **Insert** → **Text Box**
2. Type: "Last Refreshed: [Date] | Data Through: Q4 2024"
3. Position: Under title bar, small font

### 6.4 Align Everything
1. Select multiple visuals
2. **Format** → **Align** → Distribute horizontally/vertically
3. Ensure clean, grid-like layout

---

## STEP 7: Test Interactions (10 minutes)

### 7.1 Test Slicers
- Change date range → all visuals should update
- Select category → all visuals should filter

### 7.2 Test Cross-Filtering
- Click a bar in Top Suppliers → other visuals highlight
- Click a category in donut → table filters

### 7.3 Verify Numbers Make Sense
- Total spend should be ~$42M based on sample data
- On-time % should be ~94%
- Order counts should add up

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

## ✅ FINAL CHECKLIST

Before the interview:

- [ ] Dashboard loads without errors
- [ ] All KPIs display numbers
- [ ] Charts show data (not blank)
- [ ] Slicers filter correctly
- [ ] Colors are consistent
- [ ] Titles are clear
- [ ] Can explain every visual
- [ ] Practiced 5-minute walkthrough
- [ ] File saved and backed up
- [ ] Know how to screen share

---

## 🎤 PRESENTATION TIPS

1. **Start with the "why"**: "This dashboard provides visibility into supply chain performance..."

2. **Walk top-to-bottom**: KPIs first, then details

3. **Point out insights**: "Notice how on-time delivery dipped in March..."

4. **Show interactivity**: Click a supplier to show filtering

5. **End with recommendations**: "Based on this, I'd recommend..."

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

**Can't find a field:**
- Make sure all CSV files loaded
- Check field names match exactly

---

**You've got this! Build it, practice it, present it with confidence! 🎯**

