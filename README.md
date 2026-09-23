# Project 1: Data Cleaning & Preparation Pipeline

> **DecodeLabs Data Analytics Internship — Task 1**  
> *Transforming raw, messy e-commerce data into a reliable, structured source of truth.*

---

## 📌 Executive Summary

They say 80% of data science is data wrangling. In this initial project for the **DecodeLabs Data Analytics Internship**, I built an end-to-end Python data cleaning and preprocessing pipeline using **Pandas** to ingest, inspect, sanitize, and standardize raw e-commerce transactional data (`Dataset for Data Analytics.xlsx`).

The objective was to transform raw operational data into a high-integrity, production-ready dataset suitable for downstream exploratory data analysis (EDA), business intelligence dashboards, and predictive modeling.

---

## 🛠️ Project Highlights & Key Accomplishments

* **Strategic Imputation:** Evaluated missing value distributions to preserve statistical integrity rather than resorting to blind listwise deletion. Handled `309` missing values in the `CouponCode` feature by replacing them with `'No Coupon'`, retaining 100% of the underlying transaction context.
* **Integrity & Duplicate Audit:** Performed primary key duplicate checks across `OrderID` and verified a **0% error rate** on unique identifiers across all 1,200 transaction records.
* **Standardization & Hygiene:** Cleansed categorical text columns by stripping trailing/leading whitespace and converting them to Title Case. Standardized all temporal fields into strict **ISO 8601 standard date formats (`YYYY-MM-DD`)**.
* **Export & Readiness:** Successfully generated and exported `Cleaned Dataset for Data Analytics.xlsx` without index headers, preparing the data for seamless ingestion into Power BI, SQL databases, or Tableau.

---

## 📊 Dataset Architecture & Schema

The original dataset contained **1,200 transaction records** across **14 features**:

| Field Name | Data Type | Missing Count | Handling & Transformation |
| :--- | :--- | :--- | :--- |
| `OrderID` | `object` | 0 | Verified primary key uniqueness (0 duplicates) |
| `Date` | `datetime64[ns]` | 0 | Formatted to ISO standard `YYYY-MM-DD` |
| `CustomerID` | `object` | 0 | Preserved as unique customer identifier |
| `Product` | `object` | 0 | Stripped whitespace, applied Title Case |
| `Quantity` | `int64` | 0 | Integer quantity ordered |
| `UnitPrice` | `float64` | 0 | Floating-point unit price |
| `ShippingAddress` | `object` | 0 | Stripped whitespace, applied Title Case |
| `PaymentMethod` | `object` | 0 | Stripped whitespace, applied Title Case |
| `OrderStatus` | `object` | 0 | Stripped whitespace, applied Title Case |
| `TrackingNumber` | `object` | 0 | Shipment tracking identifier |
| `ItemsInCart` | `int64` | 0 | Integer item count at checkout |
| `CouponCode` | `object` | 309 | Imputed missing values with `'No Coupon'` |
| `ReferralSource` | `object` | 0 | Stripped whitespace, applied Title Case |
| `TotalPrice` | `float64` | 0 | Calculated order revenue |

---

## 🚀 Workflow & Pipeline Steps

### 1. Ingestion & Initial Audit
* Loaded raw Excel workbook using Pandas (`pd.read_excel`).
* Generated structural metadata via `df.info()`, `df.isnull().sum()`, and `df.duplicated().sum()`.

### 2. Missing Value Imputation
```python
# Identify missing counts
missing_counts = df.isnull().sum()
print(missing_counts[missing_counts > 0])

# Impute missing coupon codes without dropping records
df['CouponCode'] = df['CouponCode'].fillna('No Coupon')
```
* **Logic:** Evaluated missing count across features (`missing_counts = df.isnull().sum()`).
* **Action:** Replaced 309 null values in `CouponCode` with `'No Coupon'` via `df['CouponCode'].fillna('No Coupon')`.
* **Result:** Reduced missing values from 309 to **0** while keeping all 1,200 records intact.

### 3. Duplicate Identification & Verification
# Scan for primary key collisions
```duplicate_orders = df[df.duplicated(subset=['OrderID'], keep=False)]
print(f'Duplicate OrderID rows found: {len(duplicate_orders)}')

# Drop duplicate OrderIDs, keeping the first record
df.drop_duplicates(subset=['OrderID'], keep='first', inplace=True)
print(f'Rows remaining after cleaning OrderID duplicates: {len(df)}')
```
* **Logic:** Audited unique key collisions using `df.duplicated(subset=['OrderID'], keep=False)`.
* **Action:** Confirmed zero duplicate order records existed in the source file.
* **Result:** **0% error rate** on unique primary keys, guaranteeing entity integrity.

### 4. Text & Date Normalization
# Strip whitespace and apply Title Case to text columns
```text_cols = ['Product', 'ShippingAddress', 'PaymentMethod', 'OrderStatus', 'ReferralSource']
for col in text_cols:
    if col in df.columns:
        df[col] = df[col].astype(str).str.strip().str.title()

# Format dates to ISO standard (YYYY-MM-DD)
df['Date'] = pd.to_datetime(df['Date']).dt.strftime('%Y-%m-%d')
```
* **Text Formatting:** Cleaned categorical text columns (`Product`, `ShippingAddress`, `PaymentMethod`, `OrderStatus`, `ReferralSource`) by stripping whitespace and applying Title Case via `.str.strip().str.title()`.
* **Date Formatting:** Standardized the `Date` column to the international ISO 8601 standard (`YYYY-MM-DD`) using `pd.to_datetime()`.


### 5. Final Sanity Check & Export
# Quick preview of the cleaned dataframe
```
print(df[['OrderID', 'Date', 'Product', 'CouponCode', 'OrderStatus']].head())

# Export the cleaned DataFrame to an Excel file
df.to_excel('Cleaned Dataset for Data Analytics.xlsx', index=False)
print('Cleaned data exported to Cleaned Dataset for Data Analytics.xlsx')
```
* **Validation:** Verified dataframe structure using `df.head()`.
* **Export:** Saved the cleaned data to `Cleaned Dataset for Data Analytics.xlsx` without index columns (`index=False`) for clean business intelligence integration.


---

## 💻 Tech Stack & Tools

* **Language:** Python 3.x
* **Data Manipulation:** Pandas
* **File Processing:** OpenPyXL
* **Environment:** Google Colab / Jupyter Notebook

---

## 🔍 Cleansed Data Preview

| OrderID | Date | Product | CouponCode | OrderStatus |
| :--- | :--- | :--- | :--- | :--- |
| `ORD200000` | `2023-01-04` | Monitor | SAVE10 | Shipped |
| `ORD200001` | `2024-08-23` | Phone | SAVE10 | Shipped |
| `ORD200002` | `2024-02-27` | Tablet | FREESHIP | Cancelled |
| `ORD200003` | `2023-10-15` | Chair | SAVE10 | Returned |
| `ORD200004` | `2025-05-08` | Printer | SAVE10 | Delivered |

---

## ⚡ How to Run

1. **Clone this repository:**
   ```bash
   git clone [https://github.com/ohabuenyirita/DecodeLabs-Internship.git](https://github.com/ohabuenyirita/DecodeLabs-Internship.git)
   cd DecodeLabs-Internship
