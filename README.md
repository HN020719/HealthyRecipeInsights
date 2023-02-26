# Recipe And Healthiness
Project for DSC 80 at UCSD \
by Bonnie Li / Nan Huang (b8li@ucsd.edu / n5huang@ucsd.edu)

## Introduction
In this project, we are going to investigate a dataset containing recipes and ratings from food.com. After extracting two raw datasets, one containing recipes and one containing reviews and ratings submitted for the recipes in the first dataset, we merged them and calculated the average ratings for each recipe. The first row of the resulting dataset will be:

| name                                 |     id |   minutes |   contributor_id | submitted   | tags                                                | nutrition                                |   n_steps | steps                                               | description                                       | ingredients                                         |   n_ingredients |   average_rating |
|:-------------------------------------|-------:|----------:|-----------------:|:------------|:----------------------------------------------------|:-----------------------------------------|----------:|:----------------------------------------------------|:--------------------------------------------------|:----------------------------------------------------|----------------:|-----------------:|
| 1 brownies in the world    best ever | 333281 |        40 |           985201 | 2008-10-27  | ['60-minutes-or-less', 'time-to-make', 'course...'] | [138.4, 10.0, 50.0, 3.0, 3.0, 19.0, 6.0] |        10 | ['heat the oven to 350f and arrange the rack i...'] | these are the most; chocolatey, moist, rich, d... | ['bittersweet chocolate', 'unsalted butter', '...'] |               9 |                4 |


<iframe src="assets/correlation.html" width=800 height=600 frameBorder=0></iframe>





