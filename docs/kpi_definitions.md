# KPI definitions

All measures are explicit DAX measures in the Data Model. Values are for the full period (Jan 2022 – Sep 2023) with no filters.

| Measure | Definition | DAX logic | Value |
|---|---|---|---|
| Units Sold | Total units sold | `SUM(sales[Units])` | 1,090,565 |
| Transactions | Number of sales rows | `COUNTROWS(sales)` | 829,262 |
| Total Revenue | Units × product price, row by row | `SUMX(sales, sales[Units] * RELATED(products[Product_Price]))` | $14,444,572.35 |
| Total Cost | Units × product cost, row by row | `SUMX(sales, sales[Units] * RELATED(products[Product_Cost]))` | $10,430,543.35 |
| Total Profit | Revenue − Cost | `[Total Revenue] - [Total Cost]` | $4,014,029 |
| Profit Margin % | Profit ÷ Revenue | `DIVIDE([Total Profit], [Total Revenue])` | 27.79% |
| Avg Units per Transaction | Units ÷ Transactions | `DIVIDE([Units Sold], [Transactions])` | 1.32 |
| Avg Revenue per Transaction | Revenue ÷ Transactions | `DIVIDE([Total Revenue], [Transactions])` | $17.42 |
| Active Stores | Stores with at least one sale | `DISTINCTCOUNT(sales[Store_ID])` | 50 |
| Revenue per Store | Revenue ÷ Active Stores | `DIVIDE([Total Revenue], [Active Stores])` | $288,891 |
| Stock On Hand | Units in stock now | `SUM(inventory[Stock_On_Hand])` | 29,742 |
| Inventory Value (Cost) | Stock × cost | `SUMX(inventory, inventory[Stock_On_Hand] * RELATED(products[Product_Cost]))` | $300,209.58 |
| Inventory Value (Retail) | Stock × price | `SUMX(inventory, inventory[Stock_On_Hand] * RELATED(products[Product_Price]))` | $410,240.58 |
| Out of Stock Items | Store–product combinations with zero stock | `CALCULATE(COUNTROWS(inventory), inventory[Stock_On_Hand] = 0)` | 77 |
| Days of Stock Cover | Stock ÷ average daily units of the last 90 days (3 Jul – 30 Sep 2023) | `DIVIDE([Stock On Hand], CALCULATE([Units Sold], 'calendar'[Date] >= DATE(2023,7,3)) / 90)` | Dino Egg 6.3 · Playfoam 7.7 |

## Notes
- **Why SUMX + RELATED:** units are in `sales`, prices in `products`. Revenue must be calculated per row; `SUM(Units) × SUM(Price)` would multiply two totals.
- **Margin is weighted:** total profit ÷ total revenue (27.8%), not the average of product margins (32.3%).
- **Stock measures** come from a snapshot and do not change with Year or Timeline filters.
- **Days of Stock Cover** uses a fixed window because the data ends on 30 Sep 2023; a live model would calculate the window from the last sale date.

## Worksheet KPIs (formulas)
| Column / output | Logic | Result |
|---|---|---|
| Margin % (tblProducts) | Profit ÷ Revenue | Jenga 70.1% highest, Dino Egg 9.1% lowest |
| Profit_Rank | `RANK.EQ` on Profit, descending | Colorbuds 1 |
| Margin_Tier | `IFS`: ≥ 40% High, ≥ 25% Medium, else Low | 11 / 11 / 13 products |
| What-if New Profit | Base Revenue × (1 + price %) × (1 + volume %) − Base Cost × (1 + volume %) | +3.36% price → $4.5M (Goal Seek) |
