# Do More Complex Recipes Get Better Ratings?

### By Akhil Venkat

---

## Introduction

This project analyzes a dataset of recipes and user ratings collected from Food.com. The dataset contains information about recipes, including their preparation time, number of steps, ingredients, and user-provided ratings.

The central question of this project is:
**What is the relationship between the cooking time and the average rating of recipes?**

This question is important because understanding how cooking time influences user ratings can provide insight into user preferences. For example, if longer recipes tend to receive higher ratings, it may suggest that users value more complex or detailed recipes. Conversely, if shorter recipes are rated more highly, it may indicate a preference for convenience. These insights can be useful for both recipe creators and recommendation systems.

The dataset used for this analysis contains over **80,000 recipes**, with each row representing a unique recipe.

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

I searched through **`description`** to find blank values and created a list of them (['***','-----','.','..','...',':)','NAN',]) to replace with NaN

I then computed the average rating per recipe and merged this back into the recipes dataset, resulting in one row per recipe.

This cleaning process ensures that our dataset accurately reflects user ratings while avoiding duplication from multiple reviews.

| name                                 |     id |   minutes |   contributor_id | submitted           | tags                                                                                                                                                                                                                                                                                               | nutrition                                                   |   n_steps | steps                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               | description                                                                                                                                                                                                                                                                                                                                                                       | ingredients                                                                                                                                                                                                                             |   n_ingredients |   avg_rating | rating_missing   |
|:-------------------------------------|-------:|----------:|-----------------:|:--------------------|:---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:------------------------------------------------------------|----------:|:--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|:----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|----------------:|-------------:|:-----------------|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27 00:00:00 | ('60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'for-large-groups', 'desserts', 'lunch', 'snacks', 'cookies-and-brownies', 'chocolate', 'bar-cookies', 'brownies', 'number-of-servings')                                                                        | ('138.4', '10.0', '50.0', '3.0', '3.0', '19.0', '6.0')      |        10 | ('heat the oven to 350f and arrange the rack in the middle', 'line an 8-by-8-inch glass baking dish with aluminum foil', 'combine chocolate and butter in a medium saucepan and cook over medium-low heat ', 'stirring frequently ', 'until evenly melted', 'remove from heat and let cool to room temperature', 'combine eggs ', 'sugar ', 'cocoa powder ', 'vanilla extract ', 'espresso ', 'and salt in a large bowl and briefly stir until just evenly incorporated', 'add cooled chocolate and mix until uniform in color', 'add flour and stir until just incorporated', 'transfer batter to the prepared baking dish', 'bake until a tester inserted in the center of the brownies comes out clean ', 'about 25 to 30 minutes', 'remove from the oven and cool completely before cutting')                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   | these are the most; chocolatey, moist, rich, dense, fudgy, delicious brownies that you'll ever make.....sereiously! there's no doubt that these will be your fav brownies ever for you can add things to them or make them plain.....either way they're pure heaven!                                                                                                              | ('bittersweet chocolate', 'unsalted butter', 'eggs', 'granulated sugar', 'unsweetened cocoa powder', 'vanilla extract', 'brewed espresso', 'kosher salt', 'all-purpose flour')                                                          |               9 |            4 | False            |
| 1 in canada chocolate chip cookies   | 453467 |        45 |          1848091 | 2011-04-11 00:00:00 | ('60-minutes-or-less', 'time-to-make', 'cuisine', 'preparation', 'north-american', 'for-large-groups', 'canadian', 'british-columbian', 'number-of-servings')                                                                                                                                      | ('595.1', '46.0', '211.0', '22.0', '13.0', '51.0', '26.0')  |        12 | ('pre-heat oven the 350 degrees f', 'in a mixing bowl ', 'sift together the flours and baking powder', 'set aside', 'in another mixing bowl ', 'blend together the sugars ', 'margarine ', 'and salt until light and fluffy', 'add the eggs ', 'water ', 'and vanilla to the margarine / sugar mixture and mix together until well combined', 'add in the flour mixture to the wet ingredients and blend until combined', 'scrape down the sides of the bowl and add the chocolate chips', 'mix until combined', 'scrape down the sides to the bowl again', 'using an ice cream scoop ', 'scoop evenly rounded balls of dough and place of cookie sheet about 1 - 2 inches apart to allow for spreading during baking', 'bake for 10 - 15 minutes or until golden brown on the outside and soft & chewy in the center', 'serve hot and enjoy !')                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    | this is the recipe that we use at my school cafeteria for chocolate chip cookies. they must be the best chocolate chip cookies i have ever had! if you don't have margarine or don't like it, then just use butter (softened) instead.                                                                                                                                            | ('white sugar', 'brown sugar', 'salt', 'margarine', 'eggs', 'vanilla', 'water', 'all-purpose flour', 'whole wheat flour', 'baking soda', 'chocolate chips')                                                                             |              11 |            5 | False            |
| 412 broccoli casserole               | 306168 |        40 |            50969 | 2008-05-30 00:00:00 | ('60-minutes-or-less', 'time-to-make', 'course', 'main-ingredient', 'preparation', 'side-dishes', 'vegetables', 'easy', 'beginner-cook', 'broccoli')                                                                                                                                               | ('194.8', '20.0', '6.0', '32.0', '22.0', '36.0', '3.0')     |         6 | ('preheat oven to 350 degrees', 'spray a 2 quart baking dish with cooking spray ', 'set aside', 'in a large bowl mix together broccoli ', 'soup ', 'one cup of cheese ', 'garlic powder ', 'pepper ', 'salt ', 'milk ', '1 cup of french onions ', 'and soy sauce', 'pour into baking dish ', 'sprinkle remaining cheese over top', 'bake for 25 minutes or until cheese is lightly browned', 'sprinkle with rest of french fried onions and bake until onions are browned and cheese is bubbly ', 'about 10 more minutes')                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                         | since there are already 411 recipes for broccoli casserole posted to "zaar" ,i decided to call this one  #412 broccoli casserole.i don't think there are any like this one in the database. i based this one on the famous "green bean casserole" from campbell's soup. but i think mine is better since i don't like cream of mushroom soup.submitted to "zaar" on may 28th,2008 | ('frozen broccoli cuts', 'cream of chicken soup', 'sharp cheddar cheese', 'garlic powder', 'ground black pepper', 'salt', 'milk', 'soy sauce', 'french-fried onions')                                                                   |               9 |            5 | False            |
| millionaire pound cake               | 286009 |       120 |           461724 | 2008-02-12 00:00:00 | ('time-to-make', 'course', 'cuisine', 'preparation', 'occasion', 'north-american', 'desserts', 'american', 'southern-united-states', 'dinner-party', 'holiday-event', 'cakes', 'dietary', 'christmas', 'thanksgiving', 'low-sodium', 'low-in-something', 'taste-mood', 'sweet', '4-hours-or-less') | ('878.3', '63.0', '326.0', '13.0', '20.0', '123.0', '39.0') |         7 | ('freheat the oven to 300 degrees', 'grease a 10-inch tube pan with butter ', 'dust the bottom and sides with flour ', 'and set aside', 'in a large mixing bowl ', 'cream the butter and sugar with an electric mixer and add the eggs one at a time ', 'beating after each addition', 'alternately add the flour and milk ', 'stirring till the batter is smooth', 'add the two extracts and stir till well blended', 'scrape the batter into the prepared pan and bake till a cake tester or knife blade inserted in the center comes out clean ', 'about 1 1 / 2 hours', 'cool the cake in the pan on a rack for 5 minutes ', 'then turn it out on the rack to cool completely')                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 | why a millionaire pound cake?  because it's super rich!  this scrumptious cake is the pride of an elderly belle from jackson, mississippi.  the recipe comes from "the glory of southern cooking" by james villas.                                                                                                                                                                | ('butter', 'sugar', 'eggs', 'all-purpose flour', 'whole milk', 'pure vanilla extract', 'almond extract')                                                                                                                                |               7 |            5 | False            |
| 2000 meatloaf                        | 475785 |        90 |          2202916 | 2012-03-06 00:00:00 | ('time-to-make', 'course', 'main-ingredient', 'preparation', 'main-dish', 'potatoes', 'vegetables', '4-hours-or-less', 'meatloaf', 'simply-potatoes2')                                                                                                                                             | ('267.0', '30.0', '12.0', '12.0', '29.0', '48.0', '2.0')    |        17 | ('pan fry bacon ', 'and set aside on a paper towel to absorb excess grease', 'mince yellow onion ', 'red bell pepper ', 'and add to your mixing bowl', 'chop garlic and set aside', 'put 1tbsp olive oil into a saut pan ', 'along with chopped garlic ', 'teaspoons white pepper and a pinch of kosher salt', 'bring to a medium heat to sweat your garlic', 'preheat oven to 350f', 'coarsely chop your baby spinach add to your heated pan ', 'stir frequently for approximately 5 min to wilt', 'add your spinach to the mixing bowl', 'chop your now cooled bacon ', 'and add it to the mixing bowl', 'add your meatloaf mix to the bowl ', 'with one egg and mix till thoroughly combined', 'add your goat cheese ', 'one egg ', '1 / 8 tsp white pepper and 1 / 8 tsp of kosher salt and mix till thoroughly combined', 'transfer to a 9x5 meatloaf pan ', 'and cook for 60 min or until the internal temperature is at least 160f', 'let stand for 5min', 'melt 1tbsp unsalted butter into a frying pan ', 'and cook up to three eggs at a time', 'crack each egg into a separate dish ', 'in order to prevent egg shells from reaching the pan ', 'then add salt and pepper to taste', 'wait until the egg whites are firm looking ', 'but slightly runny on top before flipping your eggs', 'after flipping ', 'wait 10~20 seconds before removing each egg and placing it over your slices of meatloaf') | ready, set, cook! special edition contest entry: a mediterranean flavor inspired meatloaf dish. featuring: simply potatoes - shredded hash browns, egg, bacon, spinach, red bell pepper, and goat cheese.                                                                                                                                                                         | ('meatloaf mixture', 'unsmoked bacon', 'goat cheese', 'unsalted butter', 'eggs', 'baby spinach', 'yellow onion', 'red bell pepper', 'simply potatoes shredded hash browns', 'fresh garlic', 'kosher salt', 'white pepper', 'olive oil') |              13 |            5 | False            |


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
