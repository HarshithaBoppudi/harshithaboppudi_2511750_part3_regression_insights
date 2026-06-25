# Residual Analysis


```
predicted_sales = 136,913.65 + 1.165*marketing_spend + 33.66*footfall
                  + 3,001.20*inventory_availability_pct - 58,920.81*avg_discount_pct
                  + 11,433.46*store_type_Mall - 16,575.85*store_type_Residential
                  + 21,905.71*store_type_Airport

residual = monthly_sales (actual) - predicted_sales
```

A positive residual means the store sold **more** than the model predicted (the model
under-predicted). A negative residual means the store sold **less** than the model predicted (the
model over-predicted).

## Top 5 Largest POSITIVE Residuals (model under-predicts)

| Rank | Store ID | Month | Region | Store Type | Actual Sales | Predicted Sales | Residual |
#
|---|---|---|---|---|---|---|---|
#
| 1 | STR-1030 | 2025-02 | West | Residential | 820,519 | 714,620 | +105,899 |
#
| 2 | STR-1073 | 2025-03 | East | Residential | 813,317 | 713,693 | +99,623 |
#
| 3 | STR-1032 | 2025-01 | South | High Street | 914,544 | 815,773 | +98,771 |
#
| 4 | STR-1050 | 2025-04 | North | Residential | 735,787 | 638,540 | +97,246 |
#
| 5 | STR-1028 | 2025-04 | East | Mall | 713,611 | 618,466 | +95,145 |

## Top 5 Largest NEGATIVE Residuals (model over-predicts)

| Rank | Store ID | Month | Region | Store Type | Actual Sales | Predicted Sales | Residual |
|---|---|---|---|---|---|---|---|
| 1 | STR-1017 | 2025-03 | West | High Street | 685,379 | 844,620 | −159,241 |
| 2 | STR-1023 | 2025-02 | South | Mall | 627,172 | 755,437 | −128,265 |
| 3 | STR-1012 | 2025-01 | West | Residential | 595,468 | 716,546 | −121,079 |
| 4 | STR-1007 | 2025-02 | West | Mall | 800,452 | 900,272 | −99,821 |
| 5 | STR-1009 | 2025-01 | North | Residential | 641,865 | 740,315 | −98,450 |

## What the residuals mean in business terms

- **Positive residuals (under-prediction)** point to stores that are out-performing what their
  footfall, marketing spend, inventory, discount level, and store format would suggest. This could
  reflect strong local management, a loyal customer base, a one-off local event, or a data input
  (e.g. footfall or marketing spend) that under-states the store's real selling environment. These
  stores are good candidates to study for best-practice lessons.
- **Negative residuals (over-prediction)** point to stores under-performing relative to their
  inputs. Four of the five largest negative residuals are **West region** stores, which is worth
  flagging to leadership as a possible regional execution issue, even though `region` was not a
  variable in the final model.
- The overall residual pattern is **not symmetric**: positive residuals slightly outnumber negative
  ones (167 vs 153 out of 320), and the largest gaps are on the over-prediction side. This means the
  model is, on balance, neither systematically over- nor under-predicting sales overall (the mean
  residual is ~0 by construction of OLS), but specific segments diverge more than others.

## Is the model systematically biased for any group of stores?

Average residual by **store_type** is ~0 for every category (Mall, Residential, High Street,
Airport) — expected, since `store_type` dummies are already in the model and absorb that
variation.

Average residual by **region** (a variable *not* included in the final model) is not zero:

| Region | Average residual |
|---|---|
| East | −10,321 |
| North | −2,012 |
| South | +7,948 |
| West | +7,883 |

This indicates the model tends to **slightly over-predict East region sales** and **slightly
under-predict South/West region sales**. This is a real limitation: region-specific factors (e.g.
local competition, demographics, or economic conditions) explain some sales variation that the
current model does not capture. Leadership should treat region as a secondary factor to monitor,
even though it was not strong enough to include in the final model without diluting the
clearer, more actionable variables.

## Takeaway

The residual pattern suggests the model is reasonably well-specified for the variables it includes,
but regional execution differences and store-specific factors beyond the model's inputs still
account for a meaningful share of sales swings — supporting the recommendation that regression
results should inform, not replace, local store-level judgment.

