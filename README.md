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

## Framing a Prediction Problem

We aim to predict recipe ratings based on complexity.

* **Type:** Regression
* **Target:** Rating
* **Features:** Ingredients, steps, time
* **Metric:** R² score

---

## Baseline Model

We trained a Linear Regression model using:

* Number of ingredients
* Number of steps
* Cooking time

### Result:

R² = ___

The model captures basic trends but is limited by its linear assumptions.

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
