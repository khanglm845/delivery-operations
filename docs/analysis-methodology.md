# Analysis Methodology

## Objective

The project is structured as an operations diagnostic rather than a reporting exercise.

The analytical sequence is:

```text
What happened?
    ↓
Why?
    ↓
So what?
    ↓
Now what?
```

The goal is to show how Excel can support decision-oriented analysis from raw data through operational prioritization.

---

## 1. Raw-data validation

The source contains **49,996 rows** and **49,996 distinct order IDs**, confirming an order-level grain.

Observed scope:

- Date range: 2022-05-01 to 2023-12-31
- 7 economic regions
- 62 provinces
- 10 categories
- 155 shippers
- 999 sellers
- 9,928 customers
- 18,022 observed reviews

Timestamp sequence QA found no invalid ordering in the checks used for:

- confirmation before purchase
- pickup before confirmation
- carrier handoff before pickup
- delivery before carrier handoff
- estimated delivery before purchase

Rows are not automatically deleted simply because a field is blank. Blank timestamps often reflect legitimate incomplete lifecycle states such as canceled, processing, or delivering orders.

---

## 2. Separate lifecycle from SLA

The source `status` field contains:

- success
- late
- canceled
- delivering
- processing

A key analytical test showed that `success` and `late` do not reliably encode promised-date performance.

Therefore:

### Lifecycle questions

Use source status.

Example:

> What percentage of all orders completed or canceled?

### SLA questions

Use timestamps.

Example:

> Of orders with a final measurable delivery outcome, what percentage arrived by the promised time?

This avoids denominator mixing and label leakage.

---

## 3. What happened?

The baseline analysis covers:

- total orders
- completion/cancellation
- on-time/late performance
- average delivery time
- average late duration
- monthly performance trend

Peak months are not manually selected.

The working rule is:

```text
Peak month = monthly order volume > 1.5 × median monthly volume
```

Median monthly orders:

**1,954**

Peak threshold:

**2,931 orders**

Detected peak months:

- 2022-11
- 2022-12
- 2023-01
- 2023-11

The purpose is to create a transparent, reproducible definition rather than cherry-picking visually high months.

---

## 4. Why?

### Late vs on-time decomposition

Instead of only asking:

> Which stage is longest overall?

the analysis asks:

> Which stage explains the difference between late and on-time orders?

This distinction matters.

A stage can consume a large share of total lead time without necessarily explaining late-delivery deterioration.

The observed total delivery gap is:

```text
143.1 h late
− 82.8 h on-time
= 60.4 h
```

Final-mile gap:

```text
96.0 h late
− 41.6 h on-time
= 54.4 h
```

Therefore final mile contributes approximately **90%** of the late-vs-on-time delivery-time difference.

### Peak vs normal decomposition

The same process stages are compared across peak and non-peak periods.

Observed end-to-end increase:

**+14.6 h**

Observed final-mile increase:

**+14.7 h**

The small difference between these values reflects offsetting movement in other stages.

Interpretation:

> Peak deterioration is concentrated downstream of carrier handoff.

This remains observational evidence.

---

## 5. So what? Severity × Exposure

A single rank by Late Rate can produce poor prioritization.

The project therefore uses two dimensions:

### Severity

How badly is the segment performing?

Primary metric:

- Late Delivery Rate

Supporting metrics:

- Avg Delivery Time
- Avg Late Duration

### Exposure

How much of the business problem comes from the segment?

Primary metric:

- Late Delivered Orders

Supporting metrics:

- SLA-eligible volume
- share of all late deliveries

---

## 6. Region prioritization

Examples:

### High severity / lower exposure

Northern Midlands & Mountains:

- 82.2% late
- 1,966 late orders
- 9.9% of all late deliveries

Central Highlands:

- 81.8% late
- 1,498 late orders
- 7.6% of all late deliveries

These are structural-service hotspots.

### Lower severity / high exposure

Southeast:

- 43.9% late
- 6,140 late orders
- 31.0% of all late deliveries

Red River Delta:

- 43.9% late
- 4,817 late orders
- 24.3%

Together they contribute **55.3% of all late deliveries**.

This creates two different management objectives:

- improve service quality where severity is extreme
- reduce absolute late volume where business scale is large

---

## 7. Category prioritization

The same Severity × Exposure method is applied to product category.

Examples:

### Severity

- Sports: 73.0% late
- Automotive: 72.7%
- Garden: 71.9%

### Exposure

- Fashion: 4,602 late orders
- Home: 4,264
- Beauty: 3,563

Fashion + Home + Beauty generate **62.7%** of all late deliveries.

Home is especially notable because it combines high severity and high exposure.

No product-handling root cause is claimed because product dimensions, weight, and handling requirements are not observed.

---

## 8. Region-adjusted shipper diagnostic

A global shipper leaderboard can confound:

```text
carrier execution
with
geographic difficulty
```

The workbook therefore evaluates:

```text
Shipper Late Rate − Region Late Rate
```

and screens only shipper-region combinations with at least:

**100 SLA-eligible orders**

A descriptive screening band of:

**±10 percentage points**

is used to separate likely local under/over-performance from normal regional variation.

This is explicitly **not** a statistical significance test.

Current screened underperformers include:

- Shipper 164 in Southeast: +13.5 pp
- Shipper 41 in Southeast: +10.2 pp

By contrast, very high late rates among Central Highlands shippers largely track the already-extreme regional baseline.

---

## 9. Now what?

Recommendations must follow:

```text
Evidence
    ↓
Working hypothesis
    ↓
Action / test
    ↓
Success KPI
```

This prevents descriptive findings from being presented as proven root causes.

Example:

### Evidence

Final mile explains ~90% of the late-vs-on-time lead-time gap.

### Working hypothesis

Capacity, routing, route structure, or handoff execution downstream of carrier handoff may be constraining performance.

### Action

Build a route/shipper/region scorecard and pilot a targeted peak-period intervention.

### KPI

- Final-Mile Time
- Late Delivery Rate
- Late Delivered Orders
- peak backlog

---

## 10. Impact scenario

The workbook includes a simple sensitivity calculation.

If the Late Delivery Rate improves by an absolute **5 percentage points**, while volume remains constant:

```text
Potential late deliveries avoided
=
SLA-eligible volume × 5%
```

Examples:

- Network: ~2,004
- Southeast + Red River Delta: ~1,248
- Home + Toys: ~530

The purpose is to translate operational performance into approximate scale.

It is not a causal forecast and does not incorporate:

- intervention cost
- segment mix changes
- demand growth
- behavioral response
- diminishing returns

---

## 11. Interpretation discipline

The project distinguishes three levels:

### Observed

What the data directly shows.

Example:

> Late orders spend 54.4 more hours in final mile than on-time orders.

### Hypothesized

A plausible mechanism.

Example:

> Final-mile capacity or route structure may be creating the gap.

### Tested next

What additional analysis or intervention would confirm/refute the hypothesis.

Example:

> Compare route load and carrier workload before/after a targeted peak-capacity pilot.

This distinction is central to the project’s analytical storytelling.
