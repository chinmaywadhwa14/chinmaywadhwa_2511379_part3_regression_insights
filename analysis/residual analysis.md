# Residual Analysis — Final Multiple Regression Model

Predicted sales were calculated for all 320 store-month records using the final multiple regression
model's coefficients (see `regression_workbook.xlsx`, sheet `multiple_regression`). Residual = actual
`monthly_sales` − predicted sales. Full calculation for every record is in
`regression_workbook.xlsx`, sheet `prediction_residuals`.

## Top 5 Largest Positive Residuals (model *under*-predicts these store-months)
| Rank | Store | Month | Region | Store Type | Actual Sales | Predicted Sales | Residual |
|---|---|---|---|---|---|---|---|
| 1 | STR-1058 | Feb 2025 | East | High Street | 870,937 | 751,096 | +119,841 |
| 2 | STR-1028 | Apr 2025 | East | Mall | 713,611 | 609,752 | +103,859 |
| 3 | STR-1073 | Mar 2025 | East | Residential | 813,317 | 716,078 | +97,239 |
| 4 | STR-1050 | Apr 2025 | North | Residential | 735,787 | 652,811 | +82,975 |
| 5 | STR-1069 | Apr 2025 | West | High Street | 686,738 | 604,089 | +82,649 |

## Top 5 Largest Negative Residuals (model *over*-predicts these store-months)
| Rank | Store | Month | Region | Store Type | Actual Sales | Predicted Sales | Residual |
|---|---|---|---|---|---|---|---|
| 1 | STR-1023 | Feb 2025 | South | Mall | 627,172 | 772,339 | −145,167 |
| 2 | STR-1017 | Mar 2025 | West | High Street | 685,379 | 812,196 | −126,817 |
| 3 | STR-1007 | Feb 2025 | West | Mall | 800,452 | 910,234 | −109,782 |
| 4 | STR-1012 | Jan 2025 | West | Residential | 595,468 | 701,898 | −106,431 |
| 5 | STR-1068 | Jan 2025 | East | Mall | 660,634 | 749,073 | −88,439 |

## What These Residuals May Mean in Business Terms
- **Positive residuals** (actual > predicted): these stores are out-performing what their footfall,
  marketing, inventory, region, and store-type would predict. That gap is likely explained by something
  the model doesn't measure — e.g. unusually strong store management, a local event or promotion not
  flagged in `holiday_flag`, or a recent competitor closure not reflected in `competitor_distance_km`.
  These stores are worth a closer qualitative look — there may be a replicable practice here.
- **Negative residuals** (actual < predicted): these stores are under-performing relative to what their
  inputs would suggest. STR-1023 in particular is a notable outlier (−145k) — worth checking for one-off
  issues that month (e.g. stockouts not captured in the monthly average, staffing disruption, a new
  competitor opening nearby).

## Is the Model Systematically Under- or Over-Predicting Certain Store Types?
Average residual by `region` and by `store_type` is effectively **zero in every group**
(within rounding error). This is expected and is actually a good sign: because both `region` and
`store_type` are already included as dummy variables in the model, the regression automatically forces
the average error within each group to zero — it has already "absorbed" any flat group-level advantage
or disadvantage.

The practical takeaway: the large individual residuals above are **not** a sign that the model is biased
against (or in favor of) a particular region or store format as a whole. They reflect store-specific,
month-specific factors that aren't in the dataset — which is exactly the kind of nuance a regression
model, by design, cannot fully capture and a manager's local knowledge can.
