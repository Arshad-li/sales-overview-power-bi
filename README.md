# sales-overview-power-bi
Power BI sales dashboard built on AA Data Labs 2025 practice data: cleaned in Power Query (removed 25 duplicate rows), DAX measures for net revenue, target achievement and average order value, reconciled to the answer key.

# Sales Overview Dashboard (Power BI)

![Dashboard](Screenshot 2026-11-25 141245.png‎)

A one-page Power BI dashboard built on a synthetic 2025 sales dataset. The data is made-up practice data. It is not client or employer data.

## What the dashboard shows

* KPI cards: Gross Revenue, Net Revenue, Target Achievement, Total Orders and Delivered Orders
* Net Revenue by Region
* Net Revenue by Category
* Net Revenue by Month
* A region table with annual target, delivered orders, total orders, net revenue, target achievement and average order value

## Key results

| Measure | Value |
|---|---|
| Gross revenue | 8,646,808.5 |
| Net revenue (Delivered orders only) | 7,415,806 |
| Annual target | 7,866,000 |
| Target achievement | 94.28% |
| Total orders | 1,500 |
| Delivered orders | 1,304 |
| Average order value (Delivered) | 5,686.97 |

* No region reached its target. West was closest (96.99%) and South was lowest (92.74%).
* East had the highest net revenue (2.11M), followed by North (2.08M).
* Furniture made up 55.35% of net revenue.
* January was the strongest month (0.858M) and March the weakest (0.479M).

## Data quality step

The first version of the dashboard overstated net revenue (7.53M instead of 7.42M). Order counts looked correct because a distinct count of order IDs hides repeated rows. A check measure showed 25 duplicate rows. I removed the exact duplicates in Power Query, and the results then matched the answer key supplied with the dataset.

## DAX measures

```
Revenue = Sales[quantity] * Sales[unit_price] * (1 - Sales[discount_pct] / 100)

Gross Revenue = SUM(Sales[Revenue])

Net Revenue = CALCULATE(SUM(Sales[Revenue]), Sales[status] = "Delivered")

Total Orders = DISTINCTCOUNT(Sales[order_id])

Delivered Orders = CALCULATE(DISTINCTCOUNT(Sales[order_id]), Sales[status] = "Delivered")

Annual Target = SUM(Targets[target])

Target Achievement % = DIVIDE([Net Revenue], [Annual Target])

Duplicate Rows = COUNTROWS(Sales) - DISTINCTCOUNT(Sales[order_id])
```

Net revenue counts Delivered orders only. Returned and Cancelled orders are excluded.

## Tools

Power BI Desktop, Power Query, DAX and Excel.

## Files

* `Sales_Overview_Dashboard.pbix`: the Power BI report
* `Excel_Practice_Dataset.xlsx`: the synthetic source data
* `Sales_Overview.png`: screenshot of the dashboard
