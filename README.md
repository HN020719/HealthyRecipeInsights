# From Kitchen to Your Health: Healthy Recipe Insights
Project for DSC 80 at UCSD \
by Bonnie Li / Nan Huang (b8li@ucsd.edu / n5huang@ucsd.edu)\
Website Link: [https://hn020719.github.io/](https://hn020719.github.io/HealthyRecipeInsights/)

## Introduction
In this project, we are going to investigate a dataset containing recipes and ratings from food.com. After extracting two raw datasets, one containing recipes and one containing reviews and ratings submitted for the recipes in the first dataset, we merged them and calculated the average ratings for each recipe. Before calculating the average, we replace all the '0' ratings with 'np.nan' to get the accurate average rating. If we don't replace them with 0, those recipe without a rating will not be included. The first row of the resulting dataset will be:

| Name                               |   ID   | Minutes | Contributor ID | Submitted   | Tags                                           |
|------------------------------------|-------:|--------:|---------------:|:------------|:-----------------------------------------------|
| Best Ever Brownies in the World    | 333281 |      40 |         985201 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', ...     |

| Nutrition                              | N Steps | Steps                                           | Description                                 | Ingredients                                     | N Ingredients | Average Rating |
|---------------------------------------|--------:|-------------------------------------------------|----------------------------------------------|--------------------------------------------------|--------------:|--------------:|
| [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0] |      10 | ['Preheat oven to 350°F and arrange the rack i... | These are the most chocolatey, moist, rich ... | ['bittersweet chocolate', 'unsalted butter', ... |             9 |             4 |




We will investigate the relationship between the level of healthiness and the average rating of recipes. If there is a relationship, the reader can quickly decide whether a recipe is healthy based on the rating. If there is no relationship, the reader can ignore the rating when trying to pick a healthy recipe. 

Extract the relevant information from the merged dataset above, we have a dataset with 83782 rows and 4 columns, 'name', 'id', 'nutrition', and 'average_rating', as example below: 



| Name                                   |   ID    | Nutrition                                 | Avg. Rating |
|:---------------------------------------|--------:|:------------------------------------------|------------:|
| 1 brownies in the world best ever       |  333281 | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0]   |           4 |
| 1 in canada chocolate chip cookies      |  453467 | [595.1, 46.0, 211.0, 22.0, 13.0, 51.0, 26.0]|           5 |
| 412 broccoli casserole                 |  306168 | [194.8, 20.0, 6.0, 32.0, 22.0, 36.0, 3.0]  |           5 |




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

|   healthiness level |   average_rating |
|--------------------:|-----------------:|
|                   0 |         0.125079 |
|                   1 |         0.124875 |
|                   2 |         0.124833 |
|                   3 |         0.125232 |
|                   4 |         0.124865 |
|                   5 |         0.124584 |
|                   6 |         0.125506 |
|                   7 |         0.125025 |


## Assessment of Missingness
### NMAR Analysis

According to the table below, 'name', 'description' and 'average_rating' have missing data.

| columns        |    0 |
|:---------------|-----:|
| name           |    1 |
| id             |    0 |
| minutes        |    0 |
| contributor_id |    0 |
| submitted      |    0 |
| tags           |    0 |
| nutrition      |    0 |
| n_steps        |    0 |
| steps          |    0 |
| description    |   70 |
| ingredients    |    0 |
| n_ingredients  |    0 |
| average_rating | 2609 |

We consider the 'description' to be the NMAR. Recipe creators can choose not to write a description if they feel it is not necessary.

### Missingness Dependency

Since 'description' is NMAR, we will test the missingness of 'average_rating' column. We believe 'average_rating' is MAR, since it dependent on 'submitted', but not dependent on 'n_ingredients'.

#### average_rating vs submitted

We classify the submissions into two types: early and late. dates before 2013-06-18 are defined as early and dates after 2013-06-18 are defined as late. We want to test whether the average rating is MAR dependent on early and late submissions.

In the table and bar chart below, we show the distribution of early, denoted as True, and late, denoted as False, is very different. 

| submitted   |   rating_missing = False |   rating_missing = True |
|:------------|-------------------------:|------------------------:|
| False       |                0.0457172 |                0.118053 |
| True        |                0.954283  |                0.881947 |

<iframe src="assets/Submitted-Time-by-Missingness-of-Average-Rating.html" width=800 height=600 frameBorder=0></iframe>

Using TVD as test statistics, we get an observed TVD of 0.07233. Then we ran a permutation test with 1000 repetitions, we got a p-value of 0.0 whcih is smaller than 0.01 significance level, so we reject the null.  

<iframe src="assets/Empirical-Distribution-of-the-TVD.html" width=800 height=600 frameBorder=0></iframe>

Since the null stated that the distribution of 'submitted' when 'rating' is missing is the same as the distribution of 'submitted' when 'rating' is not missing. Hence, we conclude that the missingness in the 'rating' column is dependent on 'submitted'.

#### average_rating vs n_ingredients

In the box plot and histogram below, we show the distribution of the number of components corresponding to the missingness of the average rating. The distribution of the average rating's missingness is True, and missingness is False is highly overlapping.

<iframe src="assets/n_ingredients-by-Missingness-of-Average-Rating-hist.html" width=800 height=600 frameBorder=0></iframe>
<iframe src="assets/n_ingredients-by-Missingness-of-Average-Rating-line.html" width=800 height=600 frameBorder=0></iframe>

Also, since the absolute difference between the two group means is 0.2542, which is very small, we decided to use KS as the test statistic to run our permutation test further. 

Since we obtained a p-value of 0.0132, which is greater than 0.01 significance level, we fail to reject the null hypothesis. The null hypothesis states that the distribution of 'n_ingredients' when 'average_rating' is missing is the same as the distribution of 'n_ingredients' when 'average_rating' is not missing. Therefore, we conclude that the missingness of the 'average_rating' column is not dependent on 'n_ingredients'.


## Hypothesis Testing
Again, our question is: what is the relationship between the level of healthiness and average rating of recipes?

Null Hypotheses: The level of healthiness and average rating come from the same distribution. \
Alternative Hypotheses: The level of healthiness and average rating do not come from the same distribution. 

Since we only have samples, but no information about any population distributions, we are going to conduct a permutation test. \
Since the dataset is extensive, more confounding variables may affect the result. We need to use a more strict threshold as our significance level. So we use 0.01 as the significance level, representing higher statistical significance.\
We consider each healthiness level as one group. Since the average ratings for each group are quantitative, we will choose the absolute differences in group means as the test statistic.

The observed differences in the group mean we obtained is 0.342. We ran a permutation test with 1000 repetitions and got a p-value of 0.299, which is much larger than the significance level. We fail to reject the null. Hence, we conclude that the healthiness and average rating may come from the same distribution. As the plot shows below:

<iframe src="assets/Empirical-Distribution-of-the-Absolute-Differences-in-Group-Means.html" width=800 height=600 frameBorder=0></iframe>

## Building Prediction Models

#### Prediction Problem: 
Predicting the average rating of a recipe according to minutes, nutritions(splitted), n_steps, and n_ingredients. 

#### Type:
Regression

We chose the average rating as the response variable because we wanted to further investigate the relationship between the average rating of each recipe and the other variables. We are using RMSE and R^2 as our metrics because both R squared and RMSE provide valuable information about the performance of a decision tree regressor. R squared can indicate how well the model fits the data overall, while RMSE provides information about the magnitude of errors. By considering both metrics, we can gain a better understanding of how well the model is performing and identify areas for improvement.
Since all the data were generated before the time we extracted them, we knew them at the "time of prediction".

#### Data Cleaning
We removed all brackets from the "nutrition" column and divided the entire nutrition column into 7 separate columns: total fat, sugar, sodium, protein, saturated fat, and carbohydrates. Then, we converted all the nutrition columns from string to float, and dropped all the null values.

|   minutes |   n_steps |   n_ingredients |   calories |   total_fat_(PDV) |   sugar_(PDV) |   sodium_(PDV) |   protein_(PDV) |   saturated_fat_(PDV) |   carbohydrates_(PDV) |   average_rating |
|----------:|----------:|----------------:|-----------:|------------------:|--------------:|---------------:|----------------:|----------------------:|----------------------:|-----------------:|
|        40 |        10 |               9 |      138.4 |                10 |            50 |              3 |               3 |                    19 |                     6 |                4 |
|        45 |        12 |              11 |      595.1 |                46 |           211 |             22 |              13 |                    51 |                    26 |                5 |
|        40 |         6 |               9 |      194.8 |                20 |             6 |             32 |              22 |                    36 |                     3 |                5 |
|       120 |         7 |               7 |      878.3 |                63 |           326 |             13 |              20 |                   123 |                    39 |                5 |
|        90 |        17 |              13 |      267   |                30 |            12 |             12 |              29 |                    48 |                     2 |                5 |


## Baseline Model

For our baseline model, we compared Linear Regression and Decision Tree Regressor model's RMSE and R square. It turns out that the linear regression model has RMSE=0.6397 and R^2=-0.0012, and the decision tree regressor has RMSE=0.6391 and R^2=0.0007. Since Decision Tree Regressor has a lower RMSE and higher R^2, we decided to use Decision Tree Regressor. For features, we used 10 features including minutes,	n_steps, n_ingredients,	calories,	total_fat_(PDV), sugar_(PDV), sodium_(PDV), protein_(PDV), saturated_fat_(PDV), and carbohydrates_(PDV). Minutes,	n_steps, n_ingredients,	and calories are quantitative discrete variables, while total_fat_(PDV), sugar_(PDV), sodium_(PDV), protein_(PDV), saturated_fat_(PDV), and carbohydrates_(PDV) are quantitative continuous variables. We didn't transform any of the features, meaning we use the value as it is. We don't believe our current model is good because we are having a R square of 0.0007, which is too low.



## Final Model
We started from conducting grid search with different options of max_depth and min_samples_split to acquire the optimal hyperparameters. It turns out the best max_depth is 2, and the best min_samples_split is also 2. So, in our second version model, we first put all the features in because we believe all these features are related to 'average_rating'. For example, people might tend to rate lower for recipes with bigger 'minutes','n_steps', and 'n_ingredients' because they might think the recipe is too complicated. Or people might prefer certain recipes with certain types of ingredients. So, we used optimal hyperparameters and put all the features as is to generate the model. It turns out the model has a R^2 of 0.0021 and RMSE of 0.6387, which is better than our baseline model. 

However, we still want to improve our model because the R^2 is too low. So, we decided to tranform some of the columns. 

First, after observing the distribution for sugar_(PDV), we found out their relationship with average_rating may be: average_rating = 1/ sugar_(PDV): 

<iframe src="assets/sugar_(PDV)-vs-proportional-average-rating.html" width=800 height=600 frameBorder=0></iframe>


Therefore we decided to apply np.sqrt to it. So does 'calories','total_fat_(PDV)', 'sodium_(PDV)','protein_(PDV)','saturated_fat_(PDV)','carbohydrates_(PDV)'.

Second, We double 'n_steps' and 'minutes' because we believe that when "n_steps" and "minutes" are larger, the recipe is more complex, so its average rating is worse, so we want to double these two features to add their weights. 


## Fairness Analysis

Our group X is recipes with minutes more than 35, and our group Y is recipes with minutes less than 35. For evaluation metric, we choose RMSE to evaluate our fairness. 

- Null Hypothesis: Our model is fair. Its precision for recipe with less than 35 minutes and greater than 40 minutes are roughly the same, and any differences are due to random chance.

- Alternative Hypothesis: Our model is unfair. Its precision for recipe with less than 35 minutes is different from those greater than 40 minutes.

The test statistics we chose is the absolute difference of RMSE between two groups and our observed statistics is 0.0390 .We used a significance level of 5%. After conducting the permutation test, we get a p-value of 0.0, which is smaller than our significance level 5%, so we reject the null. The conclusion is that our model may not be fair when predicting the average ratings of recipes greater than 35 minutes and recipes less than 35 minutes.
