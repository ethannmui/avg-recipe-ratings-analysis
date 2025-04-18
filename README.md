# What determines the average rating of a recipe?
**Name(s):** Ethan Mui, Daniel Yang
---
## Introduction
Our data is the Recipe and Ratings dataset. The 'recipe' part consists of numerical values like nutrition values, number of steps, minutes, and ingredients to make the recipe. It also consists of categorical parts like tags, description of the recipe, and ingredients list. On the 'ratings' section, there are ratings and reviews given by user ids for specific recipes. Just like many other people, we are both foodies. Average rating value determined by numerous people can be fair assessment of the quality of the recipe. This average rating can help consumers choose high-quality recipes to make whether that's tasty, efficient, or simple. 

To begin, we combined the recipes dataset and the ratings dataset to create a unified dataset by joining on recipe_id, keeping all recipes regardless of whether they have a review. Our combined dataset has 234429 rows.

As a result, we are trying to predict the average rating given by people for a recipe based on protein value and calories of that recipe, number of steps it has, number of minutes it takes to make the recipe, and the number of reviews given to that recipe.

***id***: Each recipe is uniquely defined by an id.

***name***: The name of the recipe.

***rating***: A rating given to a recipe id by an individual user. (Note: can be multiple ratings/reviews per recipe)

***avg_rating***: Average Rating represents the average rate of a recipe based on ratings given by individual, unique users. 

***review***: "review" represents a word review given by an individual user on a recipe.

***num_reviews***: "num_reviews" represents the total number of reviews given to a unique recipe.

***n_steps***: Number of steps to complete a given recipe.

***Protein (PDV)***: The protein value for a given recipe.

***minutes***: The total number of minutes it takes to complete a given recipe.

***Calories***: The number of calories for a given recipe. 

---
## Data Cleaning and Exploratory Data Analysis


### Data Cleaning

First, we cleaned our data by replacing all of the ratings of '0' given to a recipe by individual users with a 'NaN'. We preformed this process because logically users can't give a rating of 0 to a recipe. In most cases, users must rate recipes between a rating of 1 and 5. If a user doesn't include a rating for a recipe with their review, it is automatically assigned a '0'. To make sure that this fact wouldn't skew our 'avg_rating', we replaced all ratings with '0' with 'NaN'.

Currently, our data frame contains multiple records for a unique recipe because there could be different reviews and ratings given by unique users. As a result, we calculated the average rating given to each unique recipe and mapped to all records in our database. This allowed us to compare the value our model was predicting against the actual value for training and testing purposes. We also repeated this process to calculate the total number of reviews per unique recipe and then mapped this value to all records. 

Next, we dropped all the duplicate recipe columns after this grouping and calculating process so we retained only one record per recipe. This is so we wouldn't count the same recipe id with the same average rating as a unique point which could skew our prediction model. For example, if one recipe had a bunch of reviews and we mapped the same average rating to all those recipe's records. 

Next, we also split our original nutrition column into more specific columns: 'Calories', 'Total Fat (PDV)', 'Sugar (PDV)', 'Sodium (PDV)', 'Protein (PDV)', 'Saturated Fat (PDV)', 'Carbohydrates (PDV)' to get a better visualization of the nutrition values per recipe. However, for our prediction problem, we will be focusing specifically on 'Calories' and 'Protein (PDV)'.

Lastly, we filtered any records with outlier values that we deemed abnormal and illogical by human analysis. We removed records that had: 
- 'minutes' > 4320 (i.e a recipe that took over 3 days to make)
- 'n_steps' > 30 (i.e a recipe that has over 30 steps)
- 'Calories' > 2000 (i.e a recipe that has over 2000 calories)
- 'Protein (PDV) > 300 (i.e a recipe that has over 300 protein)

Finally, we filtered our data table to only retain the columns that we would need for our predictive model and visualization. After cleaning, our datafame has 81233 rows and 8 columns.

|     id | name                                 |   minutes |   n_steps |   Calories |   Protein (PDV) |   num_reviews |   avg_rating |
|-------:|:-------------------------------------|----------:|----------:|-----------:|----------------:|--------------:|-------------:|
| 333281 | 1 brownies in the world    best ever |        40 |        10 |      138.4 |               3 |             1 |            4 |
| 453467 | 1 in canada chocolate chip cookies   |        45 |        12 |      595.1 |              13 |             1 |            5 |
| 306168 | 412 broccoli casserole               |        40 |         6 |      194.8 |              22 |             4 |            5 |
| 286009 | millionaire pound cake               |       120 |         7 |      878.3 |              20 |             1 |            5 |
| 475785 | 2000 meatloaf                        |        90 |        17 |      267   |              29 |             2 |            5 |

### Univariate Analysis

The folllowing graph shows the distribution of the number of steps for a recipe in the whole dataset. There is definitely a right skew in our dataset which makes sense because logically it makes sense for recipes to have 5 - 12 steps as opposed to 15-20+.

 <iframe
 src="assets/file-name.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

The following graph shows the distribution of calories for a recipe in the whole dataset. Again, there is a right skew in our dataset because it makes more sense for a recipe to have 200 - 500 calories as opposed 1000+ calories. 

  <iframe
 src="assets/file-name.html"
 width="800"
 height="600"
 frameborder="0"
 ></iframe>

### Bivariate Analysis

### Interesting Aggregates

### Imputation




---
## Framing a Prediction Problem


---
## Baseline Model



---
## Final Model
