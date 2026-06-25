## Business Problem Summary

A retail chain's leadership team wants to understand what drives monthly sales performance across
its stores, so they can decide where to invest: marketing spend, inventory management, discounting
strategy, staffing, or store-type/region prioritization. This project uses regression analysis on
store-level monthly data to identify which factors are most strongly associated with monthly sales
and to produce an evidence-based business recommendation.

## Dataset Description

- File: `data/business_regression_data.xlsx` (sheet: `store_performance`)
- 320 records: 80 stores × 4 months (Jan–Apr 2025)
- 14 columns, mixing numerical, categorical, and identifier fields.

| Column | Type | Notes |
|---|---|---|
| store_id | Identifier (text) | Not useful as a regression input directly (320 unique combinations of store/month); used only to identify records and for residual analysis. |
| month | Date | Only 4 months observed; not used as a regression predictor (too few periods to estimate seasonality reliably), but kept for residual review. |
| region | Categorical (East/North/South/West) | Converted to dummy variables; tested but excluded from final model (low explanatory power). |
| store_type | Categorical (High Street/Mall/Residential/Airport) | Converted to dummy variables; included in final model. |
| marketing_spend | Numerical | Independent variable in both simple and multiple regression. |
| footfall | Numerical | Strongest predictor; independent variable in both simple and multiple regression. |
| avg_discount_pct | Numerical | Independent variable; statistically weak/insignificant predictor of sales. |
| staff_count | Numerical | **Not used in final model** — highly correlated with footfall (r ≈ 0.92), would create multicollinearity if both were included. |
| inventory_availability_pct | Numerical | Independent variable in multiple regression; significant. |
| competitor_distance_km | Numerical | Had 6 missing values; cleaned by mean imputation. Tested informally, weak correlation with sales; not used in final model. |
| holiday_flag | Binary (0/1) | Weak correlation with sales; not used in final model. |
| customer_rating | Numerical | Had 8 missing values; cleaned by mean imputation. Weak/near-zero correlation with sales; not used in final model. |
| monthly_sales | Numerical | **Dependent variable** for all regression models. |
| monthly_profit | Numerical | Excluded from regression — it is itself a downstream outcome of sales (and would leak information / cause reverse-causality issues if used to predict sales). |



# TASK:1 UNDERSTAND THE DATA
1)**Dependant Variable**: Monthly Sales , (Monthly profit if profitability is focus)

2)**Potential independent variables**: marketing_spend, footfall, avg_discount_pct, customer_rating, store_type, region, staff_count,             inventory_availability_pct, competitor_distance_km, month, holiday_flag.

3)**Numerical variables**: marketing_spend, footfall, avg_discount_pct, customer_rating ,staff_count, inventory_availability_pct,                  competitor_distance_km, monthly_sales(dependant), monthly_profit(dependant)

4)**Categorical variables**:  store_type, region , month, holiday_flag, store_id

5)**Variables that may need cleaning or transformation**: store_type,region,customer_rating needs transformation to be used in regression

6)**Variables that may not be useful for regression**: store_id, month( if seasionality is not concerned)


## Regression Approach

1. **Data cleaning** — missing values in two numerical columns filled with column means.
2. **Dummy variable creation** — `region` and `store_type` converted to dummies, dropping one
   reference category each (`East`, `High Street`) to avoid the dummy-variable trap.
3. **Simple regressions** — three single-predictor models (`footfall`, `marketing_spend`,
   `avg_discount_pct`) against `monthly_sales`, to understand each variable's standalone
   relationship.
4. **Multiple regression** — one model combining `marketing_spend`, `footfall`,
   `inventory_availability_pct`, `avg_discount_pct`, and `store_type` dummies, to capture the
   combined and more realistic picture.
5. **Residual analysis** — predicted sales and residuals calculated for the final model to spot
   over/under-performing stores and patterns the model misses.


## Dummy Variable Approach

- `region`: reference category = **East**. Dummies: `region_North`, `region_South`, `region_West`.
- `store_type`: reference category = **High Street**. Dummies: `store_type_Mall`,
  `store_type_Residential`, `store_type_Airport`.

Full explanation in `outputs/model_equations.md`.

## Model Comparison Summary

| Model | R² | Significant? |
|---|---|---|
| Simple — footfall | 0.7363 | Yes |
| Simple — marketing_spend | 0.1672 | Yes |
| Simple — avg_discount_pct | 0.0083 | No |
| **Multiple Regression (Final)** | **0.8214** (Adj. R² 0.8174) | **Yes** |

Full comparison in `analysis/model_comparison.md` and `outputs/regression_summary.xlsx`.

## Final Model Selected

**Multiple Regression**:
`monthly_sales = 136,913.65 + 1.165*marketing_spend + 33.66*footfall + 3,001.20*inventory_availability_pct − 58,920.81*avg_discount_pct + 11,433.46*store_type_Mall − 16,575.85*store_type_Residential + 21,905.71*store_type_Airport`

Selected because it has the highest R² (0.82), combines realistic actionable variables, and is
overall highly statistically significant (F p-value ≈ 1.1e-112). Full reasoning in
`outputs/model_equations.md`.

## Business Recommendation

Footfall, marketing spend, and inventory availability all show strong, statistically significant
positive associations with monthly sales, and store format (especially Airport vs Residential)
matters too. Discounting (`avg_discount_pct`) shows no reliable relationship with sales and should
not be assumed to drive volume. Full recommendation, risks, and the association-vs-causation
discussion are in `outputs/final_recommendation.md`.

## Assumptions & Limitations

- Missing values were imputed with column means rather than dropped, to preserve sample size; this
  is a simplification and slightly understates variability in those columns.
- The dataset spans only 4 months for 80 stores — too short to reliably model seasonality and a
  modest sample for rarer store types (e.g. 28 Airport-month records).
- This is observational, not experimental, data — regression shows association, not proof of
  causation (see `outputs/final_recommendation.md` for full discussion).
- `region` and several other variables (`staff_count`, `competitor_distance_km`, `holiday_flag`,
  `customer_rating`) were tested but excluded from the final model for the reasons noted above;
  excluding them keeps the model interpretable but means some real-world variation is unexplained
  (residual analysis shows mild regional patterns the model does not capture).

# Screenshots included:

- `screenshots/simple_regression_output.png` — simple regression output (footfall model)
- `screenshots/multiple_regression_output.png` — multiple regression output (all 7 predictors)
- `screenshots/residuals_preview.png` — predicted values and top residuals
- `screenshots/model_comparison_preview.png` — model comparison table
