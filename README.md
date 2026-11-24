# Excel MIS Reporting Pack: US Regional Sales

A monthly MIS reporting pack built in Excel with Power Query. It combines per-warehouse sales files into one clean table, then reports sales vs target by team, fulfilment (SLA) performance and problem orders, with automatic control checks that reconcile the workbook to the source data.

**Tools:** Excel (Power Query, tables, PivotCharts, named ranges, data validation, conditional formatting, COUNTIFS / SUMIFS / INDEX-MATCH / TEXTJOIN).

![Summary sheet](screenshots/01_summary.png)

## Scenario and data

A multi-warehouse distributor sends sales files every month. The analyst has to consolidate them, confirm nothing was lost, and report to management. This workbook does that in one repeatable file.

Data: Kaggle *US Regional Sales Data* (talhabu), synthetic, in USD. I split the original CSV into six warehouse files covering May 2018 to Nov 2020 (7,748 orders) to mimic branch submissions. A reconciliation key (rows, revenue and profit) was computed from the original file **before** splitting, as an independent check.

## Workbook

- **Summary:** one-page view of the report month: KPIs, teams meeting target, fulfilment rates, exception counts, check status, revenue trend.
- **Sales_MIS:** team target vs actual (Met / Below) with pivot charts.
- **Fulfilment_MIS:** per-warehouse days to ship and deliver, and SLA breach rates.
- **Exceptions:** counts of loss-making, heavy-discount, slow-dispatch and slow-delivery orders.
- **Control_Checks:** automatic PASS / FAIL tests against the key.
- **Settings:** editable thresholds and targets, with data validation.
- **Sales_Data / Pivots:** the cleaned combined table and the pivots behind the charts.

## How it works

- Power Query combines every CSV in a folder, cleans once on the combined table, and adds Revenue, Profit, margin and order-to-ship / ship-to-delivery days.
- Thresholds are named cells on Settings (SLA_Dispatch, SLA_Delivery, Max_Discount, Loss_Limit, Target_Growth), so a limit is changed in one place and every report follows.
- A Flags column tags each order with every rule it breaks. Targets are each team's average monthly revenue plus 5%, rounded to the nearest 1,000.

**Assumptions:** dispatch SLA 21 days, delivery SLA 7 days, heavy discount 30% or more, loss = profit below 0, target growth 5%. These are analyst assumptions, not facts from the data.

## Control checks

Six tests, all passing against the key: 7,748 rows, revenue 70,832,295.81, profit 20,688,082.03, 362 loss-making orders, 0 duplicate order numbers, 28 teams. A difference under 0.10 counts as rounding.

![Control checks](screenshots/03_control_checks.png)

## Findings (Nov 2020)

- 262 orders, about $2.30M revenue, about $0.65M profit (28.1% margin).
- 11 of 28 teams (39%) met target.
- 26.3% of orders were dispatched late and 33.2% delivered late. Across all data, 53% of orders carry at least one flag, which suggests the SLAs may be tighter than real service levels.

![Sales MIS](screenshots/02_sales_mis.png)

## Refresh and limitations

New monthly files placed in the data folder are meant to be picked up with **Data > Refresh All**; then check Control_Checks and read the Summary.

- **A refresh with the held-back December 2020 files was not completed**, so I do not claim that refresh as tested.
- The query's folder path is absolute. After downloading, repoint it via **Data > Get Data > Data Source Settings > Change Source**.
- The data is synthetic, so findings illustrate the method, not real results.

## Skills demonstrated

Multi-file consolidation with Power Query, data cleaning, reconciliation to a source of truth, parameter-driven reporting, rule-based exception reporting, KPI and SLA reporting, data validation, and a one-page management summary.
