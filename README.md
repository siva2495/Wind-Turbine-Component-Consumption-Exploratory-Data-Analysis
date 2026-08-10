# Wind Turbine Component Consumption — Exploratory Data Analysis

---

## 📌 Project Overview

This project demonstrates an end-to-end **exploratory data analysis (EDA)** workflow for wind turbine component/material consumption data.

The analysis investigates how consumption varies across countries and turbines, with particular focus on:

- **Installed/observed WTG population**
- **Total component consumption**
- **Turbine age**
- **Consumption intensity per turbine**
- **Country-level statistical outliers**
- **Relationship between turbine age and consumption**

The project is designed as a **data-science portfolio example**, combining Python-based data preparation, statistical analysis and visual storytelling with an energy/wind-energy domain context.

> **Data confidentiality:** The source workbook is not included in this repository. The notebook is designed to run when an authorised copy of the source Excel file is placed in `data/`.

---

## 🎯 Project Objectives

### Primary objective

Understand the distribution and characteristics of wind turbine component consumption across countries and identify patterns that could support **reliability, maintenance and spare-parts planning**.

### Specific objectives

1. Assess the quality and structure of the source data.
2. Engineer useful temporal and turbine-age features.
3. Quantify the number of unique WTGs by country.
4. Compare total consumption across countries.
5. Normalise consumption by the number of WTGs.
6. Identify unusually high/low countries using z-scores.
7. Explore the relationship between turbine age and consumption.
8. Establish a foundation for future predictive maintenance/reliability analytics.

---


## 🧰 Tech Stack

| Category | Tools |
|---|---|
| Language | Python |
| Data manipulation | Pandas, NumPy |
| Visualisation | Matplotlib, Seaborn |
| Data source | Microsoft Excel |
| Development | Jupyter Notebook / VS Code |
| Version control | Git / GitHub |
| Domain | Wind Energy / Reliability Analytics |

---

## 🔬 Methodology

The analysis follows a reproducible EDA workflow:

```text
Raw Excel Data
      │
      ▼
Data Loading
      │
      ▼
Data Quality Assessment
      │
      ├── Shape
      ├── Data types
      └── Missing values
      │
      ▼
Data Preparation
      │
      ├── Date conversion
      ├── Numeric conversion
      ├── Age in years
      └── Calendar features
      │
      ▼
Country-level Aggregation
      │
      ├── Unique WTG count
      ├── Total consumption
      ├── Average age
      └── Consumption intensity
      │
      ▼
Statistical Analysis
      │
      ├── Z-scores
      └── Correlation
      │
      ▼
Visual Analytics
      │
      ├── Country comparisons
      └── Age vs consumption
      │
      ▼
Business / Reliability Insights
```

---

## 📊 Dataset

The displayed analysis contains **18,517 records and 15 columns**.

The source data includes fields representing:

- Country
- Wind farm (`WF`)
- Wind turbine generator (`WTG`)
- Turbine generation
- Turbine type
- Power rating
- Component
- Material code
- Material description
- Consumption date
- Turbine/component age
- Quantity consumed (`Sum of Q`)
- Unit
- Order information

The exact source data is intentionally excluded because it may contain proprietary/company information.

---

## 🧹 Data Preparation

The notebook performs the following preparation steps:

### Date handling

The consumption date is converted to a proper datetime field, enabling extraction of:

- Year
- Month
- Day

### Turbine age

Age is available in days and is converted to years:

```python
data["Age (years)"] = (data["Age (d)"] / 365).round(2)
```

### Numeric standardisation

Consumption and age fields are converted to numeric types to support aggregation and statistical analysis.

---

## 📈 Key Metrics

### 1. Unique WTG count

The number of unique wind turbines represented in each country:

```python
unique_WTG = WTG.nunique()
```

This provides the turbine population context behind country-level consumption.

### 2. Total consumption

Total quantity consumed by country:

```python
total_consumption = Sum_of_Q.sum()
```

### 3. Average turbine age

Mean turbine age is calculated both in days and years.

### 4. Consumption intensity

A normalised country-level metric is introduced:

\[
CI_{country} =
\frac{\text{Total Consumption}}
{\text{Number of Unique WTGs}}
\]

This is important because absolute consumption is naturally influenced by the number of turbines in a country.

A country with a large fleet may have high total consumption without necessarily having high consumption per turbine.

### 5. Z-score

Z-scores are calculated to identify countries that deviate from the country-level average:

\[
z = \frac{x-\mu}{\sigma}
\]

This is applied to:

- Total consumption
- Average age
- Average age in years
- Consumption intensity

---

performance.

This motivated the introduction of `CI_country`, consumption per unique WTG.

### 2. Consumption intensity provides a more meaningful comparison

Normalising by WTG population helps distinguish:

> "Countries with high consumption because they have many turbines"

from:

> "Countries with high consumption relative to their turbine population."

This is more useful for comparative reliability and maintenance analysis.

### 3. Country-level variation is substantial

The z-score analysis demonstrates that some countries deviate considerably from the overall country-level distribution in consumption, age and consumption intensity.

These observations can be used to identify candidates for deeper investigation.

### 4. Turbine age and consumption show a positive exploratory association

The notebook calculates the correlation between country-level average turbine age and total consumption. The displayed result is approximately:

**r ≈ 0.069**

This is a **very weak linear association** at the country-aggregated level.

Importantly, this does **not** mean turbine age has no relationship with component consumption. It means that this particular country-level aggregation does not reveal a strong linear relationship.

### 5. Aggregation limits causal interpretation

Because the correlation is calculated using country-level averages, it should not be interpreted as evidence that turbine age causes consumption.

A stronger analysis should operate at **WTG/component level** and control for:

- Turbine generation
- Power rating
- Component type
- Operating age
- Wind farm
- Calendar/time effects
- Number of operating hours
- Maintenance history

---

# 💡 Business & Data Science Relevance

This EDA can serve as the first stage of a broader **wind turbine reliability analytics framework**.

The insights can potentially support:

### Spare-parts planning

Identify countries/components with consistently high consumption.

### Reliability benchmarking

Compare consumption intensity across turbine fleets.

### Maintenance prioritisation

Identify WTGs or components with unusually high consumption.

### Failure-risk modelling

Use historical consumption as a potential feature for predictive maintenance models.

### Fleet health monitoring

Combine consumption, age and operational variables into an asset-health framework.

---

# 🚀 Future Work

The current project is intentionally an EDA foundation. The next stage could include:

### 1. Component-level analysis

Analyse consumption by:

- Component
- Material code
- Material description
- Turbine type

### 2. WTG-level anomaly detection

Identify turbines with unusually high consumption using:

- IQR
- Robust z-score
- Isolation Forest
- Local Outlier Factor

### 3. Time-series analysis

Investigate monthly/quarterly consumption trends and seasonality.

### 4. Reliability modelling

Create features such as:

- Consumption frequency
- Time between consumption events
- Repeat component replacements
- Component-specific consumption rate
- Age at replacement

### 5. Predictive modelling

Potential targets:

```text
Probability of component replacement
        ↓
Expected consumption
        ↓
Maintenance / spare-parts risk
```

Candidate models:

- Logistic Regression
- Random Forest
- XGBoost
- Survival Analysis
- Gradient Boosting

### 6. Dashboard

A Power BI dashboard could provide:

- Fleet overview
- Country benchmarking
- Component consumption
- WTG-level drill-down
- High-risk asset identification
- Maintenance indicators

---
size, total consumption and consumption intensity. Applied z-score analysis and correlation analysis to identify fleet-level patterns and potential reliability indicators.


## 👤 Author

**Sivakumar R.**

Data Science Practitioner | Energy & Renewable Energy Analytics

**Focus areas:** Data Science • Time Series • Renewable Energy • Energy Analytics • Reliability Analytics • Predictive Maintenance

---

## 📄 License

This project is released under the MIT License. The original source dataset is excluded from the repository due to potential proprietary/confidential information.
