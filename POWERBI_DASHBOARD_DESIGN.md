# 📊 POWER BI DASHBOARD DESIGN: Global Supply Chain Performance

## For Northrop Grumman Database Engineer Interview

---

## 🎯 DASHBOARD CONCEPT

**Title:** "Global Supply Chain Performance Dashboard"

**Business Problem:** Supply chain visibility across procurement, delivery performance, and supplier quality to enable data-driven decision-making for GSC leadership.

**Target Audience:** Analytics Manager / Supply Chain Leadership

**Why This Will Impress:**
1. Directly addresses "Global Supply Chain Operations Performance" team needs
2. Shows SQL + Python ETL skills through data model
3. Demonstrates data visualization best practices
4. Includes actionable KPIs with a clear data story
5. Shows understanding of defense contractor supply chain context

---

## 📐 TWO-PAGE DASHBOARD LAYOUT

### Page 1: Executive Summary (GLOBAL SUPPLY CHAIN PERFORMANCE)

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
│  │ BREAKDOWN (Donut)   │    │                    --- Target 95%  │   │
│  │    ◐ 69% Delivered  │    │  📈 ~~~~~~~~~~~~~~~~~~~~~~         │   │
│  │    16% In Transit   │    │     Performance below target       │   │
│  │    12% Delayed      │    │  Jan Feb Mar Apr May Jun Jul ...   │   │
│  │    3% Open          │    │                                    │   │
│  └─────────────────────┘    └────────────────────────────────────┘   │
│  ┌─────────────────────┐    ┌────────────────────────────────────┐   │
│  │ SPEND TREND BY      │    │ SPEND BY CATEGORY (Pie)            │   │
│  │ MONTH (Line)        │    │    ◕ Mechanical (32%)              │   │
│  │  📈                 │    │    ◔ Avionics (27%)                │   │
│  │     ~~~~~           │    │    ◔ Electronics (22%)             │   │
│  │ Jan ... Nov         │    │    ◔ Composites, Raw Mat, etc.     │   │
│  └─────────────────────┘    └────────────────────────────────────┘   │
└──────────────────────────────────────────────────────────────────────┘
```

### Page 2: Supplier Deep Dive (SUPPLIER PERFORMANCE)

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
│  │ (Horizontal Bar)          │  │ (Horizontal Bar)              │    │
│  │ Structural Dyn ████ 6.7M  │  │ Metal Fab Pro    ████ 3       │    │
│  │ Aerospace Comp ███ 5.8M   │  │ Quality Fastener ████ 3       │    │
│  │ Metal Fab Pro  ██ 4.1M    │  │ Structural Dyn   ████ 3       │    │
│  │ Precision Part ██ 3.5M    │  │ Advanced Mat     ██ 2         │    │
│  │ ...                       │  │ Defense Elec     █ 1          │    │
│  └───────────────────────────┘  └───────────────────────────────┘    │
│  ┌────────────────────────────────────────────────────────────────┐  │
│  │ SUPPLIER SCORECARD QUALITY AT A GLANCE (Table)                 │  │
│  │ Supplier Name      │ Region │ Avg Quality │ Avg On-Time │Delays│  │
│  │ Quality Fasteners  │ Ohio   │ 🔴 72.30%   │    68.50%   │  3   │  │
│  │ Metal Fabrication  │ Texas  │ 🔴 76.80%   │    71.20%   │  3   │  │
│  │ Structural Dyn     │Florida │ 🔴 81.50%   │    74.80%   │  3   │  │
│  │ Thermal Mgmt       │ Nevada │ 🔴 85.10%   │    78.30%   │  -   │  │
│  │ Advanced Materials │Califor │ 🟡 88.40%   │    79.20%   │  2   │  │
│  │ Defense Electronics│ Texas  │ 🟢 91.20%   │    84.50%   │  1   │  │
│  │ ...more suppliers (green)                                      │  │
│  └────────────────────────────────────────────────────────────────┘  │
└──────────────────────────────────────────────────────────────────────┘
```

---

## 📊 ACTUAL KPIs AND VALUES

### Page 1 - Executive Summary KPIs

| KPI | Value | Target | Status |
|-----|-------|--------|--------|
| **Total Spend YTD** | $39.7M | N/A | On track |
| **On-Time Delivery %** | 84.06% | ≥95% | ⚠️ Below target |
| **Order Count** | 100 | N/A | Informational |
| **Delayed Orders** | 12 | 0 | 🔴 Problem area |

### Page 2 - Supplier Performance KPIs

| KPI | Value | Threshold | Status |
|-----|-------|-----------|--------|
| **Supplier Count** | 14 | N/A | Active suppliers |
| **Average Quality Rating** | 91.23% | ≥95% | Room to improve |
| **At Risk Suppliers** | 4 | 0 | 🔴 Quality < 88% |

### Order Status Breakdown

| Status | Count | Percentage |
|--------|-------|------------|
| Delivered | 69 | 69% |
| In Transit | 16 | 16% |
| Delayed | 12 | 12% |
| Open | 3 | 3% |

---

## 🗄️ DATA MODEL

### Tables Used:

```
┌─────────────────┐     ┌─────────────────┐     ┌─────────────────┐
│   SUPPLIERS     │     │ PURCHASE_ORDERS │     │   CATEGORIES    │
├─────────────────┤     ├─────────────────┤     ├─────────────────┤
│ supplier_id (PK)│◄────│ supplier_id (FK)│     │ category_id (PK)│
│ supplier_name   │     │ po_id (PK)      │     │ category_name   │
│ region          │     │ category_id (FK)│────►│                 │
│ quality_rating  │     │ order_date      │     └─────────────────┘
│ on_time_rating  │     │ expected_deliv  │
│ contract_start  │     │ actual_delivery │     ┌─────────────────┐
│ contract_end    │     │ amount          │     │     PARTS       │
└─────────────────┘     │ quantity        │     ├─────────────────┤
                        │ status          │     │ part_id (PK)    │
                        │ on_time_flag    │     │ part_number     │
                        │ part_id (FK)    │────►│ description     │
                        └─────────────────┘     │ lead_time_days  │
                                                │ unit_cost       │
┌─────────────────┐                             └─────────────────┘
│   INVENTORY     │
├─────────────────┤
│ inventory_id(PK)│
│ part_id (FK)    │
│ quantity_on_hand│
│ reorder_point   │
│ warehouse       │
└─────────────────┘
```

### Relationships:
- `Suppliers` 1:N `Purchase_Orders`
- `Categories` 1:N `Purchase_Orders`
- `Parts` 1:N `Purchase_Orders`
- `Parts` 1:N `Inventory`

---

## 🎨 DESIGN SPECIFICATIONS

### Color Palette:

```
PRIMARY:    #1E3A5F (Navy Blue - Title bars)
SECONDARY:  #4A90A4 (Steel Blue)
SUCCESS:    #28A745 (Green - Good performance, ≥90%)
WARNING:    #FFC107 (Yellow - Borderline, 88-89%)
DANGER:     #DC3545 (Red - At risk, <88%)
BACKGROUND: #FFFFFF (White)
TEXT:       #212529 (Dark Gray)
```

### Conditional Formatting Rules

**Quality Rating Column:**
| Range | Color | Meaning |
|-------|-------|---------|
| < 88% | Red (#DC3545) | At Risk |
| 88% - 89.9% | Yellow (#FFC107) | Borderline |
| ≥ 90% | Green (#28A745) | Acceptable |

**On-Time Rating Column:**
| Range | Color | Meaning |
|-------|-------|---------|
| < 80% | Red (#DC3545) | Poor |
| 80% - 89.9% | Yellow (#FFC107) | Needs Improvement |
| ≥ 90% | Green (#28A745) | Good |

---

## 🔧 DAX MEASURES

```dax
// Total Spend YTD
Total Spend YTD = 
SUM(purchase_orders[amount])

// Order Count
Order Count = 
COUNT(purchase_orders[po_id])

// On-Time Delivery %
On-Time Delivery % = 
VAR DeliveredOnTime = CALCULATE(COUNT(purchase_orders[po_id]), purchase_orders[on_time_flag] = 1)
VAR TotalDelivered = CALCULATE(COUNT(purchase_orders[po_id]), purchase_orders[status] = "Delivered", NOT(ISBLANK(purchase_orders[on_time_flag])))
RETURN DIVIDE(DeliveredOnTime, TotalDelivered, 0) * 100

// Average Quality Rating
Average Quality Rating = 
AVERAGE(suppliers[quality_rating])

// Supplier Count
Supplier Count = 
DISTINCTCOUNT(purchase_orders[supplier_id])

// Delayed Orders
Delayed Orders = 
CALCULATE(COUNT(purchase_orders[po_id]), purchase_orders[status] = "Delayed")

// At Risk Suppliers
At Risk Suppliers = 
CALCULATE(
    DISTINCTCOUNT(suppliers[supplier_id]),
    suppliers[quality_rating] < 88
)
```

---

## 📖 THE DATA STORY

### The Problem (Page 1):
> "12% of our orders are delayed. Our on-time delivery rate is 84%, consistently below our 95% target."

### The Root Cause (Page 2):
> "5 suppliers account for ALL 12 delayed orders. 4 suppliers have quality ratings below 88% - they're flagged as at-risk. The scorecard shows a clear pattern: red quality ratings correlate with delivery delays."

### The Insight:
> "Low quality ratings directly predict delivery delays."

### The Recommendation:
> "Implement a Supplier Performance Improvement Program for the 4 at-risk suppliers: Quality Fasteners, Metal Fabrication Pro, Structural Dynamics, and Thermal Management."

---

## 🎤 5-MINUTE PRESENTATION FLOW

| Time | Page | Key Message |
|------|------|-------------|
| 0:00-1:30 | Page 1 | Set the stage: $39.7M spend, 84% on-time (below target), 12 orders delayed |
| 1:30-2:00 | Transition | "What's causing this? Let me show you..." |
| 2:00-3:30 | Page 2 | Root cause: 5 suppliers cause all delays, 4 are at-risk |
| 3:30-4:30 | Page 2 | The pattern: red quality = red on-time = delays |
| 4:30-5:00 | Page 2 | Recommendation: Supplier Performance Improvement Program |

---

## ✅ DASHBOARD CHECKLIST

Before interview, ensure:

- [x] Dashboard loads without errors
- [x] All KPIs show correct values
- [x] Page 1: Status donut shows 69/16/12/3% breakdown
- [x] Page 1: On-time trend shows performance below 95% target
- [x] Page 2: Delayed orders chart shows 5 suppliers with delays
- [x] Page 2: Scorecard has red/yellow/green conditional formatting
- [x] Date and supplier slicers work correctly
- [x] Can explain every visualization and the story behind it
- [x] Have practiced 5-minute walkthrough
- [x] Know the recommendation by heart

---

**This dashboard demonstrates SQL skills, ETL understanding, visualization expertise, and supply chain domain knowledge! 🎯**

