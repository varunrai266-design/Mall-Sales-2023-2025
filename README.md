# 🛍️ Mall Sales Analysis (2023–2025)

An end-to-end data analysis project on retail mall sales data covering 2023–2025.  
The notebook handles data cleaning, feature engineering, exploratory analysis, and multi-sheet Excel export.

---

## 📁 Project Structure

```
mallsale.ipynb           ← Main analysis notebook
mall_sales 2023-2025.csv ← Raw input dataset
final_sales_report.xlsx  ← Output: cleaned data + 7 metric sheets
```

---

## 📦 Dependencies

| Library      | Purpose                              |
|-------------|--------------------------------------|
| `pandas`    | Data loading, cleaning, aggregation  |
| `numpy`     | Numerical operations                 |
| `matplotlib`| Visualisation (imported, ready to use)|
| `duckdb`    | In-memory SQL queries on DataFrames  |
| `openpyxl`  | Multi-sheet Excel export             |

Install all dependencies:

```bash
pip install pandas numpy matplotlib duckdb openpyxl
```

---

## 🗂️ Dataset

**File:** `mall_sales 2023-2025.csv`

| Column          | Description                              |
|-----------------|------------------------------------------|
| `Sale Date`     | Date of transaction (`DD-MM-YYYY`)       |
| `Customer Name` | Name of the customer                     |
| `Product`       | Product name                             |
| `Unit Price`    | Price per unit (INR)                     |
| `Units Sold`    | Number of units sold per transaction     |
| `State`         | Indian state where the sale occurred     |
| `Sales Channel` | Channel used (e.g., Online, In-Store)    |

---

## 🔧 Data Cleaning Steps

1. **Load** raw CSV into a pandas DataFrame
2. **Inspect** shape, dtypes, and null counts (`df.describe()`, `df.isnull().sum()`)
3. **Fill missing `Unit Price`** — imputed with column mean, rounded to integer
4. **Fill missing `Units Sold`** — imputed with column mean, rounded to integer
5. **Fill missing `Customer Name`** — forward-fill, then backward-fill for edge rows
6. **Fill missing `Sales Channel`** — forward-fill
7. **Standardise `Product` names** — converted to lowercase for consistent grouping

---

## ⚙️ Feature Engineering

| New Column    | Formula / Logic                                    |
|---------------|----------------------------------------------------|
| `Total Sale`  | `Unit Price × Units Sold` (rounded, int64)         |
| `Year`        | Extracted from `Sale Date`                         |
| `Month_Year`  | Period format (`YYYY-MM`) from `Sale Date`         |

> DuckDB was also used to validate `Total Sale` computation via an in-memory SQL query.

---

## 📊 Analysis Performed

| # | Analysis                        | Grouping Key               | Metric           |
|---|---------------------------------|----------------------------|------------------|
| 1 | State transaction count         | `State`                    | Row count        |
| 2 | State-wise revenue              | `State`                    | Sum `Total Sale` |
| 3 | Yearly sales trend              | `Year`                     | Sum `Total Sale` |
| 4 | Top 10 customers by revenue     | `Customer Name`            | Sum `Total Sale` |
| 5 | Full customer revenue ranking   | `Customer Name`            | Sum `Total Sale` |
| 6 | Top 10 products by revenue      | `Product`                  | Sum `Total Sale` |
| 7 | State-wise units sold           | `State`                    | Sum `Units Sold` |
| 8 | State × Product avg unit price  | `State`, `Product`         | Mean `Unit Price`|
| 9 | Sales channel performance       | `Sales Channel`            | Sum revenue + units |
|10 | Monthly sales trend             | `Month_Year`               | Sum `Total Sale` |
|11 | Product transaction frequency   | `Product`                  | Count of transactions |

---

## 💾 Output — Excel Report

The cleaned data and all analytical summaries are exported to:

```
C:\Users\Repair\Desktop\mall sales\final_sales_report.xlsx
```

| Sheet Name              | Contents                                    |
|-------------------------|---------------------------------------------|
| `master_clean_data`     | Full cleaned dataset                        |
| `monthly_sales`         | Monthly revenue trend                       |
| `channel_summary`       | Revenue and units by sales channel          |
| `state_product_pricing` | Average unit price per state + product      |
| `state_units`           | Total units sold per state                  |
| `top_10_products`       | Top 10 revenue-generating products          |
| `customer_revenue`      | All customers ranked by revenue             |
| `top_10_customers`      | Top 10 highest-spending customers           |

> Update `save_path` in the last cell to change the output directory.

---

## 🚀 How to Run

1. Place `mall_sales 2023-2025.csv` in your local Downloads folder  
   (or update the `pd.read_csv()` path in Cell 2)
2. Open `mallsale.ipynb` in Jupyter Notebook or VS Code
3. Run all cells top to bottom (`Kernel → Restart & Run All`)
4. The Excel report will be saved to the Desktop path defined in the last cell

---

## 📝 Notes

- `Sale Date` is parsed as `DD-MM-YYYY` — ensure the source CSV uses this format
- `Product` names are normalised to **lowercase** before grouping to avoid duplicates
- DuckDB is used for one SQL validation step; the main pipeline stays in pandas

- <img width="1332" height="741" alt="Screenshot 2026-06-27 202802" src="https://github.com/user-attachments/assets/907fa68c-a584-4c08-8fbf-368ee1448fd4" />

