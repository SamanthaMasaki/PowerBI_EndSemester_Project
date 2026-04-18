# PowerBI End Semester Project

A professional Business Intelligence project built in Power BI to analyze restaurant sales performance, highlight operational risks, and support data-driven decisions.

## Table of Contents

1. [Project Title](#1-project-title)
2. [Student Name and Admission Number](#2-student-name-and-admission-number)
3. [Course Code and Class](#3-course-code-and-class)
4. [Project Overview](#4-project-overview)
5. [Problem Statement](#5-problem-statement)
6. [Dataset Description](#6-dataset-description)
7. [Tools Used](#7-tools-used)
8. [Steps Followed](#8-steps-followed)
9. [Dashboard Features](#9-dashboard-features)
10. [Main Visualizations](#main-visualizations)
11. [Key DAX Measures](#10-key-dax-measures)
12. [Key Insights](#11-key-insights)
13. [Challenges Encountered](#12-challenges-encountered)
14. [Conclusion](#13-conclusion)
15. [Repository Assets](#repository-assets)

---

## 1. Project Title

**PowerBI End Semester Project: Restaurant Sales Analytics and Performance Monitoring**

## 2. Student Name and Admission Number

- **Student Name:** Samantha Nyatichi Masaki  
- **Admission Number:** 670455

## 3. Course Code and Class

- **Course Code:** DSA 3050A  
- **Class:** SS 2026

## 4. Project Overview

This project delivers an end-to-end Power BI analytical solution for a restaurant sales environment. It covers:

- data acquisition and profiling
- data cleaning and transformation in Power Query
- dimensional modeling using a star schema
- DAX measure development for business KPIs
- interactive dashboard design across three report pages
- analytical interpretation and recommendations

The final output is an executive-ready dashboard that enables management to monitor revenue performance, product category behavior, and payment trends.

## 5. Problem Statement

The restaurant lacked clear visibility into transaction-level performance, making it difficult to:

- identify high-performing product categories and items
- monitor revenue trends over time
- understand customer payment preferences
- detect data quality issues affecting business decisions

The objective was to build a robust BI solution that transforms raw sales data into actionable insights.

## 6. Dataset Description

**Source:** Kaggle - Restaurant Sales Dataset with Dirt  
**Kaggle Link:** https://www.kaggle.com/datasets/ahmedmohamed2003/restaurant-sales-dirty-data-for-cleaning-training

### Data Files in This Repository

- `Data/restaurant_sales_data.csv` (raw/combined view)
- `Data/Order Table.csv` (order-level table)
- `Data/Customer Table.csv` (line-item/customer table)

### Size and Scope

- **Rows:** 17,534 transactional records (per table export)
- **Main dimensions represented:** customer, product category/item, date, payment method
- **Fact metrics represented:** order total, price, quantity

### Core Fields

- **Order-level:** Order ID, Order Total, Order Date, Payment Method
- **Line-item/customer-level:** Customer ID, Category, Item, Price, Quantity, Order ID

### Data Quality Characteristics

The dataset intentionally contains realistic issues for transformation practice, including:

- missing values
- inconsistent text/categorical entries
- whitespace noise
- potential invalid or incomplete records

## 7. Tools Used

- **Power BI Desktop** (data modeling, DAX, visualization)
- **Power Query** (cleaning and transformation)
- **DAX** (measures and calculated columns)
- **Python (Pandas)** for initial dataset split into logical tables
- **GitHub** for version control and project documentation

## 8. Steps Followed

1. **Data Acquisition**
	- Selected a restaurant sales dataset suitable for advanced analytics.
2. **Data Understanding**
	- Profiled columns, field types, and business meaning.
3. **Table Preparation**
	- Split the raw dataset into `Order Table` and `Customer Table`.
4. **Data Cleaning in Power Query**
	- addressed missing values (for Item, Payment Method, Price, Quantity)
	- removed duplicates
	- standardized text fields
	- corrected data types
	- extracted Year, Month, and Day Name from date
5. **Data Modeling**
	- Built a star schema with a central fact table and dimension tables.
	- Configured 1:* relationships with single filter direction.
6. **DAX Development**
	- Built calculated columns and measures for KPI reporting and time intelligence.
7. **Dashboard Design**
	- Created three report pages for executive, analytical, and insight-focused consumption.
8. **Insight Generation**
	- Interpreted trends, concentration risks, and payment behavior.
9. **Recommendation Framing**
	- Proposed actions on data quality, promotions, and menu strategy.

## 9. Dashboard Features

The report is designed as a **3-page interactive dashboard**:

- **Page 1: Executive Summary**
  - KPI cards
  - trend chart
  - category comparison visuals
  - global slicers

- **Page 2: Detailed Analysis**
  - matrix/table analysis
  - hierarchy drill-down from category to item
  - breakdown-focused visuals

- **Page 3: Insights and Performance Monitoring**
  - insight-oriented visuals for trend and composition interpretation
  - business recommendation context

### Interaction Design

- synchronized slicers (Date, Payment Method) across pages
- cross-filtering between visuals
- hover tooltips for exact quantitative values
- layout optimized for left-to-right executive reading

## Main Visualizations

### Visualization 1: Executive Summary Dashboard

![Executive Summary Dashboard](Screenshots/Screenshot%202026-04-18%20130830.png)

This view consolidates high-level KPIs and trend visuals, including total revenue, total orders, average order value, category performance, and payment method distribution.

### Visualization 2: Detailed Analysis Dashboard

![Detailed Analysis Dashboard](Screenshots/Screenshot%202026-04-18%20130907.png)

This page supports deep-dive analysis through category drill-downs, item-level performance comparisons, revenue and order relationships, and matrix-based monthly category monitoring.

### Visualization 3: Insights and Performance Monitoring Dashboard

![Insights and Performance Monitoring Dashboard](Screenshots/Screenshot%202026-04-18%20130945.png)

This view highlights time-intelligence behavior and explanatory analytics, combining YTD trends, category order volumes, prior-period item performance, and influencer analysis.

## 10. Key DAX Measures

### Calculated Columns

1. **Transaction Size**

```DAX
Transaction Size =
SWITCH(
	 TRUE(),
	 FactSales[Order Total] < 20, "Small",
	 FactSales[Order Total] <= 50, "Medium",
	 "Large"
)
```

2. **Is Weekend**

```DAX
Is Weekend = IF(WEEKDAY(FactSales[Order Date], 2) >= 6, "Weekend", "Weekday")
```

### Measures

1. **Total Revenue**

```DAX
Total Revenue = SUM(FactSales[Order Total])
```

2. **Total Orders**

```DAX
Total Orders = DISTINCTCOUNT(FactSales[Order ID])
```

3. **Average Order Value**

```DAX
Average Order Value = DIVIDE([Total Revenue], [Total Orders], 0)
```

4. **YTD Revenue**

```DAX
YTD Revenue = TOTALYTD([Total Revenue], DimDate[Date])
```

5. **Previous Month Revenue**

```DAX
Previous Month Revenue =
CALCULATE(
	 [Total Revenue],
	 DATEADD(DimDate[Date], -1, MONTH)
)
```

6. **MoM Revenue Growth %**

```DAX
MoM Revenue Growth % =
DIVIDE(
	 [Total Revenue] - [Previous Month Revenue],
	 [Previous Month Revenue],
	 0
)
```

7. **% of Total Revenue**

```DAX
% of Total Revenue =
DIVIDE(
	 [Total Revenue],
	 CALCULATE([Total Revenue], ALL(DimProduct[Category])),
	 0
)
```

8. **Category Rank**

```DAX
Category Rank =
RANKX(
	 ALL(DimProduct[Category]),
	 [Total Revenue],
	 ,
	 DESC,
	 DENSE
)
```

## 11. Key Insights

1. **Revenue Decline Across Years**  
	Total revenue decreased from approximately **175K (2022)** to about **166K (2023)**.

2. **Flat Daily Demand Pattern**  
	Revenue is evenly distributed through the week, with no strong weekend uplift.

3. **Category Dominance**  
	**Main Dishes** is the leading revenue category (over **95K**), clearly outperforming others.

4. **Item Concentration Risk + Data Quality Signal**  
	Revenue in Main Dishes is highly concentrated in a small number of items (notably Grilled Chicken and Pasta Alfredo), while **Unknown** items contribute a significant share.

5. **Payment Method Parity**  
	Cash, Credit Card, and Digital Wallet usage are nearly balanced, indicating no dominant payment preference.

## 12. Challenges Encountered

1. **Missing and dirty values** in key fields (Item, Payment Method, Price, Quantity).
2. **Text inconsistencies and formatting noise**, requiring standardization.
3. **Model design decisions** around relationship direction and cardinality for stable filtering behavior.
4. **Time-intelligence readiness**, which required robust date handling and proper date table linkage.
5. **Data reliability concerns**, especially high contribution from Unknown item labels.

## 13. Conclusion

This project demonstrates a full BI workflow from raw transactional data to strategic decision support. By combining Power Query transformation, star-schema modeling, DAX-driven KPI design, and interactive dashboard storytelling, the solution exposed meaningful operational patterns and risks.
The revenue decline and flat demand pattern suggest the need for targeted promotional efforts, while the category concentration and data quality issues highlight operational risks that must be addressed to ensure accurate reporting and sustainable growth.

The analysis indicates three immediate management priorities:

- strengthen POS data capture quality controls
- introduce weekend-focused promotional strategies
- diversify the menu mix to reduce over-dependence on a few top-selling items

With these actions, the restaurant can improve reporting accuracy, reduce operational risk, and support sustainable revenue growth.

---

## Repository Assets

- `PowerBI Results.pbix` - final Power BI report
- `End-Sem-Report.pdf` - full project report and evidence
- `Data/` - raw and prepared datasets
- `Screenshots/` - dashboard and transformation evidence


---
## License
This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details
