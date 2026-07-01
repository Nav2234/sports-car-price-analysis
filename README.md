# 🏎️ Sports Car Price Analysis & Market Trends: Decoding What Drives Six-Figure (and Seven-Figure) Price Tags

An end-to-end data analytics project — data curation, EDA, statistical correlation analysis, and an interactive Tableau dashboard — uncovering the performance variables that actually predict sports car prices across 1,007 vehicles from 38 global manufacturers.

**🔗 Live Dashboard:** [Tableau Public — Sports Car Price Analysis](https://public.tableau.com/app/profile/navjot.singh1075/viz/2SportsCarPriceAnalysis/Dashboard1)

---

## Executive Summary

**Business Problem:** The sports car market is highly competitive and price-opaque. Consumers, analysts, and dealerships lack a clear view of which performance specs (horsepower, torque, engine size, acceleration) actually justify a car's price — making it hard to buy smart, price competitively, or benchmark against rivals.

**Solution:** I built a full analytics pipeline — sourcing and cleaning a 1,007-row / 38-brand dataset, engineering a composite "Performance Score," running correlation analysis in Excel, and shipping an interactive Tableau dashboard with brand, year, and price-tier filters — to quantify exactly which specs drive price and by how much.

**Key Numbers & Impact:**
- **r = 0.80** — Horsepower is the strongest single predictor of price (after correcting a data quality error)
- **r = 0.60** — A custom-engineered composite Performance Score outperforms any individual spec as a predictor
- **8×** — Bugatti's average price ($3.25M) sits at 8x the dataset mean, evidence of a measurable "brand premium" beyond pure performance
- **54%** of the market sits below $200K — the median ($140K) tells a very different story than the skewed mean ($383K)
- Identified and corrected a **10,000 HP data entry error** that was silently understating the true HP–price relationship by 30%+

**Next Steps:** Build ICE vs. EV segmented regression models, add brand as a fixed effect to isolate "brand premium" from performance, and validate findings against real-world (non-LLM-generated) transaction data.

---

## Business Problem

Sports car pricing varies enormously — from $25,000 entry-level performance cars to $5.2M hypercars — and it's unclear how much of that spread is explained by measurable performance (horsepower, torque, engine size, acceleration) versus brand prestige alone.

**Why it matters:**
- **Consumers** overpay (or underestimate value) without knowing which specs are actually price-justified.
- **Manufacturers & dealerships** lack a data-driven benchmark for pricing strategy and product positioning.
- **Analysts** need a reliable base for building predictive pricing models.

**Goal:** Determine which performance variables most significantly impact sports car prices, and how those relationships shift across brands and market tiers — then package the findings into a tool stakeholders can actually use.

---

## Methodology

| Phase | What I Did |
|---|---|
| **Data Sourcing** | Sourced a primary dataset (Kaggle, 1,007 rows × 8 cols) plus 2 supplementary validation datasets to cross-check pricing trends |
| **Data Profiling & Cleaning** | Used Excel (AutoFilter, `COUNTBLANK()`, `MIN/MAX/AVERAGE/MEDIAN`, `SUMPRODUCT(1/COUNTIF())`) to audit every column for missing values, outliers, and inconsistent categorical entries |
| **Data Quality Investigation** | Identified a 10,000 HP data-entry error (a Tesla row) that was distorting correlation results; flagged it via a calculated field rather than deleting the row, preserving data integrity |
| **Feature Engineering** | Built 6 new columns: `price_tier`, `decade`, and a min-max normalized **composite Performance Score** (HP + torque + inverted 0–60 time) |
| **Exploratory Data Analysis** | Univariate (histograms, descriptive stats) and bivariate (scatter plots + trendlines) analysis of every performance variable against price |
| **Statistical Analysis** | Pearson correlation coefficients (Excel Data Analysis Toolpak / `CORREL()`) between each independent variable and price |
| **Dashboard Development** | Built an interactive Tableau Public dashboard with brand, year-range, and price-tier filters for non-technical stakeholders |
| **Reporting** | Documented findings, data quality issues, and recommendations in a formal final report |

**Tools:** Microsoft Excel (Data Analysis Toolpak, pivot tables, histograms), Tableau Public
**Techniques:** Data profiling & cleaning, feature engineering, min-max normalization, Pearson correlation analysis, exploratory data visualization, dashboard/BI development

---

## Skills

- **Data Cleaning & Profiling** — missing value audits, outlier detection, categorical consistency checks, data quality documentation
- **Feature Engineering** — composite metric design, min-max normalization, categorical binning
- **Exploratory Data Analysis (EDA)** — univariate and bivariate analysis, distribution diagnostics (skew detection), scatter/trendline interpretation
- **Statistical Analysis** — Pearson correlation, outlier-impact quantification (before/after correlation comparison)
- **Data Visualization** — Excel charting (histograms, pivot charts), Tableau dashboard design (KPI cards, scatter plots, heatmaps, donut charts)
- **Business Intelligence / Dashboarding** — Tableau Public, interactive filters, stakeholder-facing design
- **Analytical Storytelling & Reporting** — translating statistical output into business recommendations, structured technical report writing

---

## Results & Business Recommendations

| Finding | Recommendation | Who Benefits |
|---|---|---|
| Horsepower is the strongest single predictor (r = 0.80, outlier-corrected) | Dealerships and manufacturers can use HP as the primary lever in pricing/positioning conversations | Manufacturers, Dealerships |
| The composite Performance Score (r = 0.60) beats any single spec | Predictive pricing tools should use a blended performance metric rather than horsepower alone | Analysts, Data Science teams |
| Price is right-skewed (mean $383K vs. median $140K) | Report median, not mean, when benchmarking "typical" market pricing; segment analysis by price tier | Consumers, Analysts |
| Brand carries a premium beyond performance (e.g., Pagani: $2.79M at "only" 758 HP) | Brand should be modeled as its own variable — don't assume price = performance | Manufacturers, Investors |
| EVs break the engine-size-to-horsepower relationship | Any pricing model must include an EV/ICE flag; do not apply one model to both vehicle types | Data Science / Modeling teams |
| Engine size is a weak standalone predictor (r = 0.40) | Deprioritize engine displacement as a marketing/pricing anchor in favor of HP and torque | Marketing, Product teams |

**Bottom line:** Sports car pricing is driven primarily by measurable performance (horsepower and a composite performance metric), but brand identity adds a substantial premium on top — meaning any pricing or valuation model needs both a performance component *and* a brand component to be accurate.

---

## Next Steps & Limitations

**Limitations:**
- The primary dataset is **LLM-generated**, not sourced from real market transactions — it may not capture real-world factors like limited-edition premiums, regional pricing, dealer markups, or demand fluctuations. Findings should be treated as **illustrative, not prescriptive**.
- A data entry error (10,000 HP) required manual detection and correction — a reminder that automated validation rules would strengthen future pipelines.
- The dataset is a single snapshot; no true time-series pricing data (e.g., resale/depreciation trends) was available.

**If I had more time, I would:**
- Build **linear and log-linear regression models** to quantify the exact dollar impact of each additional HP/lb-ft/second, and formally test whether log(Price) improves model fit given the skew
- **Segment the dataset into ICE vs. EV subsets** and model each separately, since they follow fundamentally different pricing logic
- Add **brand as a fixed effect / dummy variable** in a regression to statistically isolate the "brand premium" from the performance effect
- Source **real-world transaction data** (e.g., AutoTrader, Cars.com, Hagerty) to validate — or challenge — the findings from the LLM-generated dataset
- Add a **"predict my price" input panel** to the Tableau dashboard so users can enter HP/torque/0-60 and get an estimated price range, turning it from a descriptive tool into a decision-support tool

---

## Repository Structure

```
sports-car-price-analysis/
├── README.md
├── data/
│   ├── Sport_car_price.csv
│   ├── Sport_car_price.xlsx
│   └── car_price_prediction_.csv
├── docs/
│   ├── Project_Scoping_Final.pdf
│   ├── Project_Description.pdf
│   ├── Data_Curation.pdf
│   ├── EDA_Sports_Car.pdf
│   ├── Datafolio_Sports_Car_v2.pdf
│   ├── dashboard.pdf
│   └── Final_Report.pdf
└── dashboard/
    └── dashboard_link.md   (link to live Tableau Public dashboard)
```

---

## Source & Credits

- **Primary dataset:** [Sports Car Prices Dataset — Kaggle (rkiattisak)](https://www.kaggle.com/datasets/rkiattisak/sports-car-prices-dataset)
- **Validation datasets:** [Car Price Analysis Dataset](https://www.kaggle.com/datasets/ayeshasiddiqa123/cars-pre/data), [Vehicle Price Prediction Dataset (Baselight/Kaggle)](https://baselight.app/u/kaggle/dataset/metawave_vehicle_price_prediction)
- **Tools:** Microsoft Excel, Tableau Public

