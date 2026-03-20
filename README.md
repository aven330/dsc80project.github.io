# Do More Complex Recipes Get Better Ratings?

### By Akhil Venkat

---

## Introduction

This project investigates whether more complex recipes receive higher ratings.

Recipe complexity is defined using:

* Number of ingredients
* Number of steps
* Cooking time

Understanding this relationship helps us determine whether users prefer simple recipes or more elaborate ones.

---

## Data Cleaning and Exploratory Data Analysis

The dataset was created by merging recipe data with user interaction data.

* Rows with missing ratings were preserved for missingness analysis
* Key features were extracted:

  * Number of ingredients
  * Number of steps

### Key Insight:

Most recipes are rated highly (between 4 and 5), suggesting a positive bias in user ratings.

---

## Sample Visualization

<iframe src="assets/fig_bivariate_analysis_1.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/fig_bivariate_analysis_2.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/fig_bivariate_analysis_3.html" width="800" height="600" frameborder="0"></iframe>


---
## MNAR Analysis

We believe that the `avg_rating` column is likely **MNAR (Missing Not At Random)**.

This is because ratings are only present when users choose to rate a recipe. Recipes that are less popular, less visible, or less appealing may be less likely to receive ratings. Therefore, the probability that a rating is missing depends on unobserved factors such as recipe quality or user interest.

Since these factors are not captured in the dataset, the missingness cannot be fully explained by observed variables, which is characteristic of MNAR data.

If we had access to additional data—such as the number of times a recipe was viewed or user engagement metrics—we might be able to explain the missingness using observed variables, which could make the missingness MAR instead.

## Missingness Dependency

We analyzed whether the missingness of the `avg_rating` column depends on other variables in the dataset.

We created a boolean column indicating whether the average rating is missing and performed permutation tests to compare the distributions of other variables across missing and non-missing groups.

### Dependency on Cooking Time (`minutes`)

We tested whether the missingness of `avg_rating` depends on cooking time.

The observed difference in mean cooking time between recipes with missing and non-missing ratings was **___**, and the resulting p-value was **___**.

Since the p-value is **[less than / greater than] 0.05**, we **[reject / fail to reject]** the null hypothesis.

This suggests that the missingness of `avg_rating` **[does / does not]** depend on cooking time.

### Dependency on Number of Ingredients (`n_ingredients`)

We also tested whether the missingness depends on the number of ingredients.

The observed difference was **___**, with a p-value of **___**.

Since the p-value is **[less than / greater than] 0.05**, we **[reject / fail to reject]** the null hypothesis.

This suggests that the missingness of `avg_rating` **[does / does not]** depend on the number of ingredients.

### Interpretation

These results indicate that missingness in `avg_rating` is partially dependent on observed variables, suggesting that the data may exhibit characteristics of Missing At Random (MAR). However, as discussed earlier, unobserved factors likely also influence missingness, supporting the possibility that the data is MNAR.

---

## Hypothesis Testing

We investigated whether recipes with longer cooking times tend to receive higher average ratings.

### Hypotheses

- **Null Hypothesis (H₀):**  
  Cooking time and average rating are independent. Recipes with longer cooking times do not have higher average ratings than those with shorter cooking times.

- **Alternative Hypothesis (H₁):**  
  Recipes with longer cooking times have higher average ratings than those with shorter cooking times.

### Test Statistic

We used the **difference in mean average ratings** between two groups:
- Recipes with cooking time above the median
- Recipes with cooking time at or below the median

Specifically, the test statistic is:
(mean rating of long recipes) − (mean rating of short recipes)

This is an appropriate choice because it directly measures whether longer recipes tend to receive higher ratings.

### Significance Level

We used a significance level of **α = 0.05**, which is a standard threshold for determining statistical significance.

### Method

We conducted a permutation test by randomly shuffling the average ratings across recipes 1000 times. For each permutation, we recomputed the difference in mean ratings between the two groups to generate a distribution of the test statistic under the null hypothesis.

### Results

- Observed test statistic: -0.035162877163243955
- p-value: 1.0

### Conclusion

Since the p-value is **greater than 0.05**, we fail to reject the null hypothesis.

This suggests that cooking time **does not** have a statistically significant effect on recipe ratings.

However, since this is an observational dataset and not a randomized experiment, we cannot conclude a causal relationship. Instead, we interpret this as evidence of an association between cooking time and average rating.

---

## Prediction Problem

We aim to **predict the average rating of a recipe** based on its characteristics.

### Type of Problem

This is a **regression problem**, since the response variable (average rating) is continuous.

### Response Variable

- **`avg_rating`**

We chose this variable because it summarizes user feedback for each recipe and represents overall user satisfaction. Using the average rating per recipe avoids duplication issues from multiple individual reviews and provides a stable target for prediction.

### Features Used

We use features that describe the recipe’s complexity and content, including:
- `minutes` (cooking time)
- `n_steps` (number of steps)
- `n_ingredients` (number of ingredients)

These features are all quantitative and capture different aspects of how complex or time-consuming a recipe is.

### Time of Prediction

At the time of prediction, we assume we only have access to information available **before users rate the recipe**, such as:
- cooking time  
- number of steps  
- number of ingredients  

We do **not** use:
- `avg_rating` (target)
- individual user ratings
- review text

This ensures that our model does not use information that would only be available after the recipe has already been rated.

### Evaluation Metric

We use the **R² (coefficient of determination)** to evaluate model performance.

R² measures how well the model explains the variability in the response variable (average rating). It is appropriate here because:
- the target is continuous  
- we care about how well the model captures overall trends  
- it provides an interpretable measure of goodness-of-fit  

Other metrics like RMSE could also be used, but R² is more interpretable in terms of explained variance.

### Summary

This prediction problem allows us to assess whether recipe characteristics alone can meaningfully predict user ratings, and to what extent complexity influences perceived recipe quality.

---

## Final Model

We improved performance using a Random Forest model.

### Improvements:

* Captures nonlinear relationships
* Handles feature interactions

### Result:

R² = ___

The final model performed better than the baseline.

---

## Fairness Analysis

We tested whether the model performs differently for:

* Simple recipes vs complex recipes

### Result:

The difference in RMSE was tested using permutation testing.

### Conclusion:

There is (no / some) evidence of performance disparity.

---

## Final Takeaway

Recipe complexity does not strongly determine ratings, suggesting users value factors beyond just effort or time.

---
