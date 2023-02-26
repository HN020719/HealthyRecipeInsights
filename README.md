# Recipe And Healthiness
Project for DSC 80 at UCSD \
by Bonnie Li / Nan Huang (b8li@ucsd.edu / n5huang@ucsd.edu)

## Introduction
In this project, we are going to investigate a dataset containing recipes and ratings from food.com. After extracting two raw datasets, one containing recipes and one containing reviews and ratings submitted for the recipes in the first dataset, we merged them and calculated the average ratings for each recipe. The first row of the resulting dataset will be:

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

First, we removed all brackets from the "nutrition" column and divided the entire nutrition column into 7 separate columns corresponding to calories, total fat, sugar, sodium, protein, saturated fat, and carbohydrates. Then, we converted all the nutrition columns from string to float.\
Since we wanted to examine the relationship between the health level and the average rating of the recipes, we wanted to assign a health level to each recipe based on each nutritional data. In general, the recommended daily calorie intake is 2000 calories for women and 2500 calories for men, so the healthy meal we defined should be below 833 calories. The pdv for each nutrient should be less than 33%. If the nutritional data is within this range, we will give it a True, otherwise False. By adding up the individual nutritional columns, we will get a new column called "healthiness level", ranging from 0 to 7 to represent the level of healthiness for each recipe.\

| name                                 |     id |   average_rating | calories   | total_fat_(PDV)   | sugar_(PDV)   | sodium_(PDV)   | protein_(PDV)   | saturated_fat_(PDV)   | carbohydrates_(PDV)   |   healthiness level |
|:-------------------------------------|-------:|-----------------:|:-----------|:------------------|:--------------|:---------------|:----------------|:----------------------|:----------------------|--------------------:|
| 1 brownies in the world    best ever | 333281 |                4 | False      | False             | False         | False          | False           | False                 | False                 |                   0 |
| 1 in canada chocolate chip cookies   | 453467 |                5 | False      | False             | False         | False          | False           | False                 | False                 |                   0 |
| 412 broccoli casserole               | 306168 |                5 | False      | False             | False         | False          | False           | False                 | False                 |                   0 |

<iframe src="assets/correlation.html" width=800 height=600 frameBorder=0></iframe>





