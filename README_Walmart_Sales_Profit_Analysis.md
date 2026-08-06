# Walmart Sales & Profit Analysis

An end-to-end data analysis project on Walmart transaction data, using **Python (Pandas)** for cleaning, **PostgreSQL/SQL** for advanced business analysis, and **Power BI** for an interactive sales & profit dashboard.

## Overview

This project analyzes **~10,000 Walmart transactions** across **100 branches** to uncover revenue trends, profit drivers, payment behavior, and branch-level performance — including which branches are declining year-over-year.

## Tech Stack

| Layer | Tools |
|---|---|
| Data Cleaning | Python, Pandas |
| Data Storage | PostgreSQL (loaded via SQLAlchemy) |
| Data Analysis | SQL (window functions, CTEs) |
| Visualization / Reporting | Power BI |

## Dataset

- **Source file:** `walmart_data.csv`
- **Raw size:** 10,051 rows × 11 columns
- **Key fields:** `invoice_id`, `branch`, `city`, `category`, `unit_price`, `quantity`, `date`, `time`, `payment_method`, `rating`, `profit_margin`

## Data Cleaning (Python)

- Loaded the raw CSV with Pandas (`encoding_errors='ignore'` to handle encoding issues)
- Removed duplicate rows (10,051 → 10,000)
- Dropped rows with missing values (10,000 → **9,969** clean records)
- Converted `unit_price` from a currency string (e.g. `$74.69`) to a numeric `float`
- Engineered a new `total` column (`unit_price × quantity`) to represent transaction revenue
- Standardized column names to lowercase
- Loaded the cleaned dataset into a **PostgreSQL** database using `SQLAlchemy` (`df.to_sql(...)`) for downstream SQL analysis

## SQL Analysis

Business questions answered using SQL (`sql_valmart.sql`), including:

- Total transactions, distinct branches, and basic aggregates
- Payment-method-wise transaction count and quantity sold
- **Highest-rated category per branch** (using `RANK() OVER (PARTITION BY branch ORDER BY AVG(rating) DESC)`)
- **Busiest day per branch** based on transaction count
- Total quantity sold per payment method
- City-wise category rating spread (min / max / avg)
- **Total profit per category** (`unit_price × quantity × profit_margin`)
- **Most common payment method per branch** (using a CTE + `RANK()`)
- Sales segmented into **Morning / Afternoon / Evening** shifts
- **Top 5 branches with the highest year-over-year revenue decline** (2022 vs 2023), using two CTEs joined together

**SQL concepts used:** `GROUP BY`, `CASE` statements, `CTE`s, window functions (`RANK() OVER (PARTITION BY ...)`), date/time extraction (`EXTRACT`, `TO_DATE`, `TO_CHAR`)

## Key Insights

- Total revenue across all branches: **$1.21M**
- Total profit: **$476K**
- Data spans **100 branches**, average customer rating of **5.83**
- **Credit Card** is the leading payment method, used in **42.7%** of transactions, followed by E-wallet (38.9%) and Cash (18.4%)
- **Fashion Accessories** and **Home & Lifestyle** are the top two revenue-driving categories
- A dedicated query flags the 5 branches with the steepest revenue decline from 2022 to 2023, useful for identifying underperforming locations

## Dashboard

An interactive Power BI dashboard (`WALMART_DASHBORD.PNG`) was built with:
- KPI cards: total revenue, total profit, branch count, average rating
- Revenue by category (bar chart)
- Transactions by payment method (donut chart)
- Revenue by month (trend line)
- Revenue by city (map)
- Top category per branch and top 5 branches by revenue (tables)

## Repository Structure

```
├── Python_Notebook.ipynb      # Python data cleaning & PostgreSQL loading
├── sql_valmart.sql            # SQL analysis (branch, category, payment, time-based)
├── walmart_data.csv           # Raw dataset
├── WALMART_DASHBORD.PNG       # Power BI dashboard screenshot
└── README.md
```

## How to Run

1. Install dependencies:
   ```bash
   pip install pandas sqlalchemy psycopg2
   ```
2. Open `Python_Notebook.ipynb` in Jupyter Notebook and run all cells to clean the data and load it into PostgreSQL.
3. Update the database connection string in the notebook (`postgresql+psycopg2://user:password@localhost:5432/walmart`) to match your local setup.
4. Run the queries in `sql_valmart.sql` against the `walmart` table to reproduce the analysis.
5. Open the Power BI file (if included) to explore the interactive dashboard, or view `WALMART_DASHBORD.PNG` for a static snapshot.

## Author

**Yash Bharti**
Aspiring Data Analyst | Python, SQL, Power BI, Excel
📧 yashfact2@gmail.com
