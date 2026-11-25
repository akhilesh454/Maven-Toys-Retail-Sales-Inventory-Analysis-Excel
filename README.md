# Maven Toys Retail Sales & Inventory Analysis (Advanced Excel)

An interactive one-page Excel dashboard analysing **829,262 sales transactions** across **50 toy stores in Mexico** (Jan 2022 – Sep 2023), built with Power Query, the Data Model, DAX, PivotCharts, dynamic array formulas and What-If analysis.

> **The Big Picture:** Maven Toys is growing, but not profitably enough. Revenue grew **30.9%** (Jan–Sep 2023 vs Jan–Sep 2022) while profit grew only **16.0%**, because sales shifted towards low-margin Art & Crafts and the highest-margin category, Electronics, fell 28%. At the same time, the best sellers are running out of stock.

---

## Project at a glance

| | |
|---|---|
| **Business** | Maven Toys, a fictional toy store chain (simulated dataset) |
| **Period** | 1 Jan 2022 – 30 Sep 2023 (2023 has 9 months) |
| **Data** | 5 related CSV files: sales, products, stores, inventory, calendar |
| **Size** | 829,262 transactions · 1,090,565 units · 50 stores · 29 cities · 35 products · 5 categories |
| **Tools** | Excel 2024 · Power Query · Data Model (Power Pivot) · DAX · PivotTables & PivotCharts · slicers & timeline · XLOOKUP · dynamic arrays · IFS · RANK.EQ · Goal Seek |
| **Sheets** | Dashboard · Trend · Category · Products · Location · Stores · Weekday · Growth · Inventory · Product_Summary · Stock_Detail · Formulas · What_If · Notes |

### Headline numbers

| Revenue | Profit | Margin | Units sold | Transactions | Avg revenue / transaction | Stock-outs | Inventory value (cost) |
|---|---|---|---|---|---|---|---|
| $14.44M | $4.01M | 27.8% | 1.09M | 829.3K | $17.42 | 77 | $300.2K |

---

## Business questions

| # | Question |
|---|---|
| 1 | Which categories and products drive revenue and profit? |
| 2 | How do sales change by month and weekday? |
| 3 | Which store types, cities and stores perform best? Is any store losing money? |
| 4 | Why did profit grow slower than revenue in 2023? |
| 5 | Where is stock running out? |
| 6 | What price change is needed to reach a $4.5M profit target? |

---

## Process

### 1. Power Query: import and cleaning
| Issue | Action |
|---|---|
| Prices stored as text (`"$9.99 "`) | Removed `$`, trimmed spaces, changed to Decimal |
| Calendar dates in US format (`1/2/2022`) | Change Type → **Using Locale → English (United States)** so 1/2/2022 = 2 January |
| City typo `Cuidad de Mexico` | Replaced with `Ciudad de Mexico` |
| Calendar had only a date column | Added Year, Month, Month Name, Quarter, Day Name, Week, Day Type (Weekday/Weekend), Month Start |
| 829K rows too heavy for a worksheet | Loaded as **connection only** into the **Data Model** |
| Formulas need small tables | **Group By** helper queries: `tblProducts` (35 rows) and `tblStock` (1,593 rows) |

Full log: [`docs/cleaning_log.md`](docs/cleaning_log.md)

### 2. Data model
Star schema with **5 one-to-many relationships**.
- **Facts:** `sales` (one row per transaction), `inventory` (stock snapshot per store and product)
- **Dimensions:** `products`, `stores`, `calendar` (marked as Date Table)
- `inventory` has no date, so stock measures ignore Year/Timeline filters (by design).

### 3. DAX measures
Units Sold · Transactions · Total Revenue · Total Cost · Total Profit · Profit Margin % · Avg Units per Transaction · Avg Revenue per Transaction · Active Stores · Revenue per Store · Stock On Hand · Inventory Value (Cost/Retail) · Out of Stock Items · Days of Stock Cover

Revenue and cost use `SUMX` + `RELATED` (units × price row by row). Definitions: [`docs/kpi_definitions.md`](docs/kpi_definitions.md)

### 4. Analysis and formulas
- 8 PivotTable analysis sheets: trend, category, products (Top 10), location, stores, weekday, Jan–Sep growth, inventory
- Worksheet formulas: XLOOKUP product card (multi-column return), RANK.EQ profit rank, IFS margin tiers, dynamic Top 10 (`SORTBY` + `TAKE` + `CHOOSECOLS`), city dropdown (`SORT(UNIQUE())`), out-of-stock list by city (`FILTER` + `HSTACK`)
- What-If model: price and volume inputs, **Goal Seek** for a $4.5M profit target

### 5. Dashboard
- 20 × 20 px cell grid, custom dark theme (`.thmx`), purple highlight / mint profit / red-orange stock alerts
- 5 KPI cards linked to a hidden helper PivotTable, custom number formats (`$14.44M`)
- PivotCharts: Monthly Revenue & Profit (combo), Profit by Category, Revenue per Store by Location
- Top 10 Products and Inventory Alerts shown as **Linked Pictures** of formatted pivots
- Year, Category and Store Location slicers + Date timeline connected to all visuals (Report Connections)
- What-if price dropdown showing the new profit instantly
- Sheet and workbook protection with slicers still working

---

## Key insights

1. **Revenue is not profit.** Lego Bricks is the #1 revenue product ($2.39M) but earns only 12.5%; Colorbuds makes the most profit ($835K, 53.4% margin).
2. **Mix shift explains the margin drop.** Art & Crafts +252%, Electronics −28% (Jan–Sep); margin fell from 29.6% to 26.2%.
3. **Electronics is the profit engine per product:** 3 products, 44.6% margin.
4. **Airport stores are the most productive:** $430K revenue per store vs about $275–283K for other store types (only 3 stores).
5. **Friday and Saturday bring 38.8% of revenue;** December 2022 and March 2023 were the peak months.
6. **Stock risk on best sellers:** 77 stock-outs in 37 of 50 stores; Dino Egg (6.3 days) and Playfoam (7.7 days) have about a week of stock left.
7. **Every store is profitable;** the weakest (Campeche 2) still earns $58K.

### Recommendations
1. Protect and promote high-margin products (Electronics, Colorbuds, Jenga, Playfoam); review pricing or supplier cost for low-margin best sellers (Lego Bricks, Dino Egg, Rubik's Cube).
2. Investigate why Electronics sales fell 28%.
3. Reorder Dino Egg, Playfoam, Barrel O' Slime and Hot Wheels now; slow down overstocked items (Mini Basketball Hoop, about 94 days of cover).
4. Test small price increases: **+3.36%** price would lift profit to **$4.5M** if volumes hold (Goal Seek).

---

## Limitations
- Simulated data for a fictional company; the value is in the workflow.
- Inventory is a single snapshot, so stock-outs cannot be tracked over time.
- 2023 covers only 9 months; growth compares Jan–Sep with Jan–Sep.
- The what-if model assumes volume does not react to price.
- Chart highlight colours are set manually and do not move with filters.

## Future improvements
- Demand forecast with `FORECAST.ETS` and reorder points per store
- Price elasticity test before changing prices
- One-click refresh/reset with VBA or Office Scripts
- Rebuild in Power BI for web sharing

---

## Repository structure
```
├── README.md
├── MavenToys_Sales_Inventory_Analysis.xlsx
├── dataset/
│   ├── *.csv            Maven Toys CSV files
│   └── README_data.md   source and file descriptions
└── docs/
    ├── cleaning_log.md
    └── kpi_definitions.md
```

## How to open
1. Download `MavenToys_Sales_Inventory_Analysis.xlsx` and open it in **Excel 2021 / 2024 / Microsoft 365 for Windows** (the Data Model and dynamic array formulas need a recent desktop version).
2. Click **Enable Content** if asked.
3. Use the slicers, the timeline and the what-if dropdown on the Dashboard sheet.

## Data source and credits
- Data: [Maven Analytics Data Playground](https://mavenanalytics.io/data-playground) – "Mexico Toy Sales" (Maven Toys), a simulated dataset.
- *Personal portfolio project.*

## Author
**Akhilesh Pradhan** · Data Analyst / Business Analyst
LinkedIn: _[add link]_ · Email: _[add email]_
