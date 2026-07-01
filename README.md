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

### Database DDL & Data Cleaning Scripts (MySQL)

Before building any charts, the raw data was pretty messy and spread out. I imported everything into MySQL Workbench first to build a solid foundation and clean up the records properly before moving into Power BI. 

Here is what I did on the SQL backend:
* **Setting up the Tables:** I created two distinct tables—one master table for the unique athlete profiles and an injury logs table. I linked them together using the `AthleteID` so that the database stayed organized and wouldn't allow corrupted or mismatched records.
* **Cleaning and Fixing the Numbers:** I wrote update queries to clean up the text formatting, remove accidental extra spaces.


Below are the core SQL scripts executed within MySQL Workbench to establish the relational schema and clean the datasets prior to Power BI ingestion.

```sql
-- 1. DATABASE & SCHEMA SETUP
CREATE DATABASE IF NOT EXISTS AthleteX_Care;

USE AthleteX_care;

-- Create Master Athlete Dimension Table

CREATE TABLE IF NOT EXISTS Athletes (
	AthleteID VARCHAR(10) PRIMARY KEY,
    PlayerName VARCHAR(100),
    SportEvent VARCHAR(50),
    Country VARCHAR(50),
    Age INT
);

-- Create Injury Logs Fact Table

CREATE TABLE IF NOT EXISTS InjuryLogs (
	LogID VARCHAR(10) PRIMARY KEY,
    AthleteID VARCHAR(10),
    InjuryDetails VARCHAR(100),
    Severity VARCHAR(20),
    Status VARCHAR(50),
    InjuryDate DATE,
    RecoveryDate DATE,
    TreatmentCost DECIMAL(10, 2),
    TreatmentType VARCHAR(50)
);

-- 2. DATA CLEANING & STANDARDIZATION

SELECT * FROM Athletes LIMIT 5;
SELECT * FROM InjuryLogs LIMIT 5;


SELECT AthleteID, COUNT(*) as Row_Count
FROM Athletes
GROUP BY AthleteID
HAVING COUNT(*) > 1;


SELECT LogID, COUNT(*) as Row_Count
FROM InjuryLogs
GROUP BY LogID
HAVING COUNT(*) > 1;


SELECT DISTINCT SportEvent FROM Athletes;

SELECT DISTINCT Status FROM InjuryLogs;

SELECT LogID, AthleteID, InjuryDate, RecoveryDate
FROM InjuryLogs
WHERE RecoveryDate < InjuryDate;
```

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
