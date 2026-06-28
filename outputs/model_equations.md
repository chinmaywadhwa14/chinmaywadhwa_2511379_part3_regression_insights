# Model Equations and Selection Rationale

## 1. Simple Regression Equations
*(coefficients rounded; see `regression_workbook.xlsx`, sheet `simple_regression`, for full precision)*

1. `monthly_sales = 446,411 + 35.68 × footfall`
2. `monthly_sales = 560,777 + 2.13 × marketing_spend`
3. `monthly_sales = 484,814 + 2,217.83 × inventory_availability_pct`
4. `monthly_sales = 697,836 − 138,730.45 × avg_discount_pct` *(not statistically significant, p = 0.10)*
5. `monthly_sales = 701,367 − 5,284.87 × customer_rating` *(not statistically significant, p = 0.64)*

## 2. Multiple Regression Equation (Final Model)
```
monthly_sales =
    83,221
  + 1.23   × marketing_spend
  + 33.68  × footfall
  − 37,240 × avg_discount_pct            (not significant, p = 0.28)
  + 3,006  × inventory_availability_pct
  − 3,367  × competitor_distance_km
  + 14,176 × holiday_flag
  + 11,908 × customer_rating
  + 10,896 × region_North                (not significant, p = 0.10)
  + 20,358 × region_South
  + 25,193 × region_West
  + 25,124 × store_Airport
  + 12,781 × store_Mall
  − 19,072 × store_Residential
```

## 3. Explanation of Each Coefficient (Business Language)
- **marketing_spend (+1.23):** every extra ₹1 spent on marketing is associated with about ₹1.23 more in
  monthly sales — a small but positive return, holding everything else (including footfall) constant.
- **footfall (+33.68):** every additional visitor in a month is associated with about ₹33.68 more in
  sales — by far the single biggest lever in the model.
- **avg_discount_pct (−37,240, not significant):** the direction (more discount → lower sales) matches
  intuition, but the data doesn't support this confidently — it could be noise.
- **inventory_availability_pct (+3,006):** for every 1-point increase in product availability, sales rise
  by about ₹3,006 — keeping shelves stocked pays off.
- **competitor_distance_km (−3,367):** stores *closer* to the original median actually do slightly
  better as distance increases is associated with *lower* sales in this data — read this one with
  caution; it may reflect that stores near competitors are in busier commercial areas overall rather than
  a direct causal effect of distance.
- **holiday_flag (+14,176):** months with a meaningful holiday effect see about ₹14,176 more in sales on
  average, controlling for everything else.
- **customer_rating (+11,908):** once other factors are controlled for, higher-rated stores do show a
  positive link to sales (note this flips from "not significant" in the simple model — a sign that
  customer rating's effect was being masked by other variables when looked at alone).

## 4. Explanation of Dummy Variables and Reference Category
- **region:** `region_North`, `region_South`, `region_West` were created. **East is the reference
  category** (dropped). Each coefficient is read as "vs. a typical East-region store, holding all other
  factors equal." E.g. `region_West (+25,193)` means West stores sell ~₹25,193 more per month than an
  otherwise-identical East store.
- **store_type:** `store_Airport`, `store_Mall`, `store_Residential` were created. **High Street is the
  reference category** (dropped). E.g. `store_Residential (−19,072)` means Residential stores sell
  ~₹19,072 less per month than an otherwise-identical High Street store.
- Both reference categories were chosen because they are the **most frequent** category in the dataset
  (East: 104/320 rows; High Street: 116/320 rows), which keeps the comparison grounded in the most
  "typical" store.
- One category per group was always dropped to avoid the dummy-variable trap (perfect multicollinearity).

## 5. Final Model Selected
The **multiple regression model** above.

## 6. Reason for Selecting This Final Model
- Highest explanatory power: R² ≈ 0.85 vs. ≈ 0.74 for the best simple model (footfall alone).
- It is the only model that can separate the effects of correlated factors (e.g. marketing spend and
  footfall move together in raw data; the multiple model isolates each one's independent contribution).
- It directly answers the business question — which factors matter, and by how much — across every lever
  leadership mentioned: marketing, inventory, discounting, region, and store format.

## 7. Variables That Are Statistically Weak or Difficult to Interpret
- **avg_discount_pct (p = 0.28):** not significant; don't use this model to claim discounting helps or
  hurts sales.
- **region_North (p = 0.10):** borderline; North stores are not reliably different from East stores once
  other factors are controlled for.
- **competitor_distance_km:** statistically significant, but the negative sign is counter-intuitive and
  likely reflects a confound (e.g., store location density) rather than a direct causal driver — flagged
  here as a variable that needs a non-statistical, on-the-ground explanation before acting on it.
