# Dataset: Maven Toys (Mexico Toy Sales)

**Source:** [Maven Analytics Data Playground](https://mavenanalytics.io/data-playground) – "Mexico Toy Sales". Simulated sales and inventory data for a fictional chain of toy stores in Mexico, published for learning and portfolio use.

**Period:** 1 January 2022 – 30 September 2023 · **Format:** CSV (UTF-8)

## Files

| File | One row represents | Rows | Columns |
|---|---|---|---|
| `sales.csv` | One sales transaction | 829,262 | Sale_ID, Date, Store_ID, Product_ID, Units |
| `products.csv` | One product | 35 | Product_ID, Product_Name, Product_Category, Product_Cost, Product_Price |
| `stores.csv` | One store | 50 | Store_ID, Store_Name, Store_City, Store_Location, Store_Open_Date |
| `inventory.csv` | Current stock of one product in one store (snapshot, no date) | 1,593 | Store_ID, Product_ID, Stock_On_Hand |
| `calendar.csv` | One date | 638 | Date |
| `data_dictionary.csv` | Description of each field | 19 | Table, Field, Description |

## Notes on the raw data
- `Product_Cost` and `Product_Price` are text such as `"$9.99 "` (dollar sign and trailing space).
- `sales.Date` and `stores.Store_Open_Date` use `yyyy-mm-dd`; `calendar.Date` uses US `m/d/yyyy`.
- One city is misspelt in the source: `Cuidad de Mexico` (corrected in Power Query).
- `Store_Location` has four values: Downtown (29 stores), Commercial (12), Residential (6), Airport (3).
- Categories: Toys (9 products), Art & Crafts (8), Games (8), Sports & Outdoors (7), Electronics (3).
- No missing values, no duplicate Sale_IDs and no IDs without a matching product or store.
- `sales.csv` is about 21.8 MB, under GitHub's 25 MB web upload limit.

Cleaning steps: [`../docs/cleaning_log.md`](../docs/cleaning_log.md)
