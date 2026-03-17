# 📊 DAX Measures Repository

A comprehensive collection of **DAX (Data Analysis Expressions) measures** for business intelligence and analytics in **Power BI**, **Analysis Services**, and **Excel Power Pivot**. This repository provides **reusable, well-documented DAX formulas** for common analytical scenarios.

---

## 📋 Table of Contents
- [Prerequisites](#-prerequisites)  
- [Repository Structure](#-repository-structure)  
- [Date Table](#-date-table)  
- [Moving Average Measures](#-moving-average-measures)  
- [Installation](#-installation)  
- [Usage Guidelines](#-usage-guidelines)  
- [Customization](#-customization)  
- [Contributing](#-contributing)  
- [Best Practices](#-best-practices)  
- [License](#-license)  
- [Additional Resources](#-additional-resources)  
- [Acknowledgments](#-acknowledgments)  

---

## 🔧 Prerequisites

- **Tools:** Power BI Desktop, SQL Server Data Tools, or Excel Power Pivot  
- **Data Model:** Properly structured star schema with a date table relationship  
- **Tables:** At minimum, a `Sales` table with numeric values and a `DateTable`  
- **Knowledge:** Basic understanding of DAX syntax and context transition  

---

## 📁 Repository Structure
dax-measures/
│
├── README.md # Project documentation
├── date-table.dax # Date table generation script
├── moving-averages.dax # Moving average calculations
├── time-intelligence.dax # YoY, MoM, QoQ comparisons
├── financial-measures.dax # Financial KPIs (revenue, profit, margins)
└── statistical-measures.dax # Statistical calculations



---

## 📅 Date Table

The foundation for all time intelligence calculations. This table generates a comprehensive **date dimension from 2015 to 2030**.

**Key Features:**
- **Date Range:** 2015-2030 (customizable)  
- **Attributes:** Year, Month (name/number/short), Quarter, Week Number  
- **Day Properties:** Day name, short name, weekend flag  
- **Sorting Ready:** Properly formatted for chronological sorting  

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

📈 Moving Average Measures

Six variations of 12-day moving averages for different analytical needs:

| Measure Name                     | Description                             | Use Case                    |
| -------------------------------- | --------------------------------------- | --------------------------- |
| 12 Day Moving Average            | Trailing 12 days including current date | Standard rolling average    |
| 12 Day Moving Average Excl Today | Past 12 days only, excludes today       | Lagging indicator           |
| 12 Day Moving Average Sum        | Sum-based calculation (total/12)        | When you need sum context   |
| 12 Day MA Last 12 Visible        | Based on last visible date in filter    | Period-end analysis         |
| 12 Day MA By Date                | Ignores date filters for row-level      | Independent trend analysis  |
| 12 Day MA of Total Sales         | Works with existing measure             | Reuse existing calculations |



Key Differences:

ALLSELECTED vs REMOVEFILTERS: Controls whether user filters affect the calculation

-11 vs -12 days: Determines whether to include or exclude current date

AVERAGE vs SUM/12: Different calculation methods for the same result


🎯 Usage Guidelines

Measure Selection Guide:

graph TD
    A[Need Moving Average?] --> B{Include Today?}
    B -->|Yes| C[Use 12 Day Moving Average]
    B -->|No| D[Use 12 Day Moving Average Excl Today]
    C --> E{Need Exact Division?}
    E -->|Yes| F[Use 12 Day Moving Average Sum]
    E -->|No| G{Based on Measure?}
    G -->|Yes| H[Use 12 Day MA of Total Sales]
    G -->|No| I{Respect Filters?}
    I -->|Yes| J[Use 12 Day MA Last 12 Visible]
    I -->|No| K[Use 12 Day MA By Date]

📝 License

This project is licensed under the MIT License – see the LICENSE file for details.


🙏 Acknowledgments

Inspired by real-world business intelligence requirements

Built upon Microsoft DAX language specifications

Community feedback and contributions

Maintainers: [George Ejembi]
Last Updated: March 2026
