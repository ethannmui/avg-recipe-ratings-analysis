# What determines the average rating of a recipe?
**Name(s):** Ethan Mui, Daniel Yang
**Email(s):** ethanmui@umich.edu, ydaniel@umich.edu
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

The folllowing graph shows the distribution of the number of steps for a recipe in the whole dataset. There is definitely a right skew in our dataset which makes sense because logically it makes sense for recipes to have 5 - 12 steps as opposed to 15-20+. This distribution of the number of steps for a recipe could give insight on whether people rate recipes higher if they have less or more steps. 
 <iframe
 src="assets/distribution-calories.html"
 width="800"
 height="450"
 frameborder="0"
 ></iframe>

The following graph shows the distribution of calories for a recipe in the whole dataset. Again, there is a right skew in our dataset because it makes more sense for a recipe to have 200 - 500 calories as opposed 1000+ calories. The distribution of calories could give possible insight on the distribution of average ratings. For example, if how many calories a recipe has influences the rating they give that recipe.
  <iframe
 src="assets/distribution-steps.html"
 width="800"
 height="450"
 frameborder="0"
 ></iframe>

### Bivariate Analysis

The following graph displays the possible association and relationship between number of steps and time to make. The graph has a pretty even distribution and displays that there isn't too strong of an association between number of steps and minutes to make the recipe. This could give insight that the number of steps doesn't tell too much detail because different steps could take different amounts of time which is why number of steps doesn't influence minutes to make recipe strongly.
 <iframe
 src="assets/relationship-steps-n-time.html"
 width="800"
 height="450"
 frameborder="0"
 ></iframe>
The following graph displays the relationship between number of number of minutes it takes to make the recipe and the average rating for the recipe. The graph displays how there is definitely an abundance of datapoints within the 0 - 1000 minutes area compared to the 1000+ minutes (x-axis) datapoints. However, we can notice in the graph that as the number of minutes increases the number of low ratings decreases overall, but not necessarily the number of low ratings compared to the total number of points in the each x-axis section (i.e 500-1000, 1000-1500, etc). This shows that possibly the number of minutes for a recipe doesn't affect its average rating. 
 <iframe
 src="assets/relationship-avgrating-n-time.html"
 width="800"
 height="450"
 frameborder="0"
 ></iframe>
 
### Interesting Aggregates

The following table shows the average time in minutes to make a recipe in each calorie group:
- 0 - 100 calories
- 101 - 200 calories
- 201 - 300 calories
- 301 - 500 calories
- 501 - 1000 calories

According to the table, it showcases how the average time to make the recipe tends to increase as the number of calories or the calorie group tends to increase. From a logical perspective, it makes sense that meals that might make up more calories would take longer to make since usually more calories equals more food.
 <iframe
 src="assets/avg-recipe-time-by-calories.html"
 width="800"
 height="450"
 frameborder="0"
 ></iframe>

### Imputation
We did not impute any values because we didn't have any null or missing values in our feature columns. We instead cleaned our values in the dataset to help make our analysis more accurate. 

---
## Framing a Prediction Problem
The question we want to pursue is "How do convenience, complexity, and nutritional value of a recipe impact its overall rating?" The response variable we are predicting is the average rating of a recipe, so this is a regression problem. Average rating can have a value anywhere from 0 to 5, which is a continuous numerical variable. We chose to predict this variable because average ratings are a good way to measure how well-perceived a recipe is. The selected features to help us answer this question are 'minutes,' 'n_steps,' 'Calories,' 'Protein (PDV),' and 'num_reviews.'

The evaluation metric we will use is Mean Absolute Error (MAE). MAE is less sensitive to outliers in the data compared to other regression metrics such as MSE and RMSE. Even after cleaning the data and filtering out severe outliers, there are still natural outliers, such as long cooking times or high calorie counts. This will skew our MSE and RMSE, so using MAE will give us a more generalized understanding of how our model performs.

---
## Baseline Model

Our model is a linear regression model. We split our dataset into subsets: 'X_train', 'X_test', 'y_train', and 'y_test' where 'X_train' and 'X_test' include the columns 'n_steps' and 'minutes', and 'y_train' and 'y_test' contain 'avg_rating' column. We aim to minimize the minimum absolute error of our predicted average value against the recipe's actual average rating value. 

The features in our baseline model are all quantitative. In our dataset, the 'n_steps' is a quantitative discrete value specifically because you can't have a decimal number or fraction for the number of steps. The "minutes" column is also a quantitative discrete value because in our database is represented by only integers. 

We understand that standardizing the numerical features in our linear regression model has no affect on MSE or change the predictions because it is scale-invariant. The standardizing allows us directly compare magnitudes of the features because originally their units are different. It basically makes the coefficients easier to compare. 

We measured the performance of our model based on the Mean Absolute Error (MAE). We trained our model on 80% of the the dataset and tested our model's performance on the other 20% of the dataset. Our reported mean absolute error is 0.4666. This means our model's prediction is off by 0.4666 compared to the actual average rating. I would say our model is slightly subobtimal because both 'n_steps' and 'minutes' kind of capture the same predictive reasoning we had for why these features might affect the average rating of a recipe. We don't believe these features capture the different dimensions or possible reasons that play into the average rating of a recipe. We hope to solve this problem through our final model. 


---
## Final Model

For our final model, we added three new features: 'Calories,' 'Protein (PDV),' and 'num_reviews.'

- 'Calories': The calorie information of a recipe reflects meal satisfaction. Higher calorie meals may be rated higher because they are more filling. Or, lower calories meals may be rated higher because they are seen as healthier.
- 'Protein (PDV)': The protein information of a recipe reflects nutritional value. Higher protein meals may be rated higher because they are more filling, especially to health conscious cooks who are interested in high protein meals.
- 'num_reviews': The number of reviews a recipe receives reflects popularity. More reviews may indicate it is a popular recipe and will receive higher ratings. Or, as psychology shows, people tend to leave negative reviews after negative experiences, which will lead to lower ratings.A

These three features all introduce meaningful context to the satisfaction and nutrition of a recipe, which can impact the recipe's average rating. We hypothesized that adding these to the final model would help improve model performance.

We chose to use Random Forest Regressor because it works well for potentially non-linear relationships. Our chosen features, ‘minutes,’ ‘n_steps,’ ‘Calories,’ ‘Protein (PDV),’ and ‘num_reviews’ may not have linear relationships with our response variable ‘avg_rating.’ Random Forest Regressor is also more robust to outliers, which we have acknowledged exist in our data, compared to other linear models.

The three hyperparameters we chose to use were ‘n_estimators,’ ‘max_depth,’ and ‘min_samples_split.’ ‘n_estimators’ controls the number of trees in the forest, with higher values improving performance but at a computational cost. The scikitlearn documentation uses 100 as a default value, so we tried the values [50, 100, 150] to get a good range. ‘Max_depth’ controls the maximum depth of the three, where higher values risk overfitting the data. We used [5, 10, None], where 5 and 10 are small enough to prevent overfitting on noisy data. ‘None’ allows the tree to grow as much as possible to capture complex relationships. Finally, ‘min_samples_split’ controls the minimum number of samples required to split an internal node. We used [2, 5], where 2 helps the tree grow deeper and capture more complex relationships, but 5 limits growth to reduce overfitting. The best parameters were ‘max_depth’: 5, ‘min_samples_split’: 5, and ‘n_estimators’: 150.

Our final model had an MAE of 0.4649, which is an improvement over our baseline model’s performance of 0.4666. Although this is a small improvement, the inclusion of the three additional features ‘Calories,’ ‘Protein (PDV),’ and ‘num_reviews’ likely helped capture nutritional and popularity-related information that were previously ignored by ‘minutes’ and ‘n_steps’ in our baseline model.
