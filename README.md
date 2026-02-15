# sg-cost-of-living

Singapore cost-of-living snapshot analytics using **Azure Data Factory + Microsoft Fabric + Power BI**.

This project demonstrates an end-to-end analytics workflow:
- Raw CPI datasets sourced from **data.gov.sg**
- Transformation and modelling in **Microsoft Fabric**
- Analytical measures and visual storytelling in **Power BI Desktop**

The focus is on comparing **headline CPI inflation** against a **household-weighted inflation index**, highlighting how official inflation can differ from lived household experience.

---

## What This Dashboard Shows

The Power BI dashboard answers three core questions:

1. **How is headline CPI inflation trending over time?**
   - Monthly YoY CPI movements using official headline CPI.

2. **How does household-weighted inflation compare to headline CPI?**
   - A weighted inflation index based on household expenditure weights (HEXP).

3. **Which categories are driving inflation in the latest month?**
   - Top 5 CPI categories by YoY inflation for the most recent reporting month.

Together, these visuals provide a clearer view of cost-of-living pressures faced by households.

---

## Power BI Model Overview

The Power BI semantic model follows a simple star-style design:

### Dimension Tables
- **dim_date_month**
  - Month-level calendar dimension (month_key, year, month_name)
- **dim_price_category**
  - CPI category metadata

### Fact / View Tables
- **fact_cpi_monthly**
  - Monthly CPI values and YoY changes by category
- **vw_weighted_inflation_monthly**
  - Pre-aggregated household-weighted YoY inflation by month

Relationships:
- `dim_date_month[month_key]` → fact tables
- `dim_price_category[category_key]` → `fact_cpi_monthly`

Cross-filter direction is single, ensuring clean and predictable aggregation.

---

## Key DAX Measures (Summary)

The dashboard is driven by a small set of focused DAX measures:

- **Headline CPI YoY**
  - Average YoY CPI across categories

- **Headline CPI YoY (Latest Month)**
  - YoY CPI filtered to the most recent month in context

- **Household-Weighted Inflation YoY**
  - Aggregated weighted inflation based on household expenditure

- **Latest Household-Weighted Inflation YoY**
  - Weighted inflation for the latest available month

Full DAX definitions with comments are available in:
➡️ **measures.md**

---

## Repository Structure

```
sg-cost-of-living/
│
├─ data/
│  └─ raw_snapshot/          # Raw CPI datasets from data.gov.sg
│
├─ powerbi/
│  └─ sg_cost_of_living_weighted_inflation.pbix
│
├─ docs/
│  ├─ dashboard_overview.png
│  └─ top5_categories.png
│
├─ measures.md               # Documented DAX measures
├─ README.md
└─ LICENSE
```

---

## Notes

- Fabric Lakehouse tables and views are **not exported** to GitHub.
- This repository focuses on **reproducibility, logic, and portfolio clarity**, not raw platform artefacts.
- Screenshots are included to allow reviewers to understand the dashboard without opening Power BI.

---

## License

MIT License
