# 📊 Data Immersion & Wrangling — ApexPlanet Internship Task 1

![Python](https://img.shields.io/badge/Python-3.13-blue)
![Pandas](https://img.shields.io/badge/Pandas-2.x-green)
![Status](https://img.shields.io/badge/Status-Completed-brightgreen)
![Timeline](https://img.shields.io/badge/Timeline-10%20Days-orange)

---

## 📌 Objective

To rapidly acquaint with the provided dataset and master the critical first step of any data analysis pipeline: **acquiring, cleaning, and preparing data for analysis**.

---

## 📁 Repository Structure

```
data-wrangling-task/
│
├── sales_transactions.csv          ← Raw original dataset (16 rows, 11 columns)
├── final_sales_transactions.csv    ← Cleaned, analysis-ready dataset (13 rows, 12 columns)
├── Data_wrangling.ipynb            ← Full Jupyter Notebook with cleaning steps
├── data_dictionary.csv             ← Variable descriptions and metadata
└── README.md                       ← This file
```

---

## 📋 Dataset Overview

The dataset contains **sales transaction records** from multiple Indian cities.

| Column | Type | Description |
|---|---|---|
| Order_ID | int | Unique identifier for each order |
| Order_Date | object → datetime | Date the order was placed |
| Customer_Name | object | Name of the customer |
| City | object | City where the order was placed |
| Product | object | Product purchased |
| Category | object | Product category (Electronics / Furniture) |
| Quantity | float | Number of units ordered |
| Unit_Price | int | Price per unit (INR) |
| Payment_Method | object | UPI / Card / Cash |
| Discount | int | Discount applied (INR) |
| Rating | int | Customer rating (1–5) |

---

## 🔍 Data Quality Issues Found

| Issue | Details | Count |
|---|---|---|
| Missing Values | `Customer_Name` (1), `Quantity` (1) | 2 |
| Duplicate Rows | Full row duplicate (Order_ID 1001) | 1 |
| Inconsistent Dates | Mixed formats: `01-01-2024`, `2024/01/02`, `2024-01-04` | 3 formats |
| Outlier in Discount | Discount = 55,000 on a 55,000 laptop (100% discount) | 1 row |
| Unparseable Dates | 2 dates could not be parsed after standardization | 2 |

---

## 🧹 Cleaning Steps Performed

### Step 1 — Load & Explore
```python
import pandas as pd
import numpy as np

df = pd.read_csv("sales_transactions.csv")
df.shape      # (16, 11)
df.info()
df.isnull().sum()
df.duplicated().sum()
```

### Step 2 — Standardize Column Names
```python
df.columns = df.columns.str.strip().str.lower().str.replace(" ", "_")
```

### Step 3 — Handle Missing Values
```python
# Fill missing customer name with 'Unknown'
df["customer_name"] = df["customer_name"].fillna("Unknown")

# Fill missing quantity with median
df["quantity"] = df["quantity"].fillna(df["quantity"].median())
```

### Step 4 — Remove Duplicates
```python
df = df.drop_duplicates()
df = df.drop_duplicates(subset="order_id", keep="first")
# Result: 0 duplicate rows
```

### Step 5 — Fix Date Formats
```python
df["order_date"] = pd.to_datetime(df["order_date"], errors="coerce", dayfirst=True)
df = df.dropna(subset=["order_date"])  # Drop 2 unparseable dates
```

### Step 6 — Feature Engineering
```python
# Create order_size category from quantity
df["order_size"] = pd.cut(
    df["quantity"],
    bins=[0, 1, 3, 10],
    labels=["Small", "Medium", "Large"]
)
```

### Step 7 — Save Cleaned Dataset
```python
df.to_csv("final_sales_transactions.csv", index=False)
```

---

## 📊 Before vs After Cleaning

| Metric | Before | After |
|---|---|---|
| Rows | 16 | 13 |
| Columns | 11 | 12 |
| Missing Values | 2 | 0 |
| Duplicates | 1 | 0 |
| Date Format | Mixed (3 formats) | Uniform datetime |
| New Features | — | `order_size` |

---

## 🛠️ Tools & Libraries Used

- **Python 3.13**
- **Pandas** — data loading, cleaning, transformation
- **NumPy** — numerical operations
- **Jupyter Notebook** — interactive development

---

## ▶️ How to Run

```bash
# 1. Clone the repository
git clone https://github.com/YOUR_USERNAME/data-wrangling-task.git
cd data-wrangling-task

# 2. Install dependencies
pip install pandas numpy jupyter

# 3. Open the notebook
jupyter notebook Data_wrangling.ipynb
```

---

## 💡 Key Learnings

- Real-world data is **never clean** — always profile first
- Date columns often have **multiple inconsistent formats** — use `errors="coerce"` with `dayfirst=True`
- **Never edit raw data** — always work on a copy and save cleaned output separately
- `drop_duplicates(subset="order_id")` is safer than full-row deduplication for transactional data
- Feature engineering (like `order_size`) adds **business value** beyond just cleaning

---

## 👩‍💻 Author

G.Naga sai prasanna
ApexPlanet Software Pvt. Ltd. — Data Analytics Intern
📅 March 2026

---

*This project is part of the ApexPlanet Internship Program — Task 1: Data Immersion & Wrangling*
