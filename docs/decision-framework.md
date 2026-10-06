# Decision Framework

This document translates the workbook findings into an operations decision model.

The core principle is:

> **Do not prioritize only by the worst percentage. Prioritize by severity, exposure, diagnostic confidence, and measurable impact.**

---

## 1. Priority dimensions

### Severity

How badly is the segment performing?

Primary signals:

- Late Delivery Rate
- Avg Late Duration
- Avg Delivery Time

### Exposure

How much of the total operational problem sits in the segment?

Primary signals:

- Late Delivered Orders
- SLA-eligible delivery volume
- Share of all late deliveries

### Diagnostic confidence

How directly does the current data support the proposed action?

Examples:

- Final-mile gap: strong diagnostic evidence
- Carrier issue after regional adjustment: moderate diagnostic evidence
- Product handling cause: weak / unsupported with current fields

### Actionability

Can the business test an intervention and measure the result?

Every recommendation should have:

- a defined scope
- an action/test
- a success KPI

---

## 2. Severity × Exposure matrix

```text
                         HIGH SEVERITY
                              ↑
       Targeted Diagnosis     |      Top Priority
       Low volume / high risk |      High volume / high risk
                              |
LOW EXPOSURE  ←───────────────┼────────────────→ HIGH EXPOSURE
                              |
       Monitor                |      Scale Opportunity
       Low volume / low risk  |      High volume / moderate risk
                              ↓
                         LOWER SEVERITY
```

This prevents two common mistakes:

1. ranking only by Late Rate
2. ranking only by order volume

---

## 3. Regional decision map

### Severity hotspots

**Northern Midlands & Mountains**
- 82.2% late
- 1,966 late orders
- 171.7 h average delivery
- 81.8 h average late duration

**Central Highlands**
- 81.8% late
- 1,498 late orders
- 170.7 h average delivery
- 80.1 h average late duration

### Decision

Treat as **structural-service diagnostics**.

Investigate:

- province concentration
- route length / density
- carrier handoff timing
- local capacity
- service-promise realism

Do not immediately replace a shipper based only on headline late rate.

---

### Exposure hotspots

**Southeast**
- 6,140 late orders
- 31.0% of total late deliveries

**Red River Delta**
- 4,817 late orders
- 24.3%

Together:

**55.3% of all late deliveries**

### Decision

Treat as **scale-efficiency opportunities**.

Focus on:

- highest-volume routes
- absolute late-order count
- repeatable process improvements

A modest percentage improvement here can create larger total impact than a much larger percentage improvement in a small region.

---

## 4. Category decision map

### High severity

- Sports: 73.0% late
- Automotive: 72.7%
- Garden: 71.9%

These are targeted diagnostic candidates.

### High exposure

- Fashion: 4,602 late orders
- Home: 4,264
- Beauty: 3,563

These three generate:

**62.7% of all late deliveries**

### Strongest combined signal

**Home**
- 60.8% late
- 4,264 late orders
- 21.5% of total late deliveries

### Decision

Home should be investigated first using:

- region
- seller
- shipper
- route / handoff context

Do not infer weight or handling complexity from category alone.

---

## 5. Final-mile decision rule

Observed:

- Late-vs-on-time total delivery gap: +60.4 h
- Final-mile contribution: +54.4 h
- Share of total gap: ~90%
- Peak-period total delivery increase: +14.6 h
- Peak-period final-mile increase: +14.7 h

### Decision

Final mile is the first process stage to diagnose.

### Next operational breakdown

Create a scorecard by:

```text
Region
× Shipper
× Month / Peak period
× Final-Mile Time
× Late Rate
× Late Orders
```

If route-level fields become available, add:

- route distance
- number of stops
- daily shipment load
- delivery density
- service type

---

## 6. Carrier decision rule

Do not rank shippers globally.

First compare each shipper to its operating context.

```text
Gap vs Region
=
Shipper Late Rate − Regional Late Rate
```

Current screening:

- minimum 100 SLA-eligible deliveries
- ≥ +10pp vs region → underperformance candidate
- ≤ −10pp vs region → outperformance candidate
- otherwise → within local range

Current targeted candidates:

### Shipper 164 — Southeast
- Late Rate: 57.4%
- Region: 43.9%
- Gap: +13.5pp

### Shipper 41 — Southeast
- Late Rate: 54.1%
- Region: 43.9%
- Gap: +10.2pp

These warrant deeper diagnosis.

The ±10pp threshold is a **descriptive screening rule**, not statistical evidence.

---

## 7. Peak-period operating playbook

Peak months identified from volume:

- Nov 2022
- Dec 2022
- Jan 2023
- Nov 2023

Observed pattern:

- higher Late Rate
- weaker Completion Rate
- higher Cancellation Rate
- longer end-to-end time
- deterioration concentrated in final mile

### Before peak

Review:

- carrier/route capacity
- historical late-order exposure
- regional backlog
- highest-volume categories
- exception thresholds

### During peak

Monitor weekly:

- SLA-eligible volume
- Late Orders
- Late Rate
- Final-Mile Time
- cancellation
- processing/delivering backlog

### After peak

Compare pilot areas against:

- pre-peak baseline
- untreated routes/regions where possible
- prior peak periods

---

## 8. Recommendation hierarchy

### Priority 1 — Final-mile reliability

**Evidence**  
~90% of late-vs-on-time lead-time gap occurs in final mile.

**Action**  
Build route/shipper/region scorecard and test targeted peak intervention.

**KPIs**
- Final-Mile Time
- Late Rate
- Late Orders

---

### Priority 2 — High-severity regions

**Evidence**  
Two regions exceed 81% late rate.

**Action**  
Diagnose geographic, route, handoff, and service-promise constraints.

**KPIs**
- regional Late Rate
- Avg Late Duration
- Avg Delivery Time

---

### Priority 3 — High-exposure regions

**Evidence**  
Southeast + Red River Delta generate 55.3% of all late orders.

**Action**  
Focus scalable improvements on highest-volume lanes.

**KPIs**
- Late Orders
- Late Rate
- SLA-eligible volume

---

### Priority 4 — Home category

**Evidence**  
60.8% late and 21.5% of all late orders.

**Action**  
Cross-tab Home by region/seller/shipper before testing handling hypotheses.

**KPIs**
- category Late Rate
- Late Orders
- concentration by operating context

---

### Priority 5 — Screened shippers

**Evidence**  
Shippers 164 and 41 exceed Southeast baseline by >10pp.

**Action**  
Audit workload, route mix, handoff timing, and exception handling.

**KPIs**
- Gap vs Region
- Final-Mile Time
- Late Orders

---

## 9. Impact translation

The workbook includes an editable sensitivity input.

For a 5 percentage-point absolute Late Rate improvement:

| Scope | SLA-Eligible Volume | Approx. Late Deliveries Avoided |
| --- | ---: | ---: |
| Network | 40,077 | ~2,004 |
| Southeast + Red River Delta | 24,952 | ~1,248 |
| Home + Toys | 10,599 | ~530 |

This helps management understand scale.

It is not a forecast of causal lift.

---

## 10. What additional data would most improve decisions?

The highest-value next fields would be:

1. route distance / origin-destination
2. daily shipper capacity and workload
3. delivery density / stop count
4. product weight and dimensions
5. seller fulfillment location
6. service level / shipping method
7. failed-attempt timestamps
8. refund / return outcomes

These would allow the analysis to move from:

> diagnostic patterns

toward:

> root-cause validation and intervention design.
