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

<iframe src="assets/rating_distribution.html" width="800" height="600" frameborder="0"></iframe>

---

## Assessment of Missingness

We analyzed missing values in the `rating` column.

### MNAR Explanation

Missing ratings are likely **Missing Not At Random (MNAR)** because users are more likely to leave ratings when they have strong opinions.

### Permutation Test Results

* **Cooking time (minutes):** p = 0.236
* **Number of ingredients:** p = 0.084

We failed to reject the null hypothesis in both cases, suggesting that missingness does not strongly depend on these variables.

### Interpretation

Missingness is likely driven by the data collection process (whether a user chose to leave a rating), rather than observed features.

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
