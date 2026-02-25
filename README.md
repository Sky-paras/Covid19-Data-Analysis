# 🦠 COVID-19 Global Data Analysis — Python | Pandas | Matplotlib | Seaborn

> A comprehensive exploratory data analysis (EDA) project examining the global spread, mortality, and recovery patterns of COVID-19 using real-world datasets from Johns Hopkins University.

---

## 📌 Project Overview

This project dives deep into the COVID-19 pandemic data to uncover meaningful patterns across countries, provinces, and time periods. The analysis spans confirmed cases, deaths, and recoveries — transforming raw, wide-format CSV data into actionable insights through data wrangling, aggregation, and visualization.

The project was completed as part of a Python Data Analysis case study and reflects real-world data engineering challenges that arise when working with messy, inconsistent datasets.

---

## 📂 Dataset

The analysis is built on three primary datasets:

| File | Description |
|------|-------------|
| `covid_19_confirmed.csv` | Cumulative confirmed COVID-19 cases by country/province over time |
| `covid_19_deaths.csv` | Cumulative death counts by country/province over time |
| `covid_19_recovered.csv` | Cumulative recovered case counts by country/province over time |

> **Source:** Johns Hopkins University CSSE COVID-19 Dataset  
> **Format:** Wide format (each date is a separate column), later transformed to long format for analysis

---

## 🛠️ Tech Stack

- **Python 3.x**
- **Pandas** — Data manipulation, reshaping, merging
- **NumPy** — Numerical operations and data clipping
- **Matplotlib** — Data visualization (line charts, bar charts)
- **Seaborn** — Enhanced statistical plots
- **Jupyter Notebook** — Interactive analysis environment

---

## 🔍 Key Analyses Performed

### 1. Data Reshaping & Transformation
- Converted wide-format date columns into long-format using `pd.melt()` for all three datasets
- Promoted headers and corrected misaligned row structures in the deaths and recovered datasets
- Merged all three long-format DataFrames on `Country/Region` and `Date`

### 2. Top 5 Countries — Confirmed Cases Over Time
- Identified the top 5 countries by total confirmed cases (US, India, Brazil, France, Turkey)
- Visualized cumulative case trajectories using multi-line plots
- Derived insights on wave patterns, surge timelines, and country-specific outbreak behavior

### 3. China — Province-Level Breakdown
- Isolated China's data and pivoted it by province
- Identified Hubei as the epicenter with a sharp containment curve
- Compared regional spread patterns across Hong Kong, Guangdong, Shanghai, and Heilongjiang

### 4. European Surge Comparison — Germany, France, Italy
- Computed daily new cases using `.diff()` on cumulative totals
- Identified France's peak single-day surge (~118,000 cases in April 2021) as the highest among the three
- Visualized daily case fluctuations to highlight wave severity and timing

### 5. Recovery Rate — Canada vs. Australia (Dec 31, 2020)
- Calculated recovery rate as `(Recovered / Confirmed) * 100`
- Canada: **84.47%** | Australia: **79.38%**
- Canada demonstrated slightly better recovery management as of that date

### 6. Death Rate Distribution — Canadian Provinces
- Computed province-level death rates from the latest available data
- Quebec recorded the highest death rate (~3%), influenced heavily by early long-term care facility outbreaks
- Provinces like BC, Alberta, and Saskatchewan maintained comparatively lower rates

### 7. Total Deaths Per Country & Daily Average Deaths
- Ranked countries by total cumulative deaths
- Calculated average daily deaths per country and visualized the top 10
- Tracked US death evolution over time using both cumulative and daily metrics

### 8. Monthly Trend Analysis — US, Italy, Brazil
- Derived daily new confirmed cases, deaths, and recoveries from cumulative data using `.diff()`
- Aggregated on a monthly basis using `dt.to_period('M')`
- Plotted monthly progressions on a log scale to accommodate exponential growth

### 9. Highest Average Death Rates in 2020
- Filtered data to 2020 only and computed full-year death rates
- Identified the 3 countries with the highest death-to-confirmed ratios
- Discussed potential factors such as healthcare capacity, reporting standards, and demographic structure

### 10. South Africa — Recoveries vs. Deaths
- Total Recoveries: ~1.55 Million | Total Deaths: ~56,363
- Recovery-dominant outcome with a clear visual comparison

### 11. US Monthly Recovery Ratio (March 2020 – May 2021)
- Calculated monthly recovery-to-confirmed ratios
- Plotted trend to identify the peak recovery month and analyze underlying causes

---

## ⚠️ Challenges Faced & How They Were Solved

### 🔸 Challenge 1: Inconsistent Header Structure in CSV Files
The deaths and recovered datasets had misaligned headers — the actual column names were stored in the first data row instead of the header row.

**Solution:** Used `header=1` parameter and manually promoted headers using `df.columns = df.iloc[0]` followed by `df.drop(0).reset_index(drop=True)`.

---

### 🔸 Challenge 2: Wide-to-Long Format Transformation
All three datasets were in wide format — each date had its own column, making time-series analysis impossible without reshaping.

**Solution:** Applied `pd.melt()` to unpivot date columns into a single `Date` column with corresponding case counts, converting all three datasets into a consistent long format.

---

### 🔸 Challenge 3: Data Type Mismatches
Numeric columns like `Deaths` and `Recovered` were stored as object/string types after reshaping, causing aggregation errors.

**Solution:** Explicitly cast using `.astype("float")` and `.astype("int64")` after numeric conversion, ensuring all mathematical operations executed without type errors.

---

### 🔸 Challenge 4: Date Parsing Inconsistencies
Date strings were in `%m/%d/%y` format and needed to be converted to proper `datetime` objects for time-series operations.

**Solution:** Used `pd.to_datetime(df['Date'], format="%m/%d/%y", errors='coerce')` to safely parse dates and handle any edge cases without crashing the pipeline.

---

### 🔸 Challenge 5: Deriving Daily New Cases from Cumulative Data
The raw dataset only contained cumulative totals — computing day-over-day new cases required additional engineering.

**Solution:** Applied `.groupby('Country/Region')['Confirmed'].diff()` to compute daily changes, then used `.clip(lower=0)` to handle data correction artifacts that caused negative values (e.g., retroactive downward revisions in reported numbers).

---

### 🔸 Challenge 6: Countries with Non-Aligned Latest Dates
Not all countries had data up to the same final date, causing issues when filtering for "latest available data."

**Solution:** Used a dynamic approach — computed the latest date per country using `.groupby().max()`, then merged it back and used `.query('Date == LatestDate')` to correctly filter each country's most recent record.

---

### 🔸 Challenge 7: Province-Level Analysis for Canada's Death Rates
Combining province-level confirmed and death data required joining two separately grouped DataFrames without losing index integrity.

**Solution:** Set `Province/State` as index on both DataFrames and used `.join()` with `how='inner'` to cleanly merge while preserving province-level granularity. Used `np.inf` replacement to handle edge cases where confirmed cases were 0.

---

## 📊 Key Insights Summary

| Analysis | Finding |
|----------|---------|
| Highest cumulative cases | 🇺🇸 United States |
| Most aggressive second wave | 🇮🇳 India (April–May 2021) |
| China's epidemic curve | Classic containment — flat after March 2020 |
| Highest single-day surge in Europe | 🇫🇷 France (~118,000 in April 2021) |
| Better recovery rate (Dec 2020) | 🇨🇦 Canada (84.47%) vs 🇦🇺 Australia (79.38%) |
| Highest death rate — Canadian province | Quebec (~3.0%) |
| South Africa recovery outcome | Recovery-dominant (~1.55M vs ~56K deaths) |

---

## 📁 Project Structure

```
Covid19-Analysis/
│
├── Covid19_Project.ipynb       # Main analysis notebook
├── covid_19_confirmed.csv      # Raw confirmed cases data
├── covid_19_deaths.csv         # Raw death cases data
├── covid_19_recovered.csv      # Raw recovered cases data
└── README.md                   # Project documentation
```

---

## ▶️ How to Run

1. Clone the repository:
   ```bash
   git clone https://github.com/your-username/Covid19-Analysis.git
   cd Covid19-Analysis
   ```

2. Install dependencies:
   ```bash
   pip install pandas numpy matplotlib seaborn jupyter
   ```

3. Update file paths in the notebook to match your local directory structure (search for `D:\\Paras\\...` and replace with your own paths or use relative paths).

4. Launch Jupyter Notebook:
   ```bash
   jupyter notebook Covid19_Project.ipynb
   ```

---

## 🙌 Acknowledgements

- Dataset sourced from **Johns Hopkins University Center for Systems Science and Engineering (CSSE)**
- Project completed as part of a Python Data Analysis curriculum at **Coding Ninjas**

---

## 📬 Connect

Feel free to connect or reach out if you'd like to discuss the analysis or collaborate on data projects.

> ⭐ If you found this project insightful, consider giving it a star on GitHub!
