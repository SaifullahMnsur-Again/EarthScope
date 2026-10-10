# 🌍 Earth System Trend Detective

[![NASA Space Apps 2026](https://img.shields.io/badge/NASA%20Space%20Apps-2026-blue)](https://www.spaceappschallenge.org/)
[![Python](https://img.shields.io/badge/Python-3.10+-green.svg)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)

**From "what the data appears to show" to "what the evidence actually supports."**

An interactive web application that transforms NASA Earth-observation data into trends you can investigate, test, and trust, region by region. Built for the **NASA Space Apps Challenge 2026** — challenge: *Be an Earth System Trend Detective*.

> **Project status:** This repository describes a functional prototype that is **in active development**. Features detailed below represent our target design for hackathon submission. No scientific finding is claimed until it has been computed from verified data, quality-checked, and rigorously tested.

---

## 📖 The Story Behind the Project

A farmer in Bangladesh stands in his field. The seasons feel unpredictable: some years bring too little rain, others more than the land can handle. He wonders, *"Is this just a bad year, or is something changing?"*

He searches for answers and finds colorful maps, charts full of numbers, and conflicting reports about environmental change. One question stays unanswered: **how do we know what is really happening?**

- A rising line may look alarming. But is the change **statistically significant**?
- A falling trend may be a long-term problem or just a temporary natural cycle.
- One region may be getting wetter while another gets drier. A **national average can hide the changes that matter most.**

The problem isn't a lack of data. Satellites observe our planet, datasets track its changes, and models help explain its behavior. **The problem is turning all that data into evidence we can trust.**

*Earth System Trend Detective* is our answer: investigate change region by region, measure how fast it is happening, test whether the trend is statistically credible, compare regions, and explore what might come next.

---

## 🎯 What the App Answers

| Question | How the App Addresses It |
|---|---|
| **What** is changing? | Variable selector, trend map, and time-series chart |
| **Where** is it changing? | Interactive map and region inspector |
| **How much** is it changing? | Sen's slope (rate of change) and baseline-vs-recent comparison in real units |
| **Is it significant?** | Mann–Kendall test, p-values, confidence intervals, and coverage status |
| **Can we trust it?** | Data-quality checks, observation coverage, window sensitivity, and a transparent methods panel |
| **Do places behave differently?** | Side-by-side regional comparison using identical methods |

Our core purpose is global. **Bangladesh serves as our primary initial case study**, where variations in rainfall and temperature directly impact agriculture and livelihoods. The core engine is architected to scale seamlessly to other global regions and environmental variables.

---

## ✨ Planned Features

### Core (MVP)

1. **Trend Explorer Map** – Choose a variable, time period, and region. The map displays trend direction and magnitude, clearly separating *significant*, *not significant*, and *insufficient data* results using pattern overlays and colorblind-aware palettes. Bangladesh is masked with official boundary polygons.
2. **Region Inspector & Time-Series Chart** – Click any region to inspect raw observations over time, units, estimated trends, observation counts, and missing-data coverage. Confidence bands are displayed strictly when supported by documented statistical methods.
3. **Statistical Trend Engine** – Calculates Sen's slope for rate/direction and the Mann–Kendall test for monotonic trends. Utilizes Benjamini–Hochberg false discovery rate (FDR) correction when evaluating multiple spatial grid cells.
4. **ML Forecast Engine** – A machine-learning model estimates potential future values for selected variables using time-ordered train/validation/test splits, benchmarked against baseline forecasts with explicit error metrics and prediction intervals.
5. **Significance, Coverage & Uncertainty** – Valid-observation counts and missing-data thresholds are explicitly reported. Regions with sparse data are flagged as *insufficient data* rather than falsely reported as *no trend*.
6. **Regional Comparison** – Side-by-side multi-region evaluation across synchronized timeframes, variables, units, and statistical parameters.
7. **Methods & Insights Panel** – Displays dataset metadata, product versions, spatial/temporal resolutions, assumptions, and limitations with rule-based plain-language summaries.
8. **Data Export** – Download filtered statistical outputs as CSV and spatial trend layers as GeoJSON.

### Optional / Future Scope

- **Mirror Finder:** Detect regions where identical variables move in opposing directions over matching timeframes.
- **Window Sensitivity View:** Interactively evaluate how trend slope and significance shift across varying start/end years.
- **Expanded Datasets:** Integrate MODIS NDVI (vegetation index) and GPM (Global Precipitation Measurement) satellite products.
- **Global Map View:** Expand boundary masking globally following regional pipeline validation.

---

## 🛰️ Data Plan

Our primary candidate dataset is **NASA POWER** (meteorological and solar parameters including surface temperature and precipitation). The application explicitly distinguishes whether data layers originate from direct satellite observations, data assimilation systems, or reanalysis models. 

For each evaluated region/cell, the engine processes:
`[Identifier, Variable, Product Version, Period, Value & Unit, Valid Observation Count, Missing Data %, Baseline vs. Recent Values, Sen's Slope, Mann–Kendall p-value, Adjusted p-value (FDR), Confidence Intervals, Engine Config]`

---

## 🧪 Methods at a Glance

- **Sen's Slope Estimator:** Robust non-parametric estimate of change per unit time.
- **Mann–Kendall Test:** Non-parametric test for monotonic trend existence.
- **Quality Assurance:** Assesses seasonality, autocorrelation, missing data density, and extreme outliers prior to trend calculation.
- **Multiple Testing Correction:** Benjamini–Hochberg procedure applied across multi-cell grid outputs.
- **ML Validation:** Rigorous time-series split validation evaluated against naive/persistence baselines.

*Note: Statistical significance (p < 0.05) indicates mathematical trend credibility, not direct physical causation.*

---

## 🧱 Proposed Technical Stack

| Category | Tools & Libraries |
|---|---|
| **App Interface** | Python, Streamlit |
| **Interactive Maps** | Folium, `streamlit-folium` |
| **Data Visualization** | Plotly |
| **Data Processing** | pandas, NumPy |
| **Geospatial Processing** | GeoPandas, Shapely, `h3-py` |
| **Gridded Datasets** | xarray, rasterio |
| **Statistical Analysis** | SciPy, `pymannkendall` |
| **Machine Learning** | scikit-learn |

---

## ⚠️ Limitations & Responsible Use

- Satellite, assimilation, and reanalysis products differ in spatial resolution and sensor uncertainty.
- Seasonal cycles, missing observations, and sensor calibrations can mimic long-term trends if unadjusted.
- A statistical trend indicates historical trajectory; it does not assign physical cause or guarantee future outcomes.
- Satellite observation records cover distinct decades; shorter time windows may capture multi-year natural oscillations rather than permanent climate shifts.

---

## 👥 Team

| Name | Role |
|---|---|
| **Shanto Islam** | Team Leader, Web Developer |
| **Sayfullah Mansur Sujon Sarkar** | ML Researcher |
| **Salma Fariha Koly** | Idea Generation |
| **Jamema Joty** | UI/UX Designer |
| **Zerin Islam** | UI/UX Designer |
| **Al Fahad Uthso** | Presenter, Content Lead |

---

## 🏆 Challenge Mapping

**NASA Space Apps Challenge 2026:** *Be an Earth System Trend Detective*

| Challenge Requirement | Our Approach |
|---|---|
| **NASA-measured or modeled variables** | NASA POWER Open API & NASA Earth Science datasets |
| **Visualize change over time** | Trend heatmaps, interactive time-series plots, forecast overlays |
| **What / Where / How much** | Variable selection, regional spatial inspector, Sen's slope in real units |
| **Is it significant?** | Mann–Kendall test, p-values, confidence bands, FDR corrections |
| **Regional variance** | Multi-region comparison view & Mirror Finder pipeline |

---

## 🤖 AI Use Disclosure

AI tools were utilized for brainstorming, documentation structuring, and initial code refactoring. All scientific computations, statistical tests, and data transformations displayed in the app are calculated by open-source scientific algorithms from verified NASA datasets.

---

## 📄 License

This project is licensed under the **MIT License** — see the [LICENSE](LICENSE) file for details.
