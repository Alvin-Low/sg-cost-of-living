# Power BI DAX Measures

This document lists the key DAX measures used in the **Singapore Cost of Living** dashboard, with short explanations for clarity.

---

## Headline CPI YoY

```DAX
Headline CPI YoY =
AVERAGE ( fact_cpi_monthly[yoy_pct] )
```

**Description**
- Computes the average year-on-year CPI inflation.
- Used for general CPI trend comparison.

---

## Headline CPI YoY (Latest Month)

```DAX
Headline CPI YoY (Latest Month) =
VAR LatestMonth =
    MAX ( dim_date_month[month_key] )
VAR YoYValue =
    CALCULATE (
        AVERAGE ( fact_cpi_monthly[yoy_pct] ),
        dim_date_month[month_key] = LatestMonth
    )
RETURN
DIVIDE ( YoYValue, 100 )
```

**Description**
- Identifies the most recent month in the current filter context.
- Returns headline CPI YoY for that month only.
- Scaled to percentage format for reporting.

---

## Household-Weighted Inflation YoY

```DAX
Household-Weighted Inflation YoY =
SUM ( vw_weighted_inflation_monthly[weighted_yoy_inflation_pct] )
```

**Description**
- Aggregates household-weighted inflation values.
- Reflects cost pressures adjusted for household spending patterns.

---

## Latest Household-Weighted Inflation YoY

```DAX
Latest Household-Weighted Inflation YoY =
VAR LatestMonth =
    MAX ( dim_date_month[month_key] )
RETURN
CALCULATE (
    AVERAGE ( vw_weighted_inflation_monthly[weighted_yoy_inflation_pct] ),
    dim_date_month[month_key] = LatestMonth
)
```

**Description**
- Filters the weighted inflation index to the latest available month.
- Used for KPI card highlighting current household inflation.

---

## Usage Notes

- All measures rely on `dim_date_month` for consistent time filtering.
- Single-direction relationships are assumed.
- Measures are slicer-aware (year, month).
