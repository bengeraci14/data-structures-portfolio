### What Drives MLB Regular Season Wins?
## Research Question 
Which team-level factors (offensive production, pitching, and defense) are the strongest predictors of MLB regular season wins, and do a linear regression model and a random forest model agree on their relative importance?
## Problem Definition
This project predicts MLB win totals for regular seasons using team statistics. The target variable is wins, scaled to 162 games (strike-shortened and 2020 covid year have been excluded). It is a regression problem and not a classification problem. This project matters to me and should matter to front offices because teams have to allocate their resources wisely. Specifically across their hitting, pitching and defense, and knowing what the most important factors and what predicts wins can change where the resources are spent.

## Dataset
The dataset used for this project came from Lahman Baseball data specifically Teams.csv, wheere each row is one team's season. The data comes from 1998-present, giving 29 seasons x 30 years of data. The jey features come from batting, pitching and fielding totals.

## Data Understanding
Wins roughly cluster around 81, with most falling between 65-100, including a few outliers above 110 and below 50. OBP and SLG correlate most strongly with team wins among the offensive stats. ERA and WHIP come through as the most prominent pitching stats correlating to team wins. There are several overlapping stats with counting stats like walks heavily correlating with rate stats such as OBP, and BB/9 and WHIP. Runs scored/allowed are excluded since they correlate so heavily to wins, all other variables would be null.

## Data Prep and Feature Selection 
There were no duplicates. The features included in the project were six offense, OBP, SLG, HR, BB, SO, and SB/game. Along with five pitching, ERA, WHIP, K/9, BB/9, and HRallowed/9. Two defensive stats was included as well, fielding % and Double plays/game. All of the features were standardized for Ridge. The data was split by season, training was done on earlier years and the test data was obtained from later years to prevent data leakage. 

## Baseline and Model Development
The baseline for every team is 81 wins every year since the average requires no team data. Two models were trained: Ridge regression and Random Forest. Ridge's alpha was tuned via cross-validation on training data only. Random Forest settings were set to defaults rather than extensively tuned. Both used the same key features listed earlier.

## Model Evaluation and Selection
RMSE, MAE and Rsquared, since RMSE/MAE report error directly in wins and Rsquared shows explained variance. Both models were compared against the 81-win baseline and each other on test seasons. 

            
                    RMSE    MAE   Rsquared
              Ridge	5.387	4.307	0.825
      Random Forest	5.845	4.534	0.794

<img width="1184" height="484" alt="Image" src="https://github.com/user-attachments/assets/fb00d72f-6909-4b46-a52e-760998c418d7" />
The Ridge model slightly outperforms the Random Forest model in terms of RMSE and the Rsquared value.

## Model Interpretation and Insights
Ridge coefficients show each feature's direction and amount of association with total team wins, Random Forest importances show which features it relies on most.


            
            Ridge |coef|	RF impurity	RF permutation	group
            ERA	1	2	1	Pitching
            OBP	2	3	4	Offense
            HR_pg	3	5	5	Offense
            SLG	4	4	3	Offense
            FP	5	10	9	Defense
            BB9	6	7	7	Pitching
            SO_pg	7	11	11	Offense
            HRA9	8	8	8	Pitching
            SB_pg	9	12	12	Offense
            BB_pg	10	6	6	Offense
            WHIP	11	1	2	Pitching
            DP_pg	12	13	13	Defense
            K9	13	9	10	Pitching

