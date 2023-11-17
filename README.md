# Side-Analysis-of-League-Of-Legends-2023
This is a DSC80 project of UCSD, and it involves tasks such as cleaning data, performing exploratory data analysis, assessing missingness, and conducting hypothesis tests. The results and insights from these analyses are presented on this website, serving as a comprehensive report of our findings.

Creators: Angelina Zhang and Ziyu Huang
## Introduction

We choose the dataset documenting professional League of Legends games throughout 2023; each game, uniquely identified by a distinct gameid shown in the first column, is represented with ten rows delineating individual player data for competing teams and an additional two rows offering comprehensive team statistics. The dataset boasts 129264 rows and 123 columns with detailed information about each game. With such data, we want to investigate whether there is a correlation between the side of the team and the win rate (question: Would the side of the team affect the win rate?). Furthermore, our investigation extends to exploring potential associations between the identified side-dependent win rate and the nuanced disparities in resource allocation across the game map. The differences involve explicitly scrutinizing the variances in resources, such as those about baron, dragon, and herald objectives, to discern whether inherent map-related distinctions contribute to divergent team performances based on their designated sides. As a result, we added a new column, called `natural_resources,` dedicated to explicitly recording perceived resources, aiming to encapsulate the nuanced dynamics surrounding resource acquisition.

Our analysis aims to shed light on the intricate dynamics and strategic implications associated with team positioning in the League of Legends competitive landscape by providing a more granular understanding of the impact that map-related differences may exert on team outcomes. 


### Descriptions of Columns
`‘side’` is the side of one team in the game, which is `Blue` or `Red`

`‘result’` is `True` when the team won the game and `False` otherwise

`‘gamelength’` is the total time taken to finish the game

`‘league’` is the league to which the game belongs

`‘golddiffat15’` is the variance in total gold between teams at the 15-minute mark in a game

`‘golddiffat10’` is the variance in total gold between teams at the 10-minute mark in a game

`‘totalgold’` is the cumulative total gold accumulated by a team throughout the duration of the game

`‘earned gpm’` is the earned gold per minute, offering insights into the team’s gold generation rate

`‘barons’` is the number of baron objectives secured by a team in the game

`‘dragons’` is the total number of dragon objectives secured by a team

`‘heralds’` is the number of herald objectives secured by a team in the game

`‘datacompleteness’` is the given completeness of each row in the original dataframe, which contains `complete` and  `partial`

`‘gamelength_new’` is the length of a game and we convert the original `gamelength` into hours: minutes format

`‘natural_resources’` we add this column after imputation to better showcase the relation between side and resources distribution, it contains the sum of `barons`, `dragons`, and `heralds`


## Cleaning and EDA

We cleaned the data first by making a copy of the original dataset. Then we keep relevant columns and rows, to be more specific, those columns described before and rows that contain team summary for each game. We change `‘barons’` and `‘dragons’` from float type into int and change `‘result’` into boolean type. After investigation, we found out that the `‘game length’` column contains the total time of a game in minutes, and we chose to keep it to facilitate time comparison but added a new column `‘gamelength_new’` to let it become a readable format. We did not convert `‘golddiffat15’`, `‘golddiffat10’`, and `‘heralds’` because they contain missing values, and we will address that in a later section. We will also add the `‘natural_resources’` after imputation in a later section.

### Here is the head of our `cleaned` DataFrame:

| side   | result   |   gamelength | league   |   golddiffat15 |   golddiffat10 |   totalgold |   earned gpm |   barons |   dragons |   heralds | datacompleteness   | gamelength_new   |
|:-------|:---------|-------------:|:---------|---------------:|---------------:|------------:|-------------:|---------:|----------:|----------:|:-------------------|:-----------------|
| Blue   | True     |         2612 | LFL2     |           -530 |             75 |       72807 |     1028.8   |        1 |         4 |         2 | complete           | 43:32            |
| Red    | False    |         2612 | LFL2     |            530 |            -75 |       62745 |      797.665 |        0 |         3 |         0 | complete           | 43:32            |
| Blue   | False    |         2436 | LFL2     |            673 |           -361 |       80627 |     1339.95  |        2 |         3 |         2 | complete           | 40:36            |
| Red    | True     |         2436 | LFL2     |           -673 |            361 |       77449 |     1261.67  |        1 |         4 |         0 | complete           | 40:36            |
| Blue   | True     |         1980 | LFL2     |          -1901 |          -1001 |       60938 |     1192.85  |        0 |         4 |         0 | complete           | 33:00            |

### Univariate Analysis:

<iframe src="Side-Analysis-of-League-Of-Legends-2023/assets/Winning_Sides_Distribution.html" width=800 height=600 frameBorder=0></iframe>

This pie chart shows the distribution of win rates for the red and blue sides, and we calculated them by separating the cleaned data into two sides and computing the win proportion for each. Two numbers add up to 100% because the total number of games for the red or blue side is the same. 

### Bivariate Analysis:

<iframe src="Side-Analysis-of-League-Of-Legends-2023/assets/Side_Resources_Distribution.html" width=800 height=600 frameBorder=0></iframe>

This bar chart shows the average resources allocated by the two sides after we imputed the missing data and calculated the data for the amount of natural resources.

### Interesting Aggregates

| league     |   complete |   partial |
|:-----------|-----------:|----------:|
| AL         |        346 |         0 |
| CBLOL      |        484 |         0 |
| CBLOLA     |        496 |         0 |
| CDF        |        136 |         0 |
| CT         |         86 |         0 |
| DDH        |        172 |         0 |
| EBL        |        318 |         0 |
| EL         |         82 |         0 |
| EM         |        542 |         0 |
| EPL        |        172 |         0 |
| ESLOL      |        604 |         0 |
| GL         |        178 |         0 |
| GLL        |        328 |         0 |
| HC         |        282 |         0 |
| HM         |        324 |         0 |
| IC         |        134 |         0 |
| LAS        |        504 |         0 |
| LCK        |        974 |         0 |
| LCKC       |       1010 |         0 |
| LCO        |        280 |         0 |
| LCS        |        528 |         0 |
| LDL        |          0 |      1712 |
| LEC        |        574 |         0 |
| LFL        |        484 |         0 |
| LFL2       |        498 |         0 |
| LHE        |        118 |         0 |
| LJL        |        516 |         0 |
| LJLA       |        184 |         0 |
| LLA        |        384 |         0 |
| LMF        |        172 |         0 |
| LPL        |          0 |      1510 |
| LPLOL      |        320 |         0 |
| LRN        |        232 |         0 |
| LRS        |        238 |         0 |
| LVP SL     |        492 |         0 |
| MSI        |        152 |         0 |
| NACL       |       1734 |         0 |
| NEXO       |        338 |         0 |
| NLC        |        314 |         0 |
| PCS        |        586 |         0 |
| PGN        |        402 |         0 |
| PRM        |        484 |         0 |
| SL (LATAM) |        180 |         0 |
| TCL        |        360 |         0 |
| UL         |        490 |         0 |
| VCS        |        646 |         0 |
| VL         |        176 |         0 |
| WLDs       |        242 |        26 |


We aim to address the missingness in the columns `'golddiffat15'` and `'golddiffat10'`. To gain insights into the missingness patterns, we examine a pivot table of `'datamissingness'` grouped by league. The resulting table shows that missing values are present only in the leagues `LPL`, `LDL`, and `WLDs`. This observation suggests that the missingness may follow a pattern known as Not Missing at Random (NMAR).
