# Model Comparison

This document compares all four regression models built for this analysis: three simple linear
regressions and one multiple regression. 

## Model 1 — Simple Regression: monthly_sales ~ footfall

| Item | Detail |
|---|---|
| Variables used | DV: `monthly_sales`. IV: `footfall` |
| Equation | monthly_sales = 446,410.58 + 35.68 × footfall |
| R-squared | 0.7363 (footfall alone explains ~74% of sales variation) |
| Significant variables | footfall: p = 4.75e-94 (highly significant) |
| Business usefulness | High for a single-variable check. Footfall is the strongest standalone predictor of sales, confirming that getting more customers into a store is the single biggest lever leadership has. |
| Limitations | Ignores everything else (marketing, inventory, store format, discounting). Cannot explain *why* footfall varies, and footfall itself is only partly controllable by management (it depends on location, marketing, and local demand). |

## Model 2 — Simple Regression: monthly_sales ~ marketing_spend

| Item | Detail |
|---|---|
| Variables used | DV: `monthly_sales`. IV: `marketing_spend` |
| Equation | monthly_sales = 560,777.35 + 2.13 × marketing_spend |
| R-squared | 0.1672 (marketing spend alone explains ~17% of sales variation) |
| Significant variables | marketing_spend: p = 2.48e-14 (highly significant) |
| Business usefulness | Moderate. Confirms marketing spend has a real, statistically significant relationship with sales, and it is one of the more directly controllable levers leadership has — but alone it tells a much weaker story than footfall. |
| Limitations | Low explanatory power on its own (R² = 0.17). Does not capture diminishing returns, nor interaction with store type or region. |

## Model 3 — Simple Regression: monthly_sales ~ avg_discount_pct

| Item | Detail |
|---|---|
| Variables used | DV: `monthly_sales`. IV: `avg_discount_pct` |
| Equation | monthly_sales = 697,835.63 − 138,730.45 × avg_discount_pct |
| R-squared | 0.0083 (essentially no explanatory power) |
| Significant variables | avg_discount_pct: p = 0.104 (**not** significant at the 5% level) |
| Business usefulness | Low. This model demonstrates that discounting, on its own, does not show a reliable relationship with monthly sales in this dataset — useful as a finding ("don't assume discounting drives sales"), but not useful as a forecasting tool. |
| Limitations | Weak/noisy relationship; negative coefficient should not be read as "discounts hurt sales" given the high p-value — it is statistically indistinguishable from no effect. |

## Model 4 — Multiple Regression (Final Model)

monthly_sales ~ marketing_spend + footfall + inventory_availability_pct + avg_discount_pct +
store_type_Mall + store_type_Residential + store_type_Airport (reference category: High Street)

| Item | Detail |
|---|---|
| Variables used | DV: `monthly_sales`. IVs: marketing_spend, footfall, inventory_availability_pct, avg_discount_pct (numerical) + store_type_Mall, store_type_Residential, store_type_Airport (dummies) |
| Equation | monthly_sales = 136,913.65 + 1.165×marketing_spend + 33.66×footfall + 3,001.20×inventory_availability_pct − 58,920.81×avg_discount_pct + 11,433.46×store_type_Mall − 16,575.85×store_type_Residential + 21,905.71×store_type_Airport |
| R-squared | 0.8214 |
| Adjusted R-squared | 0.8174 |
| F-statistic / p-value | 204.98 / 1.14e-112 (model is highly significant overall) |
| Significant variables (p < 0.05) | footfall (p≈1.1e-102), marketing_spend (p≈1.4e-17), inventory_availability_pct (p≈4.7e-10), store_type_Residential (p=0.0068), store_type_Airport (p=0.0205) |
| Weak / borderline variables | store_type_Mall (p=0.083, borderline), avg_discount_pct (p=0.110, **not significant**) |
| Business usefulness | Highest. This model explains 82% of the variation in monthly sales using a realistic combination of levers leadership can actually act on (marketing spend, inventory availability, store format) plus the strongest organic driver (footfall). |
| Limitations | Does not prove causation (see `outputs/final_recommendation.md`). avg_discount_pct remains statistically weak even with other variables controlled for, so discount-strategy conclusions should not be drawn from this model. `region` and `staff_count` were tested but excluded — region added little explanatory power, and `staff_count` is too highly correlated with `footfall` (r ≈ 0.92) to include both without multicollinearity problems. |

## Summary Table

| Model | R² | Adj. R² | Overall significant? |
|---|---|---|---|
| Simple — footfall | 0.7363 | – | Yes |
| Simple — marketing_spend | 0.1672 | – | Yes |
| Simple — avg_discount_pct | 0.0083 | – | No |
| **Multiple Regression (Final)** | **0.8214** | **0.8174** | **Yes** |

**Conclusion:** the multiple regression model is selected as the final model. It captures far more
of the variation in monthly sales than any single variable, combines numerical and categorical
(store-format) information, and every key business variable except `avg_discount_pct` is
statistically meaningful.
