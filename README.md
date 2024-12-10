# League of Legends Game Contribution Analysis
Final Project for DSC 80 at UCSD 

Matti Key

## Introduction 
In this project, I will be analyzing the League of Legends (LoL) 2022 Competition dataset. LoL is a multiplayer online battle arena (MOBA) game in which the player controls a character ("champion") with a set of unique abilities from an isometric perspective. The game first gained popularity in 2009 and has only grown since, although simply a video game, the game gives lots of insight into how individuals may improve over time, how each position is valued, and how teamwork can make or break a win for a team. 

Throughout this analysis, I will be analyzing player's early game statistics to see how the later game statistics are affected. The results of this analysis can be replicated into real-world themes, like the effectiveness of working in a team in the workplace and having roles set, how early career contributions could be an insight into how an individual may do later in their career, 

The main question being asked throughout this analysis is **how much can early statistics tell us about a player later on**? Throughout this analysis, I will be cleaning data, assessing missingness, competeting a hypothesis test and predictive model to try and answer this question. 

The original dataset contains over 150,000 rows and 160 columns all containing information about each player, their specific statistic, and each game played in 2022. The dataset can be found here: [Link to LoL Dataset](https://en.wikipedia.org/wiki/League_of_Legends) 

### Columns
As stated before, this dataset is vast and contains information about dynamics, comparisons to competitors, team values, and individual values as well. To gain some perspective on the question in focus, these are the columns that I will be using throughout this analysis:

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


## Assessment of Missingness

## Hypothesis Test 

## Framing a Prediction Problem

## Baseline Model

## Final Model 

## Fairness Analysis
