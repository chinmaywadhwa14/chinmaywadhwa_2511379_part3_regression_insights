# Model Comparison — Part 3 Regression Analysis

All models predict `monthly_sales`. R² and p-values below are taken directly from
`regression_workbook.xlsx` (sheets `simple_regression` and `multiple_regression`).

| Model | Variables Used | R² | Significant Variables (p < 0.05) | Business Usefulness | Limitations |
|---|---|---|---|---|---|
| Simple 1 | DV: monthly_sales · IV: footfall | 0.736 | footfall (p < 0.001) | Strongest single predictor; useful as an early-warning signal, but footfall itself isn't a lever leadership can pull directly. | Ignores marketing, inventory, store mix, region, seasonality. |
| Simple 2 | DV: monthly_sales · IV: marketing_spend | 0.167 | marketing_spend (p < 0.001) | Confirms spend↔sales link, but explains a small share of variation alone. | Doesn't control for footfall; spend may be confounded with naturally busier stores. |
| Simple 3 | DV: monthly_sales · IV: inventory_availability_pct | 0.013 | inventory_availability_pct (p = 0.041, borderline) | Directionally right (more stock → more sales) but very weak in isolation. | Tiny R²; not reliable as a standalone story. |
| Simple 4 | DV: monthly_sales · IV: avg_discount_pct | 0.008 | None (p = 0.104) | Cannot be used to justify or reject discounting on its own. | Effect not statistically distinguishable from zero. |
| Simple 5 | DV: monthly_sales · IV: customer_rating | 0.001 | None (p = 0.638) | Essentially no standalone link to *monthly* sales. | May still matter for loyalty/retention — just not captured here. |
| **Multiple (FINAL)** | DV: monthly_sales · IV: marketing_spend, footfall, avg_discount_pct, inventory_availability_pct, competitor_distance_km, customer_rating, holiday_flag, region (3 dummies), store_type (3 dummies) | **0.853** (Adj. R² 0.847) | marketing_spend, footfall, inventory_availability_pct, competitor_distance_km, holiday_flag, customer_rating, region_South, region_West, store_Airport, store_Mall, store_Residential | Best fit by far; isolates each factor's effect while holding the others constant — the model leadership should actually use for decisions. | `avg_discount_pct` and `region_North` are *not* significant here (p = 0.278 and p = 0.102) and should not be over-interpreted; based on only 4 months of data; shows association, not causation. |

## Why the multiple regression model wins
- It explains ~85% of month-to-month sales variation vs. ~74% for the best single-variable model.
- It controls for confounding: e.g., `marketing_spend`'s effect on its own (R² = 0.167) could partly
  reflect that better-located stores both get more footfall *and* more marketing budget. The multiple
  model separates these effects — `marketing_spend` (coef ≈ ₹1.23 in sales per ₹1 spent) and `footfall`
  (coef ≈ ₹33.68 per visitor) both remain significant even after controlling for each other.
- It directly answers the business question by quantifying region and store-type effects via dummy
  variables, which no simple regression can do.

## Where the multiple model still falls short
- `avg_discount_pct` is not significant — the data doesn't support strong conclusions either way about
  discounting's effect on sales.
- `region_North` is not significant — North stores don't reliably differ from East stores once other
  factors are controlled for.
- R² ≈ 0.85 still leaves ~15% of variation unexplained — see `residual_analysis.md` for what that
  unexplained part looks like at the individual-store level.
