# League of Legends Game Contribution Analysis
Final Project for DSC 80 at UCSD 

Matti Key

## Introduction 
This project focuses on analyzing the **League of Legends (LoL) 2022 Competition Dataset**, which contains detailed statistics for over 150,000 games played in 2022. LoL is one of the most popular multiplayer online battle arena (MOBA) games in the world, captivating millions of players and fans since its release in 2009. The game's competitive nature, reliance on teamwork, and strategic depth make it a rich source of data for studying performance and collaboration.

At its core, LoL is a game of roles, timing, and teamwork, where each player takes on a specific position to contribute to the team’s overall success. The dataset provides a unique opportunity to explore how early-game performance metrics—like gold earned, experience gained, and damage dealt—translate to success later in the game. This analysis could uncover patterns that mirror real-world dynamics, such as the importance of early contributions to long-term outcomes and the value of role specialization in team settings.

The central question guiding this project is: **To what extent can early-game statistics predict a player's performance later in the game?**

This question matters because it goes beyond the game itself. Understanding the relationship between early-game contributions and later performance has implications for broader themes like:
- **Team Dynamics:** How does a team’s early coordination and individual role fulfillment contribute to long-term success? These findings could apply to workplace teams, where early collaboration and clearly defined roles can impact overall productivity.
- **Performance Forecasting:** Can we predict future outcomes based on initial efforts? This concept is relevant to fields like education, career development, and project management, where early indicators often signal future success.
- **Skill Development:** How do early achievements in a structured environment like LoL translate to long-term skill improvement? This could provide insights into training and growth, applicable to sports, education, and even professional development.

By answering this question, this project aims to provide a deeper understanding of performance metrics, teamwork, and the strategic importance of early contributions in both gaming and real-world scenarios.

### Dataset Overview
The dataset comprises over 150,000 rows and 160 columns, capturing granular information about individual players, team dynamics, and game outcomes. For the purposes of this analysis, the following columns are most relevant:

| Column Name      | Description                                                                 |
|------------------|-----------------------------------------------------------------------------|
| `position`       | The role or lane assigned to the player in the game: Top Lane, Jungle, Mid Lane, Bot Lane (ADC), or Support. |
| `goldat10`       | The total amount of gold earned by the player by the 10-minute mark in the game. |
| `goldat15`       | The total amount of gold earned by the player by the 15-minute mark in the game. |
| `xpat10`         | The total experience points accumulated by the player by the 10-minute mark. |
| `csat10_diff`    | The difference in creep score (CS, which measures minions and monsters killed) at the 10-minute mark compared to the lane opponent or average competitor. |
| `goldat15_diff`  | The difference in gold earned by the 15-minute mark compared to the lane opponent or average competitor. |
| `xpat10_diff`    | The difference in experience points at the 10-minute mark compared to the lane opponent or average competitor. |
| `damageshare`    | The proportion of the team's total damage dealt to champions attributed to the player. |
| `kills`          | The total number of kills secured by the player during the game. |
| `teamkills`      | The total number of kills achieved by the player’s team during the game. |
| `earnedgoldshare`| The percentage of the team’s total gold earned by the player. |
| `result`         | The outcome of the game for the player’s team, typically represented as `1` (Win) or `0` (Loss). |

These columns were chosen because they provide insight into the early-game performance of players (`goldat10`, `xpat10`, etc.) and the outcomes of those performances (`result`, `damageshare`, etc.). By analyzing these metrics, the project seeks to uncover how individual contributions at specific points in the game influence overall success.
[Link to Original LoL Dataset](https://en.wikipedia.org/wiki/League_of_Legends) 

## Data Cleaning and Explanatory Data Analysis
### Data Cleaning
The first thing I did when cleaning this dataset was only focus on the columns that I thought would have some kind of significance to the question at hand. So, the first thing I did was only keep the columns, I needed for the analysis and got rid of all the other ones, so now, instead of 160 columns the dataset only has 12. After looking through the data, it became clear that a lot of the values were missing, so for quantitative columns, I decided to use mean imputation to tackle the Null values and for the categorical column (`position`), I decided to drop those columns as there is no way to verify the positions of the missing without affecting the rest of the data. With that being said, the first few rows of the data frame look as follows:

| position   | goldat10 | goldat15 | xpat10 | csat10_diff | goldat15_diff | xpat10_diff | damageshare | kills | teamkills | earnedgoldshare | result |
|------------|----------|----------|--------|-------------|---------------|-------------|-------------|-------|-----------|-----------------|--------|
| Top Lane   | 3228.0   | 5025.0   | 4909.0 | 8.0         | 391.0         | -44.0       | 0.28        | 2     | 9         | 0.25            | 0      |
| Jungle     | 3429.0   | 5366.0   | 3484.0 | -5.0        | 541.0         | 432.0       | 0.21        | 2     | 9         | 0.19            | 0      |
| Mid Lane   | 3283.0   | 5118.0   | 4556.0 | 0.0         | -475.0        | 71.0        | 0.25        | 2     | 9         | 0.21            | 0      |
| Bot Lane   | 3600.0   | 5461.0   | 3103.0 | -12.0       | -793.0        | 265.0       | 0.20        | 2     | 9         | 0.24            | 0      |
| Support    | 2678.0   | 3836.0   | 2161.0 | 1.0         | 443.0         | -587.0      | 0.06        | 1     | 9         | 0.10            | 0      |

### Univariate Analysis

First, I want to look at the distribution of the total amount of gold earned by a player at the 10-minute mark.
<iframe src="graphs/goldat10.html" width="800" height="600" frameborder="0" ></iframe>As shown, there looks to be a "mini" bimodal graph, where a majority of the distribution is between the bins 2k and 5.5k, but another small peak between the bins 14k and 17k, but the graph is still heavily skewed to the right. This could be due to a number of things. 

To get a better sense of the distribution, I want to create a boxplot that contains the distribution of the goldat10 and xpat10 by position to see if that could explain why the historgram above is so spread. <iframe src="graphs/bivariate2.html" width="800" height="600" frameborder="0" ></iframe> Based on this graph, it looks like although the mean for most of the positions in the univariate and bivariate are similar, there are a lot of outliers that influence the distribution. 

### Bivariate Analysis

Now I want to focus on a different column, that may also hold a significant amount of weight when it comes to an individual's statistics, the kills. Here, I created a boxplot that shows the distribution of kills on winning teams vs losing teams. <iframe src="graphs/bivariate3.html" width="800" height="600" frameborder="0" ></iframe> On average, winning teams, tend to have more kills in the beginning of the game, but not by a significant amount. 


### Interesting Aggregates
I grouped together the positions, and found the average goldat10, the standard deviation of goldat10, the average xpat10, and the standard deviation xpat10. With this, we can see that the average gold and experience varies a lot on the position someone is playing, hence showing that not all roles are able to earn the same amount of gold. The pivot table is shown below:

| Position   | Avg Gold at 10 | Std Gold at 10 | Avg XP at 10 | Std XP at 10 |
|------------|----------------|----------------|--------------|--------------|
| Bot Lane   | 3676.60        | 767.30         | 3645.65      | 1072.68      |
| Jungle     | 3619.29        | 769.53         | 3781.89      | 1022.62      |
| Mid Lane   | 3686.79        | 715.17         | 4828.82      | 605.56       |
| Support    | 2732.44        | 1090.76        | 3004.38      | 1329.97      |
| Top Lane   | 3556.54        | 777.64         | 4774.39      | 651.35       |

## Assessment of Missingness
### NMAR Analysis 
I think that the columns 

### Missingness Dependency
In this missingness test, 

Null Hypothesis: The distribution of the `results` is the same when `goldat10` is missing vs when it is not missing.
Alternative Hypothesis: The distribution of `results` is different when `goldat10` is missing vs when it is not missing. 
<>
I observed a TVD of 0.478 with a p-value of 0.0. With these statistics, I reject the null hypothesis and conclude that `results` is dependent on `goldat10`.

Now I will look at the missingness between  `earnedgoldshare` and `result`.
Null Hypothesis: The distribution of the `results` is the same when `earnedgoldshare` is missing vs when it is not missing. 
Alternative Hypothesis: The distribution of `results` is different when `earnedgoldshare` is missing vs when it is not missing. 
<>
conclude that they are not dependent

## Hypothesis Test 
Null Hypothesis: Players with higher positive differentials (`csat10_diff`, `goldat15_diff`, `xpat10_diff`) are likely to have the same amount of kills as everyone else. 
Alternative Hypothesis: Players with higher positive differentials (`csat10_diff`, `goldat15_diff`, `xpat10_diff`) are more likely to have higher kills

For this test, I compared, the differentials of players with above-median kills to those with below-median kills using permutation testing, and having the test statistic be the absolute difference between the observed difference and the given. 


## Framing a Prediction Problem
For the prediction problem, I will be training a model to predict `goldat15`, a later game statistic based on the early game variable (`goldat10`, `xpat10`, `csat10`). To do this, I will be using a binary classifier and I will be using both an F1 score and an accuracy score for this prediction because ... 
I will be using 20% of the data to train and 80% for testing to prevent overfitting the data. The features used were `csat10_diff` (quantitative), `goldat15_diff`(quatitiative), `xpat10_diff`(quantitative), `damageshare`, `position`

## Baseline Model
For the baseline model, I used a preprocessor and Random Forest Classifier to train the data on

## Final Model 

## Fairness Analysis
