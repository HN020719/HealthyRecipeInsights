# Recipe And Healthiness
Project for DSC 80 at UCSD \
by Bonnie Li / Nan Huang (b8li@ucsd.edu / n5huang@ucsd.edu)

## Introduction
In this project, we are going to investigate a dataset containing recipes and ratings from food.com. After extracting two raw datasets, one containing recipes and one containing reviews and ratings submitted for the recipes in the first dataset, we merged them and calculated the average ratings for each recipe. Before calculating the average, we replace all the '0' ratings with 'np.nan'. The first row of the resulting dataset will be:

| name                                 |     id |   minutes |   contributor_id | submitted   | tags                                                | nutrition                                |   n_steps | steps                                               | description                                       | ingredients                                         |   n_ingredients |   average_rating |
|:-------------------------------------|-------:|----------:|-----------------:|:------------|:----------------------------------------------------|:-----------------------------------------|----------:|:----------------------------------------------------|:--------------------------------------------------|:----------------------------------------------------|----------------:|-----------------:|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', 'course...'] | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0] |        10 | ['heat the oven to 350f and arrange the rack i...'] | these are the most; chocolatey, moist, rich, d... | ['bittersweet chocolate', 'unsalted butter', '...'] |               9 |                4 |

We will investigate the relationship between the level of healthiness and the average rating of recipes. If there is a relationship, the reader can quickly decide whether a recipe is healthy based on the rating. If there is no relationship, the reader can ignore the rating when trying to pick a healthy recipe. 

Extract the relevant information from the merged dataset above, we have a dataset with 83782 rows and 4 columns, 'name', 'id', 'nutrition', and 'average_rating', as example below: 

| name                                 |     id | nutrition                                    |   average_rating |
|:-------------------------------------|-------:|:---------------------------------------------|-----------------:|
| 1 brownies in the world    best ever | 333281 | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]     |                4 |
| 1 in canada chocolate chip cookies   | 453467 | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0] |                5 |
| 412 broccoli casserole               | 306168 | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]    |                5 |


A description of each column in both datasets is given below.

| Column         | Description                                                                                                                                                                                                                               |
|:---------------|:------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| name           | Recipe name                                                                                                                                                                                                                               |
| id             | Recipe ID                                                                                                                                                                                                                                 |
| nutrition      | Nutrition information in the form [calories (#),                     total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV),                     carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| average_rating | Average rating of each recipe                                                                                                                                                                                                            |


## Cleaning and EDA
### Data Cleaning

First, we removed all brackets from the "nutrition" column and divided the entire nutrition column into 7 separate columns corresponding to calories, total fat, sugar, sodium, protein, saturated fat, and carbohydrates. Then, we converted all the nutrition columns from string to float.

Since we wanted to examine the relationship between the health level and the average rating of the recipes, we wanted to assign a health level to each recipe based on each nutritional data. In general, the recommended daily calorie intake is 2000 calories for women and 2500 calories for men, so the healthy meal we defined should be below 833 calories. The pdv for each nutrient should be less than 33%. If the nutritional data is within this range, we will give it a True, otherwise False. By adding up the individual nutritional columns, we will get a new column called "healthiness level", ranging from 0 to 7 to represent the level of healthiness for each recipe.

| name                                 |     id |   average_rating | calories   | total_fat_(PDV)   | sugar_(PDV)   | sodium_(PDV)   | protein_(PDV)   | saturated_fat_(PDV)   | carbohydrates_(PDV)   |   healthiness level |
|:-------------------------------------|-------:|-----------------:|:-----------|:------------------|:--------------|:---------------|:----------------|:----------------------|:----------------------|--------------------:|
| 1 brownies in the world    best ever | 333281 |                4 | False      | False             | True          | False          | False           | False                 | False                 |                   1 |
| 1 in canada chocolate chip cookies   | 453467 |                5 | False      | True              | True          | False          | False           | True                  | False                 |                   3 |
| 412 broccoli casserole               | 306168 |                5 | False      | False             | False         | False          | False           | True                  | False                 |                   1 |


### Univariate Analysis

According to the scatter plot, most recipes are found in health levels 1 and 2. Since the number of recipes is decreasing as the health level increases, this trend implies that there are few healthy recipes.

<iframe src="assets/healthiness-level-distribution.html" width=800 height=600 frameBorder=0></iframe>



In the process, we found an outlier with an average rating of 5, and the number of ids was almost half of the total. Since this outlier might affect the shape of the output and the actual result, we took it away. 

|   average_rating |   count |
|-----------------:|--------:|
|          4.97368 |       1 |
|          4.97727 |       1 |
|          4.98113 |       1 |
|          4.99078 |       1 |
|          5       |   47784 |

According to the distribution plot for the average rating of less than 5, the id counts are concentrated in cases with an average rating greater than 3.
<iframe src="assets/average-rating-distribution.html" width=800 height=600 frameBorder=0></iframe>


### Bivariate Analysis

In the scatter plot, the points are very dispersed. However, the regression line shows a weak positive correlation between the average rating and the level of healthiness. The plot implies that the average rating increases as the healthiness of the food increases. 

<iframe src="assets/healthiness-hevel-vs-average-rating-scatter.html" width=800 height=600 frameBorder=0></iframe>

Since the histograms we created were uniform, the average score for each health level was averaged. This finding supports the scatterplot we drew above. The correlation between the health level and the average score is very ambiguous.

<iframe src="assets/healthiness-level-vs-proportional-average-rating.html" width=800 height=600 frameBorder=0></iframe>

### Interesting Aggregates

The pivot table we generated allows us to see that the proportion of average rating for each healthiness level is evenly distributed, which is consistent with the histogram we plotted earlier. Therefore, there is a large uncertainty in the correlation between health levels and average rating.

|   average_rating |
|-----------------:|
|         0.125079 |
|         0.124875 |
|         0.124833 |
|         0.125232 |
|         0.124865 |
|         0.124584 |
|         0.125506 |
|         0.125025 |
