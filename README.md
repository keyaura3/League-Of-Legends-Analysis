# League of Legends Early Statistics Analysis
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
| `xpat10`         | The total experience points accumulated by the player by the 10-minute mark. |
| `csat10`         | The total creep score (CS), which measures the number of minions and monsters killed by the player, by the 10-minute mark. |
| `csdiffat10`     | The difference in creep score at the 10-minute mark compared to the lane opponent or average competitor. |
| `xpdiffat10`     | The difference in experience points at the 10-minute mark compared to the lane opponent or average competitor. |
| `golddiffat15`   | The difference in gold earned by the 15-minute mark compared to the lane opponent or average competitor. |
| `opp_goldat10`   | The total gold earned by the lane opponent by the 10-minute mark.           |
| `goldat25`       | The total amount of gold earned by the player by the 25-minute mark.        |
| `killsat15`      | The total number of kills secured by the player by the 15-minute mark.      |
| `damageshare`    | The proportion of the team's total damage dealt to champions attributed to the player. |
| `kills`          | The total number of kills secured by the player during the game.            |
| `teamkills`      | The total number of kills achieved by the player’s team during the game.    |
| `earnedgoldshare`| The percentage of the team’s total gold earned by the player.               |
| `result`         | The outcome of the game for the player’s team, typically represented as `1` (Win) or `0` (Loss). |


These columns were chosen because they provide insight into the early-game performance of players (`goldat10`, `xpat10`, etc.) and the outcomes of those performances (`result`, `damageshare`, etc.). By analyzing these metrics, the project seeks to uncover how individual contributions at specific points in the game influence overall success.
[Link to Original LoL Dataset](https://en.wikipedia.org/wiki/League_of_Legends) 

## Data Cleaning and Explanatory Data Analysis
### Data Cleaning Steps

The data cleaning process was guided by the goal of making the dataset relevant and usable for analysis while preserving its integrity and handling missing values appropriately. Below is a detailed breakdown of the steps taken:

#### 1. Standardizing Position Names
The `position` column contained abbreviations (e.g., `top`, `jng`) which were inconsistent with the rest of the dataset. To standardize these values, I mapped each abbreviation to its full descriptive name (e.g., `top` → `Top Lane`, `jng` → `Jungle`). This ensures clarity and consistency, which is essential for categorical analysis.

#### 2. Selecting Relevant Columns
The original dataset contained 160 columns, many of which were irrelevant to the analysis. To focus on meaningful variables, I retained only 15 columns that were directly related to in-game performance metrics and outcomes. These included:

- **Quantitative columns**: `goldat10`, `xpat10`, `csat10`, `csdiffat10`, `xpdiffat10`, `golddiffat15`, `opp_goldat10`, `goldat25`, `killsat15`, `damageshare`, `kills`, `teamkills`, `earnedgoldshare`.
- **Categorical columns**: `position`, `result`.

By narrowing down the columns, I reduced the dataset's dimensionality, making it more efficient to analyze while preserving all critical information.

#### 3. Identifying and Separating Player-Level Data
Rows where the `position` column was missing were identified as team summary rows (i.e., not specific to individual players). To handle these cases, I created a new column, `is_player`, that marks whether a row corresponds to a player (`True`) or not (`False`). This separation allows the analysis to focus exclusively on player-level data.

#### 4. Handling Missing Values
- For **numeric columns**, I used mean imputation to fill missing values. This choice maintains the overall distribution of the data while avoiding the introduction of biases that might occur with arbitrary replacements.
- For the **categorical column** (`position`), I dropped rows with missing values. Since there is no reliable way to impute positions without affecting the data's integrity, this step prevents inaccuracies in categorical analysis.

These cleaning steps ensured that the dataset was consistent, relevant, and free from missing values that could skew the results.

  
| position   |   goldat10 |   xpat10 |   csat10 |   csdiffat10 |   xpdiffat10 |   golddiffat15 |   opp_goldat10 |   goldat25 |   killsat15 |   damageshare |   kills |   teamkills |   earnedgoldshare |   result | is_player   |   goldat25_missing |
|:-----------|-----------:|---------:|---------:|-------------:|-------------:|---------------:|---------------:|-----------:|------------:|--------------:|--------:|------------:|------------------:|---------:|:------------|-------------------:|
| Top Lane   |       3228 |     4909 |       89 |            8 |          -44 |            391 |           3176 |       8462 |           0 |     0.278784  |       2 |           9 |          0.253859 |        0 | True        |                  0 |
| Jungle     |       3429 |     3484 |       58 |           -5 |          432 |            541 |           2944 |       8254 |           2 |     0.208009  |       2 |           9 |          0.19022  |        0 | True        |                  0 |
| Mid Lane   |       3283 |     4556 |       81 |            0 |           71 |           -475 |           3121 |       8312 |           0 |     0.252086  |       2 |           9 |          0.210665 |        0 | True        |                  0 |
| Bot Lane   |       3600 |     3103 |       78 |          -12 |          265 |           -793 |           3304 |       9356 |           2 |     0.196358  |       2 |           9 |          0.242201 |        0 | True        |                  0 |
| Support    |       2678 |     2161 |       16 |            1 |         -587 |            443 |           2150 |       5840 |           1 |     0.0647631 |       1 |           9 |          0.103054 |        0 | True        |                  0 |


### Univariate Analysis
To begin the analysis, I examined the distribution of the total amount of gold earned by a player at the 25-minute mark, which is towards the later stages of the game. The original plot exhibited a widely spread distribution, making it challenging to interpret. To address this, I applied a logarithmic transformation to the data, resulting in a more readable and interpretable visualization.
<iframe src="graphs/UniGoldat25.html" width="800" height="600" frameborder="0"></iframe>

### Bivariate Analysis
To explore the factors influencing gold accumulation patterns, I created a boxplot comparing the distributions of gold at 10 minutes and experience points at 10 minutes, segmented by player positions. This visualization reveals significant variability within each role, with notable outliers that suggest certain positions have a disproportionate impact on resource accumulation. Below is the boxplot:
<iframe src="graphs/bivariate2.html" width="800" height="600" frameborder="0"></iframe>
Next, I examined the relationship between kills and match outcomes (win/loss). The boxplot below shows the distribution of kills on winning teams versus losing teams. Winning teams tend to have slightly more kills by the 15-minute mark on average, though the difference is not substantial. This suggests that while kills provide some advantage, they are not the sole determinant of victory.
<iframe src="graphs/bivariate3.html" width="800" height="600" frameborder="0"></iframe>
These visualizations highlight the role-specific impact and nuanced relationships between game metrics, providing deeper insights into gameplay strategies.

### Interesting Aggregates
To further analyze the data, I grouped the dataset by player positions and calculated the following aggregate statistics for each position:
- Average Gold at 10 minutes (Avg Gold at 10)
- Standard Deviation of Gold at 10 minutes (Std Gold at 10)
- Average Experience Points at 10 minutes (Avg XP at 10)
- Standard Deviation of Experience Points at 10 minutes (Std XP at 10)
  
This grouped table provides insights into the variation in resource accumulation by position. The table reveals that:
- Gold at 10 minutes: The average gold earned varies significantly by position, with Support earning the least and Mid Lane earning the most on average.
- Experience Points at 10 minutes: Similarly, the average experience points are also position-dependent, with Support trailing significantly behind other roles.
- Variability: Standard deviations highlight the inherent variability within each role, with Support showing a larger spread in both gold and experience compared to other positions.
  
These findings underscore the role-specific dynamics in gameplay, where resource availability and accumulation are directly influenced by a player’s position. Below is the grouped table:

| Position   | Avg Gold at 10 | Std Gold at 10 | Avg XP at 10 | Std XP at 10 |
|------------|----------------|----------------|--------------|--------------|
| Bot Lane   | 3676.60        | 767.30         | 3645.65      | 1072.68      |
| Jungle     | 3619.29        | 769.53         | 3781.89      | 1022.62      |
| Mid Lane   | 3686.79        | 715.17         | 4828.82      | 605.56       |
| Support    | 2732.44        | 1090.76        | 3004.38      | 1329.97      |
| Top Lane   | 3556.54        | 777.64         | 4774.39      | 651.35       |

## Assessment of Missingness
### NMAR Analysis 
In this dataset, I believe the column `is_player` is Not Missing At Random (NMAR). This column appears to indicate whether the data corresponds to an actual player or not. Missingness in this column is likely tied to the specific value it represents—for example, a value might be missing if the entry doesn't correspond to a player but instead to a bot, substitute, or other non-standard entity. The missingness could depend on unobserved factors, such as how the game records entities that are not considered players, which makes it NMAR.

To potentially classify this missingness as Missing at Random (MAR), additional data would be required. For instance, if there was a column that explicitly identified whether the entry pertains to a bot, substitute, or external data source, it would allow us to determine if the missingness in `is_player` is dependent on this observed data. With this information, we could analyze whether the missingness depends on observable factors rather than unobserved ones.

This analysis helps understand how missing values in `is_player` could be addressed and the potential limitations of interpreting data without additional context.

### Missingness Dependency
To evaluate the dependency of missing values in the dataset, I conducted permutation tests. Specifically, I tested whether the missingness in the `goldat25` column is dependent on the columns `opp_goldat10` and `earnedgoldshare`. Below are the tests, hypotheses, and results.

#### Dependency on `opp_goldat10`
- **Null Hypothesis (H₀):** The distribution of `goldat25` is the same regardless of whether `opp_goldat10` is missing or not.
- **Alternative Hypothesis (H₁):** The distribution of `goldat25` is different when `opp_goldat10` is missing compared to when it is not missing.

##### Results:
- Observed Total Variation Distance (TVD): **0.0894**
- p-value: **0.001**
- **Conclusion:** Since the p-value is below the standard threshold of 0.05, we reject the null hypothesis. This result suggests that the missingness in `goldat25` is dependent on the missingness of `opp_goldat10`.

Below is a plot of the null distribution of the TVD along with the observed TVD:
<iframe src="graphs/tvd1.html" width="800" height="600" frameborder="0"></iframe>

#### Dependency on `earnedgoldshare`

- **Null Hypothesis (H₀):** The distribution of `goldat25` is the same regardless of whether `earnedgoldshare` is missing or not.
- **Alternative Hypothesis (H₁):** The distribution of `goldat25` is different when `earnedgoldshare` is missing compared to when it is not missing.

##### Results:
- Observed Total Variation Distance (TVD): **0.023**
- p-value: **1.0**
- **Conclusion:** Since the p-value is significantly greater than 0.05, we fail to reject the null hypothesis. This indicates that the missingness in `goldat25` is not dependent on the missingness of `earnedgoldshare`.

Below is a plot of the null distribution of the TVD along with the observed TVD:
<iframe src="graphs/tvd2.html" width="800" height="600" frameborder="0"></iframe>

## Hypothesis Test 
### Hypothesis
- **Null Hypothesis:** Players with higher positive differentials (`csdiffat10`, `golddiffat15`, `xpdiffat10`) are likely to have the same amount of `kills` as everyone else. 
- **Alternative Hypothesis:** Players with higher positive differentials (`csdiffat10`, `golddiffat15`, `xpdiffat10`) are more likely to have higher `kills`

### Test Statistic
- I chose the **T-statistic** for this test because it allows us to determine whether there is a statistically significant difference between the means of two populations. This is appropriate for comparing the `kills` of players with high differentials against the rest of the population.

### Methodology
1. I separated the population into two groups:
   - **High Positive Diff Group:** Players with positive differentials (`csdiffat10`, `golddiffat15`, `xpdiffat10` > 0).
   - **Rest Group:** All other players.
2. I calculated the mean `kills` for both groups:
   - Mean kills (High Positive Diff Group): **6.47**
   - Mean kills (Rest Group): **4.35**
3. I performed a two-sample t-test to compare the distributions of `kills` between these groups.

## Results
- **p-value:** **0.0**
- **Conclusion:** Since the p-value is below the standard significance level of 0.05, we reject the null hypothesis. This indicates that there is strong evidence to suggest that players with higher positive differentials (`csdiffat10`, `golddiffat15`, `xpdiffat10`) are more likely to achieve higher `kills`.

## Justification of Choices
- **Hypotheses:** These hypotheses are directly tied to the goal of understanding whether positive differentials in key metrics (gold, experience, and creep score) translate to higher kills.
- **T-statistic:** This choice is well-suited for testing the difference in means between two independent groups.
- **p-value Interpretation:** The p-value indicates the likelihood of observing the test statistic under the null hypothesis. A p-value of 0.0 provides very strong evidence against the null hypothesis, suggesting a significant difference between the groups.

Below is a density plot comparing the empirical distributions of `kills` for the two groups. The observed T-statistic of **53.06** is marked on the plot, further illustrating the difference in distributions.
<iframe src="graphs/hyp4.html" width="800" height="600" frameborder="0"></iframe>

## Framing a Prediction Problem
The goal of this prediction problem is to train a model to predict whether a player’s `goldat25` (a later-game statistic) exceeds a certain threshold based on early-game variables (`goldat10`, `xpat10`, `csat10`) and other features. This is a **binary classification problem**, as the outcome (`goldat25`) is categorized into two classes: players who meet or exceed the threshold and players who do not.

### Response Variable
The response variable for this prediction is **`goldat25`**. It was chosen because it serves as a key performance indicator of a player’s success and progression later in the game. Predicting this value based on early-game statistics is valuable for identifying impactful early-game behaviors and improving strategic decision-making.

### Features
The following features are used in the model, ensuring that only information available at the **time of prediction** (early-game statistics) is included:
- **`csat10` (quantitative)**
- **`goldat10` (quantitative):** 
- **`xpat10` (quantitative):**  
- **`position` (categorical):**
  
These features were selected because they are early-game metrics that can influence mid- and late-game performance and are available at the time of prediction.

### Evaluation Metrics
To evaluate the model, I will use both the **F1-score** and **accuracy** as evaluation metrics:
- **F1-Score:** The F1-score is chosen because it balances precision and recall, making it suitable for binary classification tasks with imbalanced datasets (if one class is more frequent than the other).
- **Accuracy:** Accuracy is included as a baseline metric to measure the overall proportion of correct predictions. While it provides a general sense of model performance, it is less reliable than F1-score when class distribution is imbalanced.

By using both metrics, I can ensure that the model performs well across different aspects of classification.

### Data Splitting
To prevent overfitting, I will split the data into:
- **Training Set:** 80% of the data
- **Testing Set:** 20% of the data

This split ensures sufficient data for training while maintaining enough unseen data for robust evaluation.


## Baseline Model
### Model Description
The current model is a **Random Forest Classifier** used to predict whether a player's in-game performance, based on early-game stats (`goldat10`, `xpat10`, `csat10`), will exceed a certain threshold (`goldat25`). The target variable, `goldat25_class`, was created as a binary classification based on whether `goldat25` exceeds its median value.

### Features in the Model
1. **Features**:
   - `goldat10` (Quantitative): Represents the gold collected by the player at the 10-minute mark.
   - `xpat10` (Quantitative): Represents the experience points accumulated by the player at the 10-minute mark.
   - `csat10` (Quantitative): Represents the number of minions or creatures killed by the player at the 10-minute mark.

### Model Performance
- **Accuracy**: 0.8147
- **F1 Score**: 0.7684

### Assessment of Current Model
The model demonstrates relatively high accuracy and a strong F1 score, indicating that it performs well in predicting whether players will surpass the median `goldat25` threshold. However, while the classification model provides useful binary predictions, it may not capture the underlying nuances of the dataset. Specifically:
- `goldat25` is a continuous variable, and binarizing it might result in a loss of information.
- Regression models could better predict the continuous nature of `goldat25` and provide more actionable insights.

### For the Final Model...
After reviewing the baseline model, it is clear that transitioning to a **regression model** instead of a classification model will allow for direct prediction of `goldat25` values. This approach avoids the need for arbitrary thresholds and retains the richness of the data. The next iteration of the model will focus on:
1. **Building a regression model** (e.g., Random Forest Regressor or Gradient Boosting Regressor).
2. **Refining feature engineering**, potentially adding derived features that may correlate with `goldat25`.
3. **Optimizing hyperparameters** for better performance.

The improvements aim to provide a more precise and interpretable prediction for the final model.

For the baseline model, I used a preprocessor and Random Forest Classifier to train the data on 

## Final Model 
### Final Model Performance
The performance of the final model is summarized as follows:
- **Training RMSE**: 1607.4356
- **Testing RMSE**: 1625.4875
- **Training R²**: 0.9850
- **Testing R²**: 0.9848
- **Mean Absolute Error (MAE)**: 1097.6798

#### Comparison to Baseline Model:
- The **Baseline Model** (Random Forest Classifier) achieved an accuracy of 0.8147 and an F1 Score of 0.7684. While these are strong metrics for classification, they reflect a simplified representation of the problem.
- The **Final Model** significantly improves predictive capability by:
  - Leveraging the continuous nature of `goldat25` for a more precise prediction.
  - Achieving high R² values (0.9850 for training, 0.9848 for testing), indicating excellent model fit and predictive power.

### Conclusion
The switch from a classification approach to a regression model aligns better with the data structure and provides a more nuanced understanding of the relationship between early-game features and mid-game performance. The final model’s high R² and low RMSE demonstrate a marked improvement in predictive accuracy over the baseline model.

## Fairness Analysis

### Groups
- **Group X:** Players grouped by their positions (e.g., Top Lane, Jungle, Mid Lane, Bot Lane, Support).
- **Group Y:** Predicted performance (`goldat25`) based on early-game metrics.

### Evaluation Metric
The evaluation metric used is the **Root Mean Squared Error (RMSE)** for each position. RMSE measures the average difference between predicted and actual values, making it a robust metric for assessing the accuracy of the regression model across different groups.

### Hypotheses
- **Null Hypothesis (H₀):** The model's RMSE is consistent across all player positions, indicating no evidence of unfairness.
- **Alternative Hypothesis (H₁):** The model's RMSE varies significantly across player positions, suggesting potential unfairness.

### Test Statistic
The test statistic is the difference between the maximum and minimum RMSE across positions. This statistic quantifies the variation in prediction accuracy among different groups.

### Significance Level
The significance level (α) is set at **0.05**, which means we will reject the null hypothesis if the p-value is less than 0.05.

### Methodology
1. **Observed Difference:** Compute the observed difference in RMSE across positions using the actual position labels.
2. **Permutation Test:** Randomly shuffle the `position` column 10,000 times to generate a null distribution of the test statistic. For each permutation:
   - Compute the RMSE for each position based on the shuffled data.
   - Calculate the difference between the maximum and minimum RMSE across positions.
3. **P-value:** Calculate the proportion of permutation differences greater than or equal to the observed difference.

### Results
- **Observed Difference:** The observed difference in RMSE across positions (calculated from the original data).
- **P-value:** The proportion of permutations with a test statistic greater than or equal to the observed difference.

#### Conclusion
- **P-value:** The resulting p-value was **0.842**.
  - p-value > 0.05: *We fail to reject the null hypothesis and conclude that there is  no evidence of unfairness across positions.*
This analysis evaluates whether the model's predictive accuracy is consistent across all player positions, ensuring fairness in its application. A low p-value would indicate potential biases, while a high p-value suggests no significant evidence of unfairness.

<iframe src="graphs/fairness2.html" width="800" height="600" frameborder="0"></iframe>

