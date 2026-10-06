# KPI Dictionary

Canonical definitions used in the Delivery Operations & SLA Diagnostic workbook.

## Population and grain

- **Analytical grain:** one row per order
- **Total orders:** 49,996
- **Completed outcome:** source status in `success` or `late`
- **SLA-eligible delivered order:** completed outcome with both actual and estimated delivery timestamps

The source `status` field is used for **lifecycle state**, not for the final SLA classification.

---

## Lifecycle KPIs

| KPI | Definition | Denominator | Current value |
| --- | --- | --- | ---: |
| Total Orders | All distinct order records | All orders | 49,996 |
| Completed Orders | `status = success or late` | All orders | 40,077 |
| Completion Rate | Completed / Total | All orders | 80.16% |
| Canceled Orders | `status = canceled` | All orders | 3,422 |
| Cancellation Rate | Canceled / Total | All orders | 6.84% |
| Delivering Orders | `status = delivering` | All orders | 4,515 |
| Delivering Rate | Delivering / Total | All orders | 9.03% |
| Processing Orders | `status = processing` | All orders | 1,982 |
| Processing Rate | Processing / Total | All orders | 3.96% |

These four lifecycle statuses reconcile to approximately 100% of the order base.

---

## SLA KPIs

### SLA-Eligible Delivered Orders

A completed order with:

- non-blank actual delivery timestamp
- non-blank estimated delivery timestamp

Current population:

**40,077 orders**

### On-Time Delivered Order

```text
delivery_time <= estimated_delivery_time
```

Current result:

**20,265 orders**

### Late Delivered Order

```text
delivery_time > estimated_delivery_time
```

Current result:

**19,812 orders**

### On-Time Delivery Rate

```text
20,265 / 40,077 = 50.57%
```

### Late Delivery Rate

```text
19,812 / 40,077 = 49.43%
```

### Reconciliation

```text
On-Time Orders + Late Orders = SLA-Eligible Delivered Orders
20,265 + 19,812 = 40,077
```

and:

```text
On-Time Rate + Late Rate = 100%
```

---

## Why source status is not the SLA metric

The QA layer compares source labels against actual timestamps.

It finds:

- **13,233** orders labeled `success` but delivered after the estimated timestamp
- **530** orders labeled `late` but delivered on/before the estimated timestamp

Therefore:

> Source status describes the operational lifecycle outcome, but timestamp comparison is the canonical SLA rule.

This is the most important KPI-governance decision in the project.

---

## Time KPIs

| KPI | Definition | Population | Current value |
| --- | --- | --- | ---: |
| Avg End-to-End Delivery Time | purchase → actual delivery | SLA-eligible delivered orders | 112.6 h |
| Avg Late Duration | actual delivery − estimated delivery | Late delivered orders only | 57.3 h |
| Avg Confirmation Time | purchase → seller confirmation | completed eligible orders | 6.5 h |
| Avg Pickup Time | seller confirmation → pickup | completed eligible orders | 12.5 h |
| Avg Transit Time | pickup → carrier handoff | completed eligible orders | 25.1 h |
| Avg Final-Mile Time | carrier handoff → customer delivery | completed eligible orders | 68.5 h |

### Important denominator rule

Late Duration is calculated only on late deliveries.

On-time deliveries are **excluded**, not inserted as zero values, because the metric answers:

> When an order is late, how severe is the lateness?

---

## Stage-gap diagnostic

The workbook compares on-time vs late orders using a common completed-order population.

| Stage | On-Time Avg | Late Avg | Gap |
| --- | ---: | ---: | ---: |
| Confirmation | 6.4 h | 6.7 h | +0.3 h |
| Pickup | 11.9 h | 13.1 h | +1.3 h |
| Transit | 22.9 h | 27.3 h | +4.4 h |
| Final Mile | 41.6 h | 96.0 h | +54.4 h |
| Total Delivery | 82.8 h | 143.1 h | +60.4 h |

Final mile accounts for approximately:

```text
54.4 / 60.4 ≈ 90%
```

of the observed late-vs-on-time lead-time gap.

This is a **diagnostic association**, not causal proof.

---

## Review KPIs

### Review Coverage

```text
18,022 reviewed orders / 49,996 total orders ≈ 36.0%
```

### Avg Customer Rating

Approximately:

**5.1**

The source supports an observed numeric score, but the analysis should avoid treating review results as representative of all customers because nearly two-thirds of orders have no review.

---

## QA rules

Before trusting a refresh:

1. Total rows = distinct order IDs.
2. Duplicate order IDs = 0.
3. Completed + Canceled + Delivering + Processing = Total Orders.
4. On-Time + Late = SLA-Eligible Delivered Orders.
5. On-Time Rate + Late Rate = 100%.
6. Time-stage durations must be non-negative.
7. Timestamp sequence anomalies must be reviewed.
8. Customer-feedback metrics must disclose review coverage.
9. Source status must never replace timestamp-derived SLA classification.
