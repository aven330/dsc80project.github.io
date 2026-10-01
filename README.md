# Do More Complex Recipes Get Better Ratings?

### By Akhil Venkat

---

## Introduction

This project analyzes a dataset of recipes and user ratings collected from Food.com. The dataset contains information about recipes, including their preparation time, number of steps, ingredients, and user-provided ratings.

The central question of this project is:
**What is the relationship between the cooking time and the average rating of recipes?**

This question is important because understanding how cooking time influences user ratings can provide insight into user preferences. For example, if longer recipes tend to receive higher ratings, it may suggest that users value more complex or detailed recipes. Conversely, if shorter recipes are rated more highly, it may indicate a preference for convenience. These insights can be useful for both recipe creators and recommendation systems.

The dataset used for this analysis contains over **80,000 recipes**, with each row representing a unique recipe.
[CSV FILES](https://drive.google.com/drive/folders/1RVFMjI1EghuGG6Q4pm0GHS2PDgsrYP7j?usp=sharing)

The main columns relevant to this analysis are:

- **`minutes`**: The total cooking time required to prepare the recipe (in minutes).  
- **`avg_rating`**: The average rating of the recipe, calculated from user ratings.  
- **`n_steps`**: The number of steps required to complete the recipe.  
- **`n_ingredients`**: The number of ingredients used in the recipe.  

These columns allow us to explore how cooking time relates to recipe ratings and whether more time-intensive recipes tend to be rated differently than quicker ones.

---

## Data Cleaning and Exploratory Data Analysis

I merged the recipes and interactions datasets using a left join so that all recipes were retained, even if they had no ratings.

I replaced ratings of 0 with NaN, since a rating of 0 represents missing data rather than a true user rating.

I converted all dates to datetime

I converted the strings that looked like lists into actual tuples for easy accsess in columns **`tags`**, **`steps`**, **`nutrition`**, and **`ingredients`**

I searched through **`description`** to find blank values and created a list of them (**`['***','-----','.','..','...',':)','NAN',]`**) to replace with NaN

I then computed the average rating per recipe and merged this back into the recipes dataset, resulting in one row per recipe.

This cleaning process ensures that our dataset accurately reflects user ratings while avoiding duplication from multiple reviews.

| name | id | minutes | contributor_id | submitted| tags | nutrition | n_steps | steps | description| ingredients| n_ingredients | avg_rating | rating_missing   |
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27 00:00:00 | ('60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings')                                                                        | ('138.4', '10.0', '50.0', '3.0', '3.0', '19.0', '6.0')      |        10 | ('heat the oven to 350f and arrange the rack in the middle', 'line an 8-by-8-inch glass baking dish with aluminum foil', 'combine chocolate and butter in a medium saucepan and cook over medium-low heat ', 'stirring frequently ', 'until evenly melted', 'remove from heat and let cool to room temperature', 'combine eggs ', 'sugar ', 'cocoa powder ', 'vanilla extract ', 'espresso ', 'and salt in a large bowl and briefly stir until just evenly incorporated', 'add cooled chocolate and mix until uniform in color', 'add flour and stir until just incorporated', 'transfer batter to the prepared baking dish', 'bake until a tester inserted in the center of the brownies comes out clean ', 'about 25 to 30 minutes', 'remove from the oven and cool completely before cutting')| these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              | ('bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour')                                                          |               9 |            4 | False            |
---

## Plots

<iframe src="assets/fig_bivariate_analysis_1.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/fig_bivariate_analysis_2.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/fig_bivariate_analysis_3.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/fig_univariate_analysis_1_new.html" width="800" height="600" frameborder="0"></iframe>
<iframe src="assets/fig_univariate_analysis_2_new.html" width="800" height="600" frameborder="0"></iframe> 


---
## Assessment of Missingness

I believe that the `avg_rating` column is likely **MNAR (Missing Not At Random)**.

This is because ratings are only present when users choose to rate a recipe. Recipes that are less popular, less visible, or less appealing may be less likely to receive ratings. Therefore, the probability that a rating is missing depends on unobserved factors such as recipe quality or user interest.

Since these factors are not captured in the dataset, the missingness cannot be fully explained by observed variables, which is characteristic of MNAR data.

If I had access to additional data—such as the number of times a recipe was vieId or user engagement metrics—I might be able to explain the missingness using observed variables, which could make the missingness MAR instead.

### Missingness Dependency

I analyzed whether the missingness of the `avg_rating` column depends on other variables in the dataset.

I created a boolean column indicating whether the average rating is missing and performed permutation tests to compare the distributions of other variables across missing and non-missing groups.

### Dependency on Cooking Time (`minutes`)

I tested whether the missingness of `avg_rating` depends on cooking time.

The observed difference in mean cooking time between recipes with missing and non-missing ratings was **117.34218767985172**, and the resulting p-value was **0.039**.

Since the p-value is **less than 0.05**, I **reject** the null hypothesis.

This suggests that the missingness of `avg_rating` **does** depend on cooking time.

### Dependency on Number of Ingredients (`n_ingredients`)

I also tested whether the missingness depends on the number of ingredients.

The observed difference was **0.25422661366086885**, with a p-value of **0.0**.

Since the p-value is **less than 0.05**, I **reject** the null hypothesis.

This suggests that the missingness of `avg_rating` **does** depends on the number of ingredients.

### Interpretation

These results indicate that missingness in `avg_rating` is partially dependent on observed variables, suggesting that the data may exhibit characteristics of Missing At Random (MAR). However, as discussed earlier, unobserved factors likely also influence missingness, supporting the possibility that the data is MNAR.
<iframe src="assets/fig_missingness_dependency.html" width="800" height="600" frameborder="0"></iframe>

---

## Hypothesis Testing

I investigated the hypothesis of 'What is the relationship between the cooking time and average rating of recipes?'

### Hypotheses

- **Null Hypothesis (H₀):**  
  Cooking time and average rating are independent. Recipes with longer cooking times do not have higher average ratings than those with shorter cooking times.

- **Alternative Hypothesis (H₁):**  
  Recipes with longer cooking times have higher average ratings than those with shorter cooking times.

### Test Statistic

I used the **difference in mean average ratings** between two groups:
- Recipes with cooking time above the median
- Recipes with cooking time at or below the median

Specifically, the test statistic is:
(mean rating of long recipes) − (mean rating of short recipes)

This is an appropriate choice because it directly measures whether longer recipes tend to receive higher ratings.

### Significance Level

I used a significance level of **α = 0.05**, which is a standard threshold for determining statistical significance.

### Method

I conducted a permutation test by randomly shuffling the average ratings across recipes 1000 times. For each permutation, I recomputed the difference in mean ratings between the two groups to generate a distribution of the test statistic under the null hypothesis.

### Results

- Observed test statistic: -0.035162877163243955
- p-value: 1.0

### Conclusion

Since the p-value is **greater than 0.05**, I fail to reject the null hypothesis.

This suggests that cooking time **does not** have a statistically significant effect on recipe ratings.

However, since this is an observational dataset and not a randomized experiment, I cannot conclude a causal relationship. Instead, I interpret this as evidence of an association between cooking time and average rating.
<iframe src="assets/fig_permutation_distribution.html" width="800" height="600" frameborder="0"></iframe> 

---

## Framing a Prediction Problem

I aim to **predict the average rating of a recipe** based on its characteristics.

### Type of Problem

This is a **regression problem**, since the response variable (average rating) is continuous.

### Response Variable

- **`avg_rating`**

I chose this variable because it summarizes user feedback for each recipe and represents overall user satisfaction. Using the average rating per recipe avoids duplication issues from multiple individual reviews and provides a stable target for prediction.

### Features Used

I use features that describe the recipe’s complexity and content, including:
- `minutes` (cooking time)
- `n_steps` (number of steps)
- `n_ingredients` (number of ingredients)

These features are all quantitative and capture different aspects of how complex or time-consuming a recipe is.

### Time of Prediction

At the time of prediction, I assume I only have access to information available **before users rate the recipe**, such as:
- cooking time  
- number of steps  
- number of ingredients  

I do **not** use:
- `avg_rating` (target)
- individual user ratings
- review text

This ensures that our model does not use information that would only be available after the recipe has already been rated.

### Evaluation Metric

I use the **R² (coefficient of determination)** to evaluate model performance.

R² measures how Ill the model explains the variability in the response variable (average rating). It is appropriate here because:
- the target is continuous  
- I care about how Ill the model captures overall trends  
- it provides an interpretable measure of goodness-of-fit  

Other metrics like RMSE could also be used, but R² is more interpretable in terms of explained variance.

### Summary

This prediction problem allows us to assess whether recipe characteristics alone can meaningfully predict user ratings, and to what extent complexity influences perceived recipe quality.

---
## Final Model

To improve upon our baseline model, we engineered additional features and used a more flexible modeling algorithm.

### Engineered Features

We added two new features:

- **log_minutes**: a log transformation of cooking time to reduce skewness and better capture relationships between cooking time and ratings.
- **steps_per_ingredient**: a measure of recipe complexity that captures how many steps are required per ingredient, reflecting how intricate a recipe may be.

These features were chosen to better represent the underlying structure of recipe complexity beyond simple counts.

### Model Choice

We used a **Random Forest Regressor**, which can capture nonlinear relationships and interactions between features that a linear model cannot.

### Hyperparameter Tuning

We tuned the following hyperparameters using GridSearchCV:
- `max_depth`: controls how deep each tree can grow
- `n_estimators`: number of trees in the forest

The best parameters were:
'''
param_grid = {
    'model__max_depth': [5, 10, 15],
    'model__n_estimators': [50, 100]
}
'''

These were selected based on cross-validation performance using R².

### Performance

- Baseline Model R²: -0.0001621902739767922  
- Final Model R²: 0.0018127207872883355  

- Baseline RMSE: 0.6359061249350496  
- Final RMSE: 0.6352779875106677  

The final model achieved better performance than the baseline model, indicating that the engineered features and nonlinear modeling approach improved predictive accuracy.

### Conclusion

The improvement suggests that the relationship between recipe characteristics and ratings is not purely linear, and that capturing complexity through engineered features leads to better predictions.

---
## Fairness Analysis

We evaluated whether our model performs differently across groups of recipes.

### Groups

We defined the following groups based on recipe complexity:

- **Group X (simple recipes):** recipes with a number of ingredients less than or equal to the median  
- **Group Y (complex recipes):** recipes with a number of ingredients greater than the median  

### Evaluation Metric

We used **Root Mean Squared Error (RMSE)** to evaluate model performance, since this is a regression problem. RMSE measures the average prediction error, with higher values indicating worse performance.

### Hypotheses

- **Null Hypothesis (H₀):**  
  The model is fair. The RMSE for simple and complex recipes is the same, and any observed difference is due to random chance.

- **Alternative Hypothesis (H₁):**  
  The model is unfair. The RMSE for simple recipes is higher than for complex recipes.

### Test Statistic

We used the difference in RMSE between the two groups:

RMSE (simple recipes) − RMSE (complex recipes)

### Significance Level

We used a significance level of **α = 0.05**.

### Method

We performed a permutation test by randomly shuffling group labels 1000 times and recomputing the difference in RMSE for each permutation.

### Results

- Observed RMSE difference: **0.020550026266543786**  
- p-value: **0.103**
- 

### Conclusion

Since the p-value is **greater than 0.05**, we **fail to reject** the null hypothesis.

This suggests that the model **does not** perform worse for simple recipes compared to complex recipes.

However, as with all observational analyses, this result reflects association rather than causation.
---
