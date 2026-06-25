## Which factors appear most strongly associated with monthly sales?

Based on the final multiple regression model (R² = 0.82):

1. **Footfall** — by far the strongest association with sales (largest t-stat, p ≈ 1.1e-102).
   Every extra shopper is associated with roughly ₹33.66 in additional monthly sales.
2. **Marketing spend** — a clear, statistically significant positive association (p ≈ 1.4e-17).
3. **Inventory availability** — a clear, statistically significant positive association
   (p ≈ 4.7e-10). Stores that keep products in stock sell more.
4. **Store format** — Airport stores significantly outperform High Street stores, while
   Residential-area stores significantly underperform them, holding other factors constant.

## Which variables should leadership focus on?

- **Footfall-driving initiatives** (location decisions, store visibility, local promotions that
  bring people through the door) — the single biggest lever in the data.
- **Marketing spend** — has a real, measurable, controllable relationship with sales.
- **Inventory availability** — keeping shelves stocked is strongly associated with higher sales,
  and is one of the most directly operational levers leadership controls day to day.
- **Store format strategy** — the data supports prioritizing Airport locations and treating
  Residential locations cautiously when considering new sites or reallocating investment, all else
  equal.

## Which variables should not be over-interpreted?

- **avg_discount_pct** — in both the simple regression (p = 0.104) and the multiple regression
  (p = 0.110), discounting shows **no statistically significant relationship** with monthly sales.
  Leadership should not conclude either "discounts boost sales" or "discounts hurt sales" from this
  data — the relationship is too weak/noisy to support either claim.
- **store_type_Mall** — borderline significance (p = 0.083). There is a plausible signal that Mall
  stores outsell High Street stores, but it falls just short of the conventional 5% significance
  threshold, so it should be treated as a hypothesis to monitor rather than a confirmed effect.
- **region** — tested but excluded from the final model because it added little explanatory power.
  Residual analysis shows mild regional patterns (East slightly over-predicted, South/West slightly
  under-predicted), so region-level differences likely exist but are not well captured by this
  model and need further investigation (e.g. local competition, demographics).

## What business action would you recommend?

1. **Protect and grow footfall-driving spend** (location strategy, local marketing, store
   visibility) before considering cuts, since footfall has the strongest measured association with
   sales.
2. **Maintain or increase marketing spend** where budgets allow — the relationship is statistically
   robust, though the size of the effect (≈₹1.17 per currency unit spent) means returns should be
   weighed against cost.
3. **Prioritize inventory availability as an operational KPI.** Stockouts have a measurable,
   significant negative relationship with sales; closing inventory gaps is a low-risk, high-confidence
   lever.
4. **Reassess discounting strategy independently of sales-volume assumptions.** Since the data does
   not support a reliable link between discount depth and sales, discounting decisions should be
   evaluated on margin and customer-acquisition grounds rather than an assumed sales lift.
5. **Favor Airport-format expansion and reconsider Residential-format investment**, holding other
   factors constant — but validate with additional site-level data before committing capital, given
   the relatively small sample size for some store types.

## What risks or limitations should leadership keep in mind?

- The dataset covers 80 stores over 4 months (320 store-months) — a useful but limited sample,
  especially for less common store types (e.g. only 28 Airport-month observations).
- `avg_discount_pct` and `region` are weak predictors **in this dataset**; that does not prove they
  are irrelevant to the business generally, only that this analysis could not detect a reliable
  effect.
- The model explains 82% of variation, leaving 18% driven by factors not captured here (seasonality
  beyond the 4 months observed, local competition, weather, one-off events, management quality,
  etc. — see `analysis/residual_analysis.md`).
- `staff_count` was deliberately excluded from the final model because it is highly correlated with
  `footfall` (r ≈ 0.92); including both would have made their individual effects statistically
  unstable and harder to interpret cleanly.

## Why does regression show association but not automatically prove causation?

Regression measures **statistical association** — how strongly variables move together — not
**causation**. Several reasons the multiple regression model cannot prove cause-and-effect on its
own:

- **Reverse causality is plausible.** Higher footfall is associated with higher sales, but it is
  also possible that successful, high-sales stores attract more foot traffic (e.g. through
  reputation or local buzz), not just the other way around.
- **Confounding variables.** Store location quality could independently drive both higher footfall
  *and* higher marketing spend (if successful stores get bigger budgets), making the marketing
  effect look stronger or weaker than it truly is.
- **No experimental control.** This is observational data — no stores were randomly assigned higher
  or lower marketing spend, footfall, or inventory levels. Without random assignment or a controlled
  test, we cannot rule out that some unmeasured factor (e.g. local management quality) is driving
  both the "cause" and the "effect" variables together.
- **Coefficients describe historical patterns, not guarantees.** Even a statistically significant
  coefficient describes what was associated with sales in this 4-month window for these 80 stores;
  it does not guarantee that changing one variable in the future will produce the same change in
  sales, especially outside the range of values observed in this data.

**Recommendation:** treat this regression as strong evidence to guide priorities and pilot tests
(e.g. trial increased marketing spend or inventory investment in a controlled subset of stores and
measure the actual change), rather than as proof that any single lever, on its own, will
mechanically produce a specific increase in sales.

