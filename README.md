# Part 3 — Regression-Based Business Insights & Model Interpretation

## 1. Business Problem Summary
A retail chain's leadership wants to understand what drives monthly sales across stores so they can act on
business levers such as marketing spend, inventory availability, discounting, staffing, and store
mix/region prioritization. This project uses regression analysis on 4 months of store-level data
(Jan–Apr 2025) to identify which factors are most strongly associated with `monthly_sales` and to turn
that into a business recommendation.

## 2. Dataset Description
- **File:** `business_regression_data.xlsx`, sheet `store_performance`
- **Shape:** 320 rows × 14 columns — 80 unique stores, each observed for 4 months (Jan, Feb, Mar, Apr 2025)
- A `data_dictionary` sheet in the source file documents every column.

## 3. Dependent and Independent Variables
- **Dependent variable:** `monthly_sales`
- **Potential independent variables:** `marketing_spend`, `footfall`, `avg_discount_pct`, `staff_count`,
  `inventory_availability_pct`, `competitor_distance_km`, `holiday_flag`, `customer_rating`, `region`,
  `store_type`

### Numerical variables
`marketing_spend`, `footfall`, `avg_discount_pct`, `staff_count`, `inventory_availability_pct`,
`competitor_distance_km`, `customer_rating`, `monthly_sales`, `monthly_profit`

### Categorical variables
`region` (East, North, South, West), `store_type` (High Street, Mall, Residential, Airport)

### Binary / flag variable
`holiday_flag` (1 = meaningful holiday effect that month, 0 = otherwise) — already numeric 0/1, so it can
be used directly in regression without creating a dummy.

### Variables that needed cleaning or transformation
- `competitor_distance_km` — 6 missing values, imputed with the column median.
- `customer_rating` — 8 missing values, imputed with the column median.
- `region` and `store_type` — converted into dummy variables (see Section 5).
- `month` — stored as a date; not used directly as a regression input (see below).

### Variables not useful for regression (as-is)
- `store_id` — a unique identifier, not a driver of sales.
- `month` — only 4 distinct values in the data; with `holiday_flag` already capturing the seasonal/holiday
  effect, the raw date adds little on its own and was excluded from the model. (Note for the
  student: with more months of history this could become a useful trend variable — flagged here as a
  limitation, not used in the current model.)
- `monthly_profit` — a related business outcome, not a driver of sales; using it as a predictor would
  reverse the cause-and-effect direction the business question is asking about.

## 4. Regression Approach
- Ran 5 **simple linear regressions** of `monthly_sales` on one predictor at a time: `footfall`,
  `marketing_spend`, `inventory_availability_pct`, `avg_discount_pct`, `customer_rating`.
- Ran 1 **multiple regression** using 6 numerical predictors (`marketing_spend`, `footfall`,
  `avg_discount_pct`, `inventory_availability_pct`, `competitor_distance_km`, `customer_rating`),
  `holiday_flag`, and dummy variables for `region` and `store_type`.
- All statistics (coefficients, standard errors, R², F-stat, p-values) were computed in Excel using
  `SLOPE`/`INTERCEPT`/`RSQ`/`DEVSQ` for simple regression and `LINEST` for multiple regression — see
  `analysis/regression_workbook.xlsx`.

## 5. Dummy Variable Approach
- `region`: 3 dummies created (`region_North`, `region_South`, `region_West`). **Reference category: East**
  (the most frequent region in the data), so each dummy coefficient is read as "vs. a typical East store."
- `store_type`: 3 dummies created (`store_Airport`, `store_Mall`, `store_Residential`). **Reference
  category: High Street** (the most frequent store type).
- Both categories were dropped (not all 4 levels kept) specifically to avoid the dummy-variable trap
  (perfect multicollinearity with the intercept).

## 6. Model Comparison Summary
See `analysis/model_comparison.md` for the full table. In short: `footfall` is the strongest single
predictor (R² ≈ 0.74 on its own); `marketing_spend` and `inventory_availability_pct` show real but smaller
standalone effects; `avg_discount_pct` and `customer_rating` are **not** statistically significant on
their own. The multiple regression model combining all factors reaches R² ≈ 0.85.

## 7. Final Model Selected
**Multiple regression model** (`monthly_sales` ~ marketing_spend + footfall + avg_discount_pct +
inventory_availability_pct + competitor_distance_km + customer_rating + holiday_flag + region dummies +
store_type dummies). It explains far more of the variation in sales (R² ≈ 0.85 vs. ≈ 0.74 for the best
simple model) and — unlike any single-variable model — lets leadership see the effect of each lever while
holding the others constant.

## 8. Business Recommendation
See `outputs/final_recommendation.md` for the full write-up. Headline: footfall and marketing spend are
the most actionable, reliable levers; inventory availability and store/region mix matter too; discounting
shows no statistically reliable sales benefit in this data.

## 9. Assumptions and Limitations
- Only 4 months of data (Jan–Apr 2025) — seasonal patterns beyond this window are not captured.
- Missing values in `competitor_distance_km` and `customer_rating` were imputed with the column median,
  which slightly understates the true variability of those variables.
- Regression shows **association**, not proof of **causation** — see `outputs/final_recommendation.md`,
  point 6, for why.
- `avg_discount_pct` and the `region_North` dummy were not statistically significant in the final model and
  should not be over-interpreted.

## 10. Screenshots Included
- `screenshots/simple_regression_output.png`
- `screenshots/multiple_regression_output.png`
- `screenshots/residuals_preview.png`
- `screenshots/model_comparison_preview.png`

*(Screenshots must be captured directly from your own Excel/LibreOffice session after opening
`regression_workbook.xlsx` — they cannot be generated for you.)*
