# Delivery Operations & SLA Diagnostic — Excel

An end-to-end **Operations Analytics case study built entirely in Excel**. The project starts from raw order-level data, validates KPI logic, diagnoses where delivery delay accumulates, separates **service severity** from **business exposure**, and translates findings into testable operational actions.

> **Analytical story:** What happened → Why → So what → Now what  
> **Tool:** Microsoft Excel  
> **Dataset:** 49,996 orders | May 2022–December 2023  
> **Use cases:** Operations Analytics, Supply Chain, Logistics, E-commerce Operations

📊 [Download the Excel analysis](Delivery%20Operations%20%26%20SLA%20Diagnostic%20%E2%80%94%20Excel%20Project.xlsx)  
📘 [KPI dictionary](docs/kpi-dictionary.md)  
🧠 [Analysis methodology](docs/analysis-methodology.md)  
🎯 [Decision framework](docs/decision-framework.md)

---

## Executive Story

### 1. What happened?

The order lifecycle looks reasonably healthy at first glance:

- **49,996** total orders
- **40,077** completed orders
- **80.16%** completion rate
- **6.84%** cancellation rate

But lifecycle completion hides a much larger service-reliability problem.

Among **40,077 SLA-eligible delivered orders**:

- **20,265** were delivered on or before the promised date
- **19,812** were delivered late
- **On-Time Delivery Rate:** **50.57%**
- **Late Delivery Rate:** **49.43%**
- **Avg End-to-End Delivery Time:** **112.6 hours**
- **Avg Late Duration:** **57.3 hours**

> Nearly **1 in 2 completed deliveries missed the promised date**.

This shifts the management question from *“Do orders eventually complete?”* to:

> **“Why are promised dates being missed, and where should Operations intervene first?”**

---

## 2. Why are orders late?

The analysis compares the same process stages for **on-time vs late deliveries**.

| Stage | On-Time Avg | Late Avg | Gap |
| --- | ---: | ---: | ---: |
| Confirmation | 6.4 h | 6.7 h | +0.3 h |
| Pickup | 11.9 h | 13.1 h | +1.3 h |
| Transit | 22.9 h | 27.3 h | +4.4 h |
| **Final mile** | **41.6 h** | **96.0 h** | **+54.4 h** |
| **Total delivery** | **82.8 h** | **143.1 h** | **+60.4 h** |

Late orders take approximately **60.4 hours longer** than on-time orders.

Of that gap, **54.4 hours — about 90% — occurs in final mile**.

### Peak-period diagnostic

Four clear volume-spike months were identified using:

```text
Peak month = monthly orders > 1.5 × median monthly volume
```

Peak months:

- November 2022
- December 2022
- January 2023
- November 2023

During these periods:

- end-to-end delivery time increases by about **14.6 hours**
- final-mile time increases by about **14.7 hours**

The deterioration is therefore concentrated almost entirely **after carrier handoff**.

> This is strong diagnostic evidence for prioritizing final-mile investigation, but it does **not** prove a specific causal mechanism such as insufficient carrier capacity.

---

## 3. So what? Severity is not the same as business impact

A region with the worst SLA percentage is not necessarily the region creating the most late deliveries.

### Severity hotspots

| Region | Late Rate | Late Orders | Avg Delivery |
| --- | ---: | ---: | ---: |
| Northern Midlands & Mountains | **82.2%** | 1,966 | 171.7 h |
| Central Highlands | **81.8%** | 1,498 | 170.7 h |
| North Central Coast | 51.0% | 532 | 113.7 h |

These regions show severe service-performance problems.

### Exposure hotspots

| Region | Late Rate | Late Orders | Share of All Late Orders |
| --- | ---: | ---: | ---: |
| Southeast | 43.9% | **6,140** | **31.0%** |
| Red River Delta | 43.9% | **4,817** | **24.3%** |

Together, Southeast and Red River Delta generate **55.3% of all late deliveries**, despite having late rates below the network average.

### Management implication

Operations should manage two different workstreams:

**Severity reduction**
- diagnose structurally difficult geographies

**Absolute late-volume reduction**
- improve high-volume markets where modest rate gains can remove many late orders

---

## 4. Product-category prioritization

Category results show the same distinction.

### Highest severity

| Category | Late Rate | Late Orders |
| --- | ---: | ---: |
| Sports | **73.0%** | 910 |
| Automotive | **72.7%** | 576 |
| Garden | **71.9%** | 546 |

### Highest exposure

| Category | Late Orders | Share of All Late Orders |
| --- | ---: | ---: |
| Fashion | **4,602** | **23.2%** |
| Home | **4,264** | **21.5%** |
| Beauty | **3,563** | **18.0%** |

Fashion + Home + Beauty account for **62.7% of all late deliveries**.

**Home** is particularly important because it combines:

- **60.8% late rate**
- **4,264 late deliveries**
- **21.5% of all late orders**

This makes Home the strongest **Severity × Exposure** category signal.

The analysis does **not** claim product weight, size, or handling complexity as the cause because those drivers are not observed in the current dataset.

---

## 5. Why raw shipper rankings can mislead

Carrier performance is benchmarked **within region**, rather than ranking shippers globally.

Example:

- Shipper 37 in Central Highlands: **83.6% late**
- Regional baseline: **81.8% late**

The headline percentage looks poor, but the shipper is only **+1.9 percentage points** above the region.

That suggests the issue may be largely **regional/systemic**, rather than specific to one carrier.

By contrast:

| Shipper | Region | Late Rate | Regional Baseline | Gap |
| --- | --- | ---: | ---: | ---: |
| **164** | Southeast | 57.4% | 43.9% | **+13.5 pp** |
| **41** | Southeast | 54.1% | 43.9% | **+10.2 pp** |

These are more defensible candidates for targeted carrier diagnostics.

> The screening threshold is descriptive, not a statistical significance test.

---

## 6. Now what?

Recommendations are written as **Evidence → Hypothesis → Action/Test → KPI**, not generic advice.

| Priority | Evidence | Recommended action | Success metrics |
| --- | --- | --- | --- |
| Final-mile reliability | 90% of late-vs-on-time time gap | Build route/shipper/region final-mile scorecard; test peak capacity/routing intervention | Final-mile hours, Late Rate, Late Orders |
| Regional severity | >81% late in two difficult regions | Diagnose province, route, handoff and local-capacity constraints | Late Rate, Avg Late Duration |
| High-volume exposure | Southeast + Red River Delta = 55.3% of late orders | Prioritize scalable fixes on highest-volume lanes | Late Orders + Late Rate |
| Home / Toys | High severity and meaningful volume | Cross-tab category with seller/region/shipper before assigning cause | Category Late Rate, Late Orders |
| Shippers 164 / 41 | +13.5pp / +10.2pp vs regional baseline | Audit local workload, route mix and exception handling | Gap vs Region, Final-Mile Time |
| Customer feedback | Reviews cover only 36.0% of orders | Link themes to SLA status before inferring customer-impact drivers | Rating/theme by SLA status |

---

## Illustrative Impact Scenario

The workbook includes an editable sensitivity input.

Example assumption:

> **5 percentage-point absolute Late Rate improvement**

At current volume, that corresponds to approximately:

- **2,004 fewer late deliveries** across the network
- **1,248 fewer late deliveries** in Southeast + Red River Delta
- **530 fewer late deliveries** across Home + Toys

This is an **illustrative sensitivity scenario, not a forecast**. It does not model cost, behavioral response, mix changes, or causal lift.

---

## Critical Data-Quality Finding

The source `status` field cannot be used directly as the SLA outcome.

The QA layer finds:

- **13,233** orders labeled `success` that are actually late when comparing timestamps
- **530** orders labeled `late` that are on-time by timestamp

Therefore the canonical SLA logic is:

```text
On Time = delivery_time <= estimated_delivery_time
Late    = delivery_time >  estimated_delivery_time
```

Source status is retained for **lifecycle status**, not SLA classification.

This correction materially changes the headline Late Delivery Rate and is why KPI governance is treated as part of the analysis rather than a reporting detail.

---

## Workbook Architecture

The Excel file is structured as an analytical case study:

```text
00_Executive_Story
01_What_Happened
02_Why_Delays
03_Region_Analysis
04_Category_Analysis
05_Shipper_Diagnostic
06_Now_What
07_KPI_Dictionary
08_Data_QA
09_Data
10_Helper
```

### Story flow

```text
Raw data
   ↓
Data QA
   ↓
Canonical KPI definitions
   ↓
What happened?
   ↓
Why?
   ↓
Severity × Exposure
   ↓
Region-adjusted shipper diagnostic
   ↓
Evidence-driven action plan
   ↓
Illustrative impact scenario
```

---

## Excel Skills Demonstrated

The project intentionally uses **Excel as the full analytical environment**.

- Excel Tables / structured data
- formula-based QA and KPI logic
- date/time arithmetic
- `IF`, `COUNTIF(S)`, `SUMIF(S)`, `AVERAGEIF(S)`
- dynamic-array logic
- lookup and benchmark calculations
- Pivot-style aggregation
- trend analysis
- process decomposition
- exception screening
- conditional formatting
- management-facing charts
- sensitivity analysis
- analytical storytelling

The point is not simply to show that Excel can produce a dashboard.

It demonstrates that Excel can support:

> **data validation → KPI governance → diagnostic analysis → prioritization → decision support**

---

## Analytical Guardrails

- SLA findings are based on timestamp-derived outcomes, not source status labels.
- Stage, region, category and shipper findings are diagnostic associations, not causal proof.
- Route distance, capacity, product dimensions and handling data are not available.
- Customer feedback covers only **36.0%** of orders.
- Region-adjusted shipper thresholds are descriptive screening rules.
- The impact scenario is sensitivity analysis, not a forecast.

---

## Repository Structure

```text
delivery-operations/
├── README.md
├── Delivery Operations & SLA Diagnostic — Excel Project.xlsx
└── docs/
    ├── kpi-dictionary.md
    ├── analysis-methodology.md
    └── decision-framework.md
```

---

## Positioning

This project is designed for **Operations Analyst, Supply Chain Analyst, Logistics Analyst, E-commerce Operations, Merchandise Analytics, and Data Analyst** roles.

Its core message is:

> **The analysis does not stop at identifying the worst KPI. It separates severity from business exposure, validates measurement logic, diagnoses where delay accumulates, and converts evidence into prioritized operational decisions.**
