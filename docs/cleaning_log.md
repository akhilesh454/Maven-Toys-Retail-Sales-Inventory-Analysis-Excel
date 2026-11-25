# Cleaning log

All cleaning was done in **Power Query**, so every step is recorded and repeats on refresh.

## Import
- Data → Get Data → From Text/CSV (File Origin 65001: UTF-8) → Transform Data.
- `data_dictionary` was not loaded (documentation only).
- Loaded with **Only Create Connection + Add this data to the Data Model**.

## Table-by-table steps

| Table | Step | Why | Check |
|---|---|---|---|
| products | Replace `$` with nothing in Product_Cost and Product_Price | Prices were text | – |
| products | Trim, change type to Decimal | Trailing space blocked the conversion | Action Figure: cost 9.99, price 15.99 |
| products | Added Unit_Profit = Price − Cost | Quick per-unit view | Action Figure 6.00 |
| stores | Store_Open_Date → Date | Text in source | 1992-09-18 to 2016-05-18 |
| stores | Replace `Cuidad de Mexico` → `Ciudad de Mexico` | Source typo (store names were spelt correctly) | 4 stores in Ciudad de Mexico |
| inventory | All columns → Whole Number | Correct types | 1,593 rows, 29,742 units |
| calendar | Removed the automatic Changed Type step; Change Type → **Using Locale → Date, English (United States)** | `1/2/2022` would be read as 1 February on Indian settings | Row 2 = 2 Jan 2022; 638 days |
| calendar | Added Year, Month, Month Name, Quarter, Day Name, Week of Year, Start of Month | Time analysis | 2022–2023 |
| calendar | Conditional column Day_Type (Saturday/Sunday = Weekend) and Month Short | Weekend analysis and chart labels | – |
| sales | Date → Date; IDs and Units → Whole Number | Correct types | 829,262 rows; 1,090,565 units |

## Helper queries (for worksheet formulas)
| Query | Steps | Output |
|---|---|---|
| Product_Summary (`tblProducts`) | Reference sales → Group By Product_ID (Sum Units) → Merge products → Revenue, Cost, Profit columns | 35 rows; totals match the Data Model ($14,444,572.35 revenue) |
| Stock_Detail (`tblStock`) | Reference inventory → Merge products and stores | 1,593 rows; 77 with zero stock |

## Model settings
- 5 relationships: sales → products, stores, calendar; inventory → products, stores.
- calendar marked as Date Table; Month Name sorted by Month (Sort by Column).
- ID columns hidden from client tools.

## Validation
| Check | Result |
|---|---|
| Total revenue (Data Model vs Product_Summary) | $14,444,572.35 in both |
| Duplicate Sale_IDs | 0 |
| Sales rows without a matching product or store | 0 |
| Blank values | 0 |

## Decisions
- 2023 has only 9 months, so year comparisons use **Jan–Sep vs Jan–Sep**.
- Inventory has no date, so Year and Timeline slicers are not connected to stock visuals.
- No rows were removed.
