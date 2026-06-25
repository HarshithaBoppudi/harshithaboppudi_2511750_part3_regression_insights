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


