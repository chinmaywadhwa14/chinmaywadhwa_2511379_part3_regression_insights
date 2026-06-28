# Final Business Recommendation

## 1. Which factors appear most strongly associated with monthly sales?
In order of strength (based on the final multiple regression model, R² ≈ 0.85):
1. **Footfall** — the single largest driver (coef ≈ ₹33.68 in sales per visitor).
2. **Marketing spend** — a smaller but reliable positive effect (coef ≈ ₹1.23 per ₹1 spent).
3. **Inventory availability** — keeping products in stock is positively associated with sales.
4. **Store format and region** — Mall and Airport stores, and West/South regions, are associated with
   meaningfully higher sales than the High Street/East baseline; Residential stores are associated with
   lower sales.
5. **Holiday periods and customer rating** — both show a smaller but statistically real positive link.

## 2. Which variables should leadership focus on?
- **Driving footfall** (e.g. store placement, local visibility, footfall-driving promotions) — the
  biggest lever in the data, even though it's the hardest to engineer directly.
- **Marketing spend** — has a confirmed positive return; can be scaled with reasonable confidence.
- **Inventory availability** — a clear, controllable operational lever: reducing stockouts is associated
  with higher sales.
- **Store format / regional mix** when planning new locations or renovations — Mall and Airport formats,
  and West/South regions, outperform in this dataset.

## 3. Which variables should not be over-interpreted?
- **avg_discount_pct** — not statistically significant in either the simple or multiple model. The data
  does not support strong claims that discounting increases (or decreases) sales.
- **region_North** — not significantly different from the East baseline; don't treat North as
  meaningfully better or worse than East based on this model.
- **competitor_distance_km** — significant, but the direction is counter-intuitive; treat this as a
  signal to investigate further on the ground rather than a lever to act on directly.

## 4. What business action would you recommend?
- Prioritize investments that grow **footfall** (location selection, local marketing, signage/visibility)
  and protect **inventory availability** (tighter stock-replenishment processes) — both are reliable,
  statistically supported levers.
- Continue **marketing spend** at a measured pace, since the data shows a real (if modest) return.
- Use the **store-format and region findings** to inform where to open new stores or renovate existing
  ones — Mall/Airport formats and West/South regions look most promising in this 4-month window.
- **Do not change discounting strategy based on this analysis alone** — run a more targeted experiment
  (e.g. an A/B test on discount levels) before making a call, since the current data doesn't show a
  reliable effect either way.

## 5. What risks or limitations should leadership keep in mind?
- Only 4 months of data (Jan–Apr 2025) — this may not capture full seasonal cycles.
- Missing values in `competitor_distance_km` and `customer_rating` were filled with the column median,
  which can understate the true relationship for those variables.
- R² ≈ 0.85 still leaves ~15% of month-to-month sales variation unexplained by this model (see
  `residual_analysis.md`) — individual store performance can still swing for reasons outside this dataset.
- The model is a **snapshot of association in this period**, not a guaranteed predictor of future
  performance if market conditions change.

## 6. Why does regression show association but not automatically prove causation?
Regression measures how strongly variables move together *in the data we have* — it cannot, on its own,
rule out the possibility that:
- A third, unmeasured factor is driving both the predictor and `monthly_sales` at the same time (e.g.
  stores in busier commercial areas may naturally get both more footfall *and* more marketing budget,
  making it hard to fully separate their individual effects from "this store/location is just stronger").
- The relationship runs in the opposite direction in part — e.g. management might increase marketing
  spend *because* a store is already trending up, rather than the spend causing the increase.
- The relationship only holds within the range and time period actually observed (Jan–Apr 2025) and may
  not generalize to a different season, region, or larger marketing-spend increase than what's in the
  data.

To move from association to a causal claim, the business would typically need a controlled experiment
(e.g. randomly varying marketing spend or discount levels across comparable stores) rather than relying
on historical regression alone.
