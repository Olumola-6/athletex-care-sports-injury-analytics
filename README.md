# AthleteX Care: Sports Medicine Performance & Injury Analytics Platform

A Power BI dashboard for sports medicine departments to track injury history, monitor treatment costs, and flag high-risk athletes.

---

## Why This Project Matters

In professional sports, player availability directly impacts team performance and financial stability. This platform bridges the gap between raw medical logs and operational decision-making. It enables sports organizations to track injury frequencies, audit treatment costs, monitor recovery timelines, and evaluate athlete re-injury risks.

---

## What This Project Does

### Core Features

- **Financial Cost Tracking** – Aggregates treatment costs across sports categories to highlight spending trends and resource allocation
- **Injury Severity Monitoring** – Breaks down injury occurrences by severity (Minor, Moderate, Severe) to help coaching staff adjust training loads
- **Athlete Risk Classification** – Flags athletes as High, Medium, or Low Risk based on historical multi-injury records before they are cleared for full-contact training
- **Squad Availability Benchmarking** – Provides visibility into historical recovery averages to help technical directors benchmark return-to-play timelines

---

## Data Architecture & Modeling

The platform uses a normalized relational database structure with a Star Schema design.

### Tables

Dim_Athletes (Dimension Table)
- AthleteID (Primary Key)
- Full Name
- Age
- Country
- Sport Type

Fact_InjuryLogs (Fact Table)
- LogID (Primary Key)
- AthleteID (Foreign Key)
- Injury Date
- Body Part
- Severity (Minor/Moderate/Severe)
- Recovery Days
- Treatment Cost

### Relationship

Dim_Athletes[AthleteID] → Fact_InjuryLogs[AthleteID] (One-to-Many)

---

## Dashboard Layout

The dashboard contains four main sections:

1. **Executive Summary Cards** – Total treatment cost, severe injury count, average recovery days
2. **Athlete Risk Matrix** – Grid showing each athlete, injury history, and risk classification (High/Medium/Low)
3. **Monthly Injury Trends** – Stacked column chart showing injury volume by month and severity
4. **Cost by Sport** – Horizontal bar chart showing medical spending distribution across sports



## Visuals
![AthleteX_dashboard](AthleteX%20Dashboard.png)
---

## Key Insights Derived From The Data

| Finding | Insight |
|---------|---------|
| High-Risk Outliers | Athletes with multiple severe injury logs flagged as High Risk → justifies extended rehabilitation protocols |
| Departmental Variances | Tennis and Football account for majority of medical costs → suggests need for targeted physiotherapy programs in those sports |
| Seasonal Patterns | Visible injury spikes during mid-season competitive phases → coaching staff can implement rotational strategies |

---

## Tools Used

- **Data Preparation:** Power Query (data type auditing, schema alignment, date standardization)
- **Data Modeling:** Power BI Desktop / DAX (measures, calculated columns, Star Schema)
- **Reporting:** Power BI Executive Layout (custom containers, cross-filtering, responsive grids)

---
