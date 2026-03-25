# Time Intelligence Semantic Modeling Framework (DAX)

A modular and scalable **DAX-based semantic modeling framework** for implementing advanced **time intelligence analytics** in **Power BI**, **Analysis Services**, and **Excel Power Pivot**.

This repository goes beyond static measures by introducing a **parameter-driven, switchable metric architecture** for rolling, cumulative, and statistical analysis.

---

# Overview

This project implements a **layered semantic model** that enables:

* Dynamic rolling window analysis (7, 12, 30, 90 days)
* Rolling Average & Rolling Median (outlier-resistant)
* Year-to-Date (YTD) Average
* Rolling YTD Hybrid Metrics
* Switchable metric logic via slicers
* Reusable and scalable DAX design patterns

---

# Architecture

The model is structured into four logical layers:

# 1. **Date Layer**

* Centralized `DateTable`
* Drives all time intelligence logic

# 2. **Parameter Layer (Control Layer)**

* Rolling Period Selector
* Metric Selector

# 3. **Computation Layer**

* Moving averages
* Rolling median
* YTD calculations

# 4. **Abstraction Layer**

* Unified **SWITCH-based measure**
* Enables dynamic metric selection

---

# 📁 Repository Structure

```
dax-time-intelligence-semantic-model/
│
├── models/
│   ├── date_table/
│   │   └── date_table.dax
│   │
│   ├── parameters/
│   │   ├── rolling_period_table.dax
│   │   └── metric_selector_table.dax
│   │
│   └── measures/
│       ├── base_measures.dax
│       ├── rolling_measures.dax
│       └── switch_measures.dax
│
├── docs/
│   ├── architecture.md
│   ├── measure_definitions.md
│   └── usage_guide.md
│
├── reports/
│   └── dashboard.pbix
│
└── README.md
```

---

# Date Table (Foundation Layer)

A fully featured date dimension supporting all time-based calculations.

# Key Features

* Configurable date range (default: 2015–2030)
* Calendar attributes: Year, Month, Quarter, Week
* Day-level attributes: Day name, weekend flag
* Optimized for sorting and filtering

```dax
DateTable = 
VAR MinDate = DATE(2015, 1, 1)
VAR MaxDate = DATE(2030, 12, 31)
RETURN
ADDCOLUMNS(
    CALENDAR(MinDate, MaxDate),
    "Year", YEAR([Date]),
    "Month Number", MONTH([Date]),
    "Month Name", FORMAT([Date], "MMMM"),
    "Month Short", FORMAT([Date], "MMM"),
    "Quarter", "Q" & FORMAT([Date], "Q"),
    "Year-Month", FORMAT([Date], "YYYY-MM"),
    "Week Number", WEEKNUM([Date], 2),
    "Day", DAY([Date]),
    "Day Name", FORMAT([Date], "DDDD"),
    "Day Short", FORMAT([Date], "DDD"),
    "Is Weekend", IF(WEEKDAY([Date], 2) > 5, TRUE(), FALSE())
)
```

---

# 🎛️ Parameter Layer (Dynamic Control)

# Rolling Period Table

Controls window size dynamically:

* 7 days
* 12 days
* 30 days
* 60 days
* 90 days

# Metric Selector

Enables switching between:

* Moving Average
* Rolling Median
* YTD Average
* Rolling YTD Average

---

# Core Measures

# Dynamic Moving Average

* Parameter-driven rolling window
* Responds to slicer selection

# Rolling Median

* Outlier-resistant alternative to averages
* Uses `MEDIANX` over rolling window

# YTD Average

* Expanding window from start of year
* Robust against sparse data

# Rolling YTD Average

* Hybrid model: rolling window constrained within year

---

# Switchable Metric (Abstraction Layer)

A single unified measure dynamically switches logic:

```dax
Switchable Rolling Metric =
VAR SelectedMetric =
    SELECTEDVALUE ( 'Rolling Metric Selector'[Metric], "Moving Average" )
RETURN
    SWITCH (
        TRUE (),
        SelectedMetric = "Moving Average", [Dynamic Moving Average],
        SelectedMetric = "Rolling Median", [Dynamic Rolling Median],
        SelectedMetric = "YTD Average", [YTD Average],
        SelectedMetric = "Rolling YTD Average", [Rolling YTD Average]
    )
```

---

# 🎯 Usage Guide

### Step 1: Model Setup

* Create `DateTable`
* Establish relationship:

  ```
  DateTable[Date] → Sales[Date]
  ```

### Step 2: Add Slicers

* Rolling Period
* Metric Selector

### Step 3: Use Measure

* Add **Switchable Rolling Metric** to visuals

---

## ⚙️ Analytical Capabilities

This framework enables:

* Trend smoothing (short vs long windows)
* Outlier-resistant analytics (median)
* Cumulative performance tracking (YTD)
* Interactive KPI exploration
* Flexible time-series analysis

---

## 🧩 Design Principles

* **Modularity** → reusable measures
* **Abstraction** → single-measure interface
* **Scalability** → easy extension
* **Interactivity** → slicer-driven analytics
* **Semantic Clarity** → business-friendly logic

---

# Use Cases

* Executive dashboards
* Financial reporting
* Sales trend analysis
* KPI monitoring
* Time-series exploration

---

# Tools & Technologies

![Power BI](https://img.shields.io/badge/Power%20BI-F2C80F?style=for-the-badge\&logo=power-bi\&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-Analytics-blue?style=for-the-badge)
![Excel](https://img.shields.io/badge/Excel-217346?style=for-the-badge\&logo=microsoft-excel\&logoColor=white)
![SQL](https://img.shields.io/badge/SQL-00758F?style=for-the-badge\&logo=postgresql\&logoColor=white)

---

# Future Enhancements

* Rolling standard deviation (volatility)
* Z-score anomaly detection
* Forecasting integration
* Calculation Groups (Tabular Editor)
* Multi-granularity (weekly/monthly)

---

# License

MIT License

---

# Acknowledgments

* Built on Microsoft DAX engine and tabular modeling principles
* Inspired by real-world business intelligence use cases

---

# Maintainer

**George Ejembi - Analytics Engineer**
📅 Last Updated: March 2026

---
