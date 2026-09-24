# Project 2: Retail Sales Exploratory Data Analysis (EDA)

> **DecodeLabs Data Analytics Internship — Task 2**  
> *Uncovering business insights, revenue drivers, and distributional metrics from clean transactional sales data.*

---

## Executive Summary

Following the initial data wrangling pipeline, this second project focuses on deep **Exploratory Data Analysis (EDA)** across a retail sales dataset of **1,200 clean transactions**. Using Python, Pandas, and Seaborn, the goal was to investigate underlying commercial patterns, spot high-value revenue drivers, analyze fulfillment order lifecycles, and evaluate data distributions using statistical methods.

Despite technical hardware challenges during development, this analysis was completed to deliver actionable commercial insights and form a solid foundation for downstream visual dashboards.

---

## Project Highlights & Key Accomplishments

* **Data Integrity & Scale:** Validated a pristine dataset of `1,200` rows and `14` features with zero missing values, ensuring a frictionless analytical pipeline.
* **Revenue Drivers:** Uncovered that `Chair` and `Printer` lead overall revenue generation at approximately `$195.6K` each, providing clear commercial insights into product performance.
* **Fulfillment Pipeline Breakdown:** Evaluated categorical distributions across order lifecycle stages—including `Cancelled`, `Returned`, `Pending`, `Shipped`, and `Delivered`—to track operational efficiency.
* **Statistical Rigor:** Applied Interquartile Range (`IQR`) outlier detection and categorical distribution checks to evaluate data spread and numerical integrity.

---

## Dataset Architecture & Schema

The dataset used for this exploratory analysis contains 1,200 unique records across 14 cleaned features:

| Field Name | Data Type | Missing Count | Handling & Transformation |
| :--- | :--- | :---: | :--- |
| `OrderID` | `object` | 0 | Verified primary key uniqueness (`ORD200000`–`ORD201199`) |
| `Date` | `datetime64[ns]` | 0 | Formatted to ISO standard `YYYY-MM-DD` |
| `CustomerID` | `object` | 0 | Preserved as unique customer profile identifier |
| `Product` | `object` | 0 | E-commerce product line item |
| `Quantity` | `int64` | 0 | Volume of units purchased per transaction |
| `UnitPrice` | `float64` | 0 | Individual unit price ($) |
| `ShippingAddress` | `object` | 0 | Standardized customer shipping location |
| `PaymentMethod` | `object` | 0 | Payment channel used |
| `OrderStatus` | `object` | 0 | Fulfillment state (`Delivered`, `Shipped`, `Pending`, `Returned`, `Cancelled`) |
| `TrackingNumber` | `object` | 0 | Package tracking code |
| `ItemsInCart` | `int64` | 0 | Total count of items present during checkout |
| `CouponCode` | `object` | 0 | Promotional code applied (`SAVE10`, `FREESHIP`, `No Coupon`) |
| `ReferralSource` | `object` | 0 | Marketing channel origin (`Direct`, `Social Media`, `Search`, `Email`) |
| `TotalPrice` | `float64` | 0 | Calculated total order value ($) |

---

## Workflow & Exploratory Pipeline

### 1. Environment Setup & Data Ingestion

```python
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns

# Ingest the cleansed dataset exported from Task 1
df = pd.read_excel('Cleaned Dataset for Data Analytics.xlsx')

# Verify dimensions and completeness
print(f"Dataset Shape: {df.shape}")
print(f"Total Missing Values: {df.isnull().sum().sum()}")
```
* **Environment Setup:**  Imported foundational Python data science libraries (pandas, numpy, matplotlib, seaborn).
* **Validation:** Re-confirmed a clean baseline of 1,200 rows and 14 features without missing values prior to statistical calculations.

### 2. Product Revenue Driver Analysis

# Aggregate total revenue by product line
```revenue_by_product = df.groupby('Product')['TotalPrice'].sum().sort_values(ascending=False)

print("Total Revenue by Product:")
print(revenue_by_product)
```
* **Commercial Insight:**  Highlighted Chair and Printer as top-performing revenue drivers, generating ~$195.6K each.
* **Business Value:** Provides clear commercial feedback on product line performance for stock allocation and marketing focus.

### 3. Order Status & Fulfillment Distribution

# Evaluate order status counts across fulfillment lifecycle
```status_distribution = df['OrderStatus'].value_counts()
status_percentage = df['OrderStatus'].value_counts(normalize=True) * 100

print("Order Status Distribution (Counts):")
print(status_distribution)
print("\nOrder Status Distribution (%):")
print(status_percentage.round(2))
```

* **Lifecycle Audit:** Categorized orders across Cancelled, Returned, Pending, Shipped, and Delivered stages.
* **Operational Insight:** Identified return and cancellation proportions to help quantify fulfillment operational loss.

### 4. Statistical Rigor & IQR Outlier Detection

# Compute Interquartile Range (IQR) for numerical transaction values
```Q1 = df['TotalPrice'].quantile(0.25)
Q3 = df['TotalPrice'].quantile(0.75)
IQR = Q3 - Q1

lower_bound = Q1 - 1.5 * IQR
upper_bound = Q3 + 1.5 * IQR

outliers = df[(df['TotalPrice'] < lower_bound) | (df['TotalPrice'] > upper_bound)]
print(f"Interquartile Range (IQR): {IQR:.2f}")
print(f"Lower Bound: {lower_bound:.2f} | Upper Bound: {upper_bound:.2f}")
print(f"Number of Revenue Outliers Identified: {len(outliers)}")
```

* **Statistical Audit:**  Computed Q1, Q3, and IQR boundaries on total transaction pricing.
* **Data Integrity:** Identified extreme values to ensure downstream aggregations remain statistically robust.

### 5. Categorical & Temporal Distribution Summary

# Summary statistics for categorical features
```
print("Summary Statistics for Transaction Features:")
print(df.describe(include='all'))
```

* **Distribution Inspection:**  Generated complete statistical metrics covering continuous metrics and categorical frequencies.
* **Exploratory Handoff:**  Prepared structural summary insights to guide interactive dashboard layout design for Task 3.

---

## Tech Stack & Tools

* **Language:** `Python 3.x`
* **Data Manipulation:** `pandas`, `numpy`
* **Data Visualization:** `seaborn`, `matplotlib`
* **Environment:** `Google Colab` / `Jupyter Notebook`

---

## Cleansed Data Preview

| OrderID | Date | Product | CouponCode | OrderStatus | TotalPrice |
| :---: | :---: | :---: | :---: | :---: | :---: |
| `ORD200000` | `2023-01-04` | Monitor | `SAVE10` | Shipped | `$150.00` |
| `ORD200001` | `2024-08-23` | Phone | `SAVE10` | Shipped | `$800.00` |
| `ORD200002` | `2024-02-27` | Tablet | `FREESHIP` | Cancelled | `$350.00` |
| `ORD200003` | `2023-10-15` | Chair | `SAVE10` | Returned | `$250.00` |
| `ORD200004` | `2025-05-08` | Printer | `SAVE10` | Delivered | `$200.00` |

---

## How to Run

1. Clone this repository:
   ```bash
   git clone [https://github.com/ohabuenyirita/DecodeLabs-Internship.git](https://github.com/ohabuenyirita/DecodeLabs-Internship.git)
```

2. Navigate to the project directory:
  
cd DecodeLabs-Internship/Project-2
```

3. Install required libraries:
```bash
pip install pandas numpy matplotlib seaborn openpyxl
```

4. Run the EDA script or launch the notebook:
```bash
jupyter notebook project_2_eda.ipynb
```
