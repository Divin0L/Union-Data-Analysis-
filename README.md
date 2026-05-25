# Union & Wage Analysis
**Analyzing the relationship between unionization rates and occupational wages using BLS data**
---
## Overview

This project investigates whether working in an occupation associated with strong unionization has a meaningful effect on wages. Using occupational employment and wage data from the U.S. Bureau of Labor Statistics (BLS), the analysis compares earnings across six occupations — three with historically high union representation and three with historically low union representation — across all U.S. states.

**Key Finding:** While high-unionhood occupations show slightly higher average wages overall, state-level economic factors appear to be a stronger predictor of wages than union association alone.

---

## Table of Contents

- [Dataset](#dataset)
- [Occupations Studied](#occupations-studied)
- [Project Structure](#project-structure)
- [Methodology](#methodology)
- [Visualizations](#visualizations)
- [Results](#results)
- [How to Run](#how-to-run)
- [Dependencies](#dependencies)
- [Possible Improvements](#possible-improvements)
- [References](#references)

---

## Dataset

- **Source:** U.S. Bureau of Labor Statistics — [Occupational Employment and Wage Statistics (OEWS)](https://data.bls.gov/oes/#/home)
- **Format:** Individual CSV files per occupation
- **Coverage:** State-level wage data across the United States

---

## Occupations Studied

| Unionhood Level | Occupations |
|---|---|
| 🔴 Low Unionhood | Agricultural Labor, General Managers, Private Investigators/Detectives |
| 🟢 High Unionhood | Police Officers, Registered Nurses, Secondary School Teachers |

---

## Project Structure

```
union-wage-analysis/
├── data/
│   ├── Agriculture (farms, ranches, aquaculture).csv
│   ├── General managers.csv
│   ├── Private investigators_detectives.csv
│   ├── Police Officers.csv
│   ├── Registered Nurses.csv
│   └── Teachers(secondary).csv
├── analysis.ipynb       # Main analysis notebook
└── README.md
```

---

## Methodology

### 1. Data Importing
Each occupation's CSV was loaded into a separate pandas DataFrame, then stored in a list for batch processing.

### 2. Data Cleaning
Each dataset had unique inconsistencies. A uniform cleaning pipeline was applied across all DataFrames:

- Converted all columns to string type for consistent processing
- Stripped currency symbols (`$`) and commas from numeric fields
- Converted numeric columns to `int64` using `pd.to_numeric(..., errors='coerce')`
- Filled or dropped missing/duplicate values
- Standardized state name formatting via a custom string extraction function
- Added `Occupation` and formatted `Area Name` columns
- Removed irrelevant columns

```python
def type_conversion(data):
    for col in data.columns:
        if data[col].dtype == 'O':
            data[col] = data[col].astype("string")
    for col in data.columns:
        if col not in ["Area Name", "Occupation"] and data[col].dtype == "string":
            data[col] = data[col].str.replace("$", "").str.replace(",", "").str.strip()
            data[col] = pd.to_numeric(data[col], errors='coerce')
```

### 3. Data Manipulation
Cleaned DataFrames were split into three merged DataFrames:

```python
low_unionhood  = pd.concat([Agricultural_labor, Managers, Private_investigators], join="inner")
high_unionhood = pd.concat([Registered_Nurses, officers, Teachers], join="inner")
combined       = pd.concat([...all six...], join="inner")
```

Additional manipulations included calculating group averages and grouping data by state and occupation for visualization.

---

## Visualizations

Six charts were produced to compare high and low unionhood occupations:

- **Bar charts** — average wages by unionhood level
- **Twin line graphs** — wage trends across states for both groups
- **Scatterplots** — relationship between state wage levels and individual occupation wages

---

## Results

- The average wages of high-unionhood jobs are higher than those of low-unionhood jobs overall
- Both groups show higher wages in states that are generally high-wage states — suggesting state-level economic conditions are a major driver
- Low-unionhood occupations are generally better compensated, with the exception of agricultural workers, who are among the lowest-paid in the dataset
- Union association alone does not appear to be a strong independent predictor of wages

---

## How to Run

1. Clone the repository:
```bash
git clone https://github.com/your-username/union-wage-analysis.git
cd union-wage-analysis

2. Run the Union_Project.ipynb file

## Dependencies
| Library | Purpose |
|---|---|
| `pandas` | Data importing, cleaning, and manipulation |
| `matplotlib` | Base plotting |
| `seaborn` | Statistical visualizations |

---

## Possible Improvements
- Expand the dataset to include more occupations for stronger statistical conclusions
- Incorporate hourly wage data in addition to annual salary figures
- Control for additional variables (education level, cost of living, industry sector)
- Apply regression analysis to better isolate the effect of unionization on wages

---

## References
- U.S. Bureau of Labor Statistics. (n.d.). *Occupational Employment and Wage Statistics (OEWS) data.* U.S. Department of Labor. https://data.bls.gov/oes/#/home
- McKinney, W. (2017). *Python for Data Analysis: Data Wrangling with pandas, NumPy, and IPython* (2nd ed.). O'Reilly Media.
- Stack Overflow. https://stackoverflow.com

---

*CSCI 171: Intro to Data Analysis | John Jay College of Criminal Justice | Spring 2026*
