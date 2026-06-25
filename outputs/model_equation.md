## Dummy Variable Approach

Two categorical variables exist in the dataset: `region` (East, North, South, West) and
`store_type` (High Street, Mall, Residential, Airport).

For each categorical variable, one category is **dropped** and used as the reference category, to
avoid the "dummy variable trap" (perfect multicollinearity that would occur if every category were
included alongside an intercept).

- **region** → reference category = **East**. Dummies created: `region_North`, `region_South`,
  `region_West`.
- **store_type** → reference category = **High Street**. Dummies created: `store_type_Mall`,
  `store_type_Residential`, `store_type_Airport`.

East and High Street were chosen as reference categories because they are the most common category
in the dataset, which makes the intercept easiest to interpret as "a typical High Street store in
the East region, with predictor values of zero."


## Simple Regression Equations

**Model 1:**
```
monthly_sales = 446,410.58 + 35.68 × footfall
```
R² = 0.7363. Each additional shopper (footfall unit) is associated with ₹35.68 more in monthly
sales, holding nothing else constant.

**Model 2:**
```
monthly_sales = 560,777.35 + 2.13 × marketing_spend
```
R² = 0.1672. Each additional currency unit of marketing spend is associated with ₹2.13 more in
monthly sales, holding nothing else constant.

**Model 3:**
```
monthly_sales = 697,835.63 − 138,730.45 × avg_discount_pct
```
R² = 0.0083 and **not statistically significant** (p = 0.104) — this equation should not be
used for business decisions; it is included to show that discounting alone has no reliable
standalone relationship with sales in this data.

## Multiple Regression Equation (Final Model)

```
monthly_sales = 136,913.65
                + 1.165 × marketing_spend
                + 33.66  × footfall
                + 3,001.20 × inventory_availability_pct
                − 58,920.81 × avg_discount_pct
                + 11,433.46 × store_type_Mall
                − 16,575.85 × store_type_Residential
                + 21,905.71 × store_type_Airport
```
R² = 0.8214, Adjusted R² = 0.8174, F = 204.98 (p ≈ 1.1e-112)

### Coefficient explanations (business language)

| Coefficient | Meaning |
|---|---|
| **Intercept (136,913.65)** | Expected monthly sales for a High Street store in any region, with marketing spend, footfall, inventory availability, and discount all at zero. This is a mathematical anchor point, not a realistic real-world scenario. |
| **marketing_spend (+1.165)** | For every extra currency unit spent on marketing (holding footfall, inventory, discount, and store type constant), monthly sales increase by about ₹1.17. Statistically significant (p < 0.001). |
| **footfall (+33.66)** | For every extra shopper visiting the store per month, sales increase by about ₹33.66. The single strongest driver in the model. Statistically significant (p < 0.001). |
| **inventory_availability_pct (+3,001.20)** | For every 1-percentage-point increase in inventory availability (e.g. going from 80% to 81% in-stock), monthly sales increase by about ₹3,001. Statistically significant (p < 0.001). |
| **avg_discount_pct (−58,920.81)** | For every 1-percentage-point increase in average discount, sales fall by about ₹58,921, holding everything else constant — but this estimate is **not statistically significant** (p = 0.110), so it should be treated as inconclusive rather than a real effect. |
| **store_type_Mall (+11,433.46)** | A Mall store is predicted to sell about ₹11,433 more per month than an otherwise-identical High Street store. Borderline significance (p = 0.083) — a plausible effect but not fully conclusive at the 5% level. |
| **store_type_Residential (−16,575.85)** | A Residential-area store is predicted to sell about ₹16,576 less per month than an otherwise-identical High Street store. Statistically significant (p = 0.0068). |
| **store_type_Airport (+21,905.71)** | An Airport store is predicted to sell about ₹21,906 more per month than an otherwise-identical High Street store. Statistically significant (p = 0.0205). |

## Final Model Selected

The **Multiple Regression model** is the final model.

### Reason for selection

1. It has by far the highest R² (0.8214 vs a maximum of 0.7363 for any single-variable model),
   meaning it explains the largest share of sales variation.
2. It mixes numerical operational variables (marketing, footfall, inventory, discount) with
   store-format dummies, matching the way leadership actually thinks about levers ("should we open
   more Airport stores," "should we increase marketing," etc.).
3. Five of seven predictors are statistically significant at the 5% level, and a sixth
   (store_type_Mall) is borderline — only `avg_discount_pct` is clearly inconclusive, which is
   itself a useful finding.
4. The overall F-test confirms the model as a whole is highly statistically significant
   (p ≈ 1.1e-112).
