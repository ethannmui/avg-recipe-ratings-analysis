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



---
## Framing a Prediction Problem


---
## Baseline Model



---
## Final Model
