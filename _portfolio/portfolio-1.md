---
title: "Indian Premier League (IPL) Win Predictor"
excerpt: "Assessed accuracies of various classification models to predict the match results of the 2021-22 Indian Premier League (IPL) Season"
collection: portfolio
---


## GitHub Repository:
[Repository](https://github.com/sameerprasadkoppolu/Group-12-IDMP-Project/tree/main)


## Introduction:
Twenty20 Cricket, abbreviated as T20, is a popular format of the sport of Cricket that originated in England in 2003. In recent years T20 appeals to millions of spectators across the globe as it is shorter than the classical cricket match (T20 matches feature just 20 overs per innings) but still manages to hold the thrill of the sport to a high degree. The Indian Premier League(IPL) is a T20 domestic cricket league held annually in India. In this context, we are focusing on predicting the winning team of a given match in the IPL, with respect to the players participating in the match.

Our aim with this project is to predict the winning team for a given matchup in the 2022 IPL season. The winning team is chosen based on team impact scores that are calculated from the impact scores of the individual players that make up the team for a given matchup. In addition to the impact
scores, the home team, the away team, the venue of the match, the winner of the toss, and their decision to bat or bowl first are also considered. The seasons from 2009 to 2021 form the training set while the 2022 season alone is the test set. 


## Data Preprocessing:
As part of the data preprocessing, additional parameters were calculated for batsmen and bowlers in order to arrive at their respective impact scores. Once, the impact scores for both bowlers and batsmen are calculated, their respective relative impact scores are calculated. Thereafter, the relative impact scores of each player for a given season are aggregated with respect to the team that they play for in a given matchup in a given season. This results in the Team Impact Score. Following this, the data is partitioned into a training and test set. The training set ranges from 2009 to 2021 seasons, while the test set is only the 2022 season as the aim is to predict the winner of every matchup in the 2022 season.


## Evauating Batting Impact:
For a given player, the Batting Impact Score is calculated for every season. It is calculated using the following parameters:
* **Batting Strike Rate:** This parameter depicts how fast a batsman scores runs. For a given season, the runs scored and balls faced by a batsman are aggregated and then used to calculate the Batting Strike Rate for that batsman for the season overall.  
*Batting Strike Rate = 100 * (Runs Scored)/(Balls Faced)*
* **Batting Average:** This parameter depicts how much a batsman scores on average before getting out. For a given season, the runs scored and the number of times a batsman has gotten out are aggregated and then used to calculate the Batting Average for that batsman for the season overall.  
*Batting Average = (Runs Scored)/(Number of times Dismissed)*
* **Quality:** This parameter depicts the performance of a batsman in a given season relative to the average batsman who has a Batting Strike Rate of 130 and a Batting Average of 30 per season.  
*Quality = (Batting Strike Rate + Batting Average) / (Average Batsman Strike Rate + Average Batsman Batting Average)*
* **Frequency:** This parameter represents the average number of balls faced by a batsman in a given season. It is calculated as follows.  
*Frequency = (Balls Faced in a Season) / (Matches Played in a Season)*
* **Batting Player Impact:** This is a preliminary impact score that is calculated using Quality and Frequency. It represents the impact a batsman has in a season. It is given by the following formula.  
*Batting Player Impact = Quality * Frequency*
* **100s Factor:** The Hundreds Factor represents the average number of Hundreds a batsman has scored in a given season.
* **50s Factor:** The Fifties Factor represents the average number of Fifties a batsman has scored in a given season.
* **Batting Impact Score:** This is the final Batting Impact score of a batsman in a given season. It is calculated as follows.  
*Batting Impact Score: Batting Player Impact + (Batting Player Impact * 100s Factor) + (Batting Player Impact * 50s Factor/2)*


## Evaluating Bowling Impact:
* **Bowling Economy:** This parameter depicts the rate at which a bowler concedes runs. For a given season, Bowling Economy Rate is given as follows.  
*Bowling Economy Rate = (Runs Conceded by Bowler in a Season)/(Overs Bowled by Bowler in a Season)*
* **Bowling Strike Rate:* This parameter depicts the rate at which a bowler earns wickets. For a given season, Bowling Strike Rate for a bowler is as follows.  
*Bowling Strike Rate = (Balls Bowled by Bowler in a Season)/(Wickets Earned by Bowler in a Season)*
* **Quality:** This parameter depicts the performance of a bowler in a given season with respect to the average bowler. It is given as follows.  
*Quality = 100/(Bowling Economy * Bowling Strike Rate)*  
Here, 100 is the Quality of the Average Bowler. The Average Bowler has a Bowling Economy of 8 and a Bowling Strike Rate of 12. The product of these two parameters is then rounded off to 100.
* **Frequency:** Frequency for a bowler depicts the average number of balls that a bowler has bowled in a given season. It is given as follows.  
*Frequency = (Balls Bowled in a Season)/(Matches Played in a Season)*
* **Bowling Player Impact:** This is a preliminary impact score that is calculated using Quality and Frequency. It represents the impact a bowler has in a season. It is given by the following formula.  
*Bowling Player Impact = Quality * Frequency*
* **4 Wicket Factor:** This parameter depicts the average number of 4 wicket hauls that a bowler has earned in a given season. A four-wicket haul happens when a bowler earns 4 wickets in a single match.
* **5 Wicket Factor:** This parameter depicts the average number of 5 wicket hauls that a bowler has earned in a given season. A five-wicket haul happens when a bowler earns 5 wickets in a single match.
* **Bowling Impact Score:** This is the final Bowling Impact score of a bowler in a given season. It is calculated as follows.  
*Bowling Impact Score = Bowling Player Impact + (Bowling Player Impact * 5WF) + (Bowling Player Impact * 4WF/2)*  
Here 5WF and 4WF are the 5 Wicket and 4 Wicket Factors respectively.


## Evaluating Relative Player Impact:
* We use Relative Player Impact for Batsmen and Bowlers to calculate a Team’s Impact Score based on the playing eleven that is present in a given match in a given season.
* To calculate the Relative Batting/Bowling Impact of a player in a given season, we first calculate the Modified Player Impact of that player, where the Modified Batting/Bowling Impact of a player is obtained by simply considering the Batting/Bowling Player Impact Score of a player in the i-th season to be that of the (i − 1)th season.
* Additionally, the 2008 season of the IPL is dropped as it was the very first season of the league. All missing values for the Modified Batting/Bowling Impact of a player are imputed using the mean of the already calculated values of the Batting/Bowling Impact of that player.
* Relative Batting/Bowling Impact score for each player in each season is calculated with its respective modified impact scores in order to consider the change in it across the seasons that the player has participated in.  
  ![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/3871a5e2-7861-4725-8038-61aece1234a4)
* Upon obtaining the Modified Batting/Bowling Impact score for each player in each season, we then calculate the Relative Batting/Bowling Impact score for each player in each season. This is done because we need to take into account any increase or decrease in the Modified Batting/Bowling Impact score for each player across the seasons of the IPL that the player has participated in.
* For the first season that the player has participated in (excluding the 2008 season), the Relative Batting/Bowling Impact Score is calculated as follows.


## Dataset Partitioning:
The ’all_season_summary.csv’ dataset is partitioned into a training and test set after the impact scores for the home teams and away teams are calculated. Thereafter, the dataset is partitioned. The training set contains observations from the 2009 season till the 2021 season while the test set contains only the observations of the 2022 season.


## Predictor and Response Variables:
* As mentioned earlier, the Team Impact Score for Batting and for Bowling for a given season is calculated as an aggregation of the Relative Batting/Bowling Impact scores for each player on that team in that season. However Relative Batting and Bowling Impact scores were found to have a
right-skewed distribution.
![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/8dbcac3d-6b1a-4ac5-921e-7a01c70f74ca)
![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/324b6e49-fbd4-4102-b562-2db3213176d4)

* Hence, Log Transformations were applied on the Relative Batting and Bowling Impact scores decrease the skewness. Thereafter, the transformed Relative Batting and Bowling Impact scores are summed up for each player in each team for a given season to give the Team Impact Scores.
* The next set of predictors are the home and away teams, toss winner, toss decision, and venue. These are categorical variables and are One Hot Encoded. The dependent variable is ’winner’ and this variable is Label Encoded.
* Observations containing the teams Royal Challengers Bangalore (RCB), Chennai Super Kings (CSK) , Rajasthan Royals (RR), Delhi Capitals (DC), Sunrisers Hyderabad (SRH), Punjab Kings (PBKS), Kolkata Knight Riders (KKR), Mumbai Indians (MI), Gujarat Titans (GT), and Lucknow Super Giants (LSG) are kept because these teams are the most consistently participating teams in the IPL since 2008.
* GT and LSG are considered because these teams, although new, take part in the 2022 IPL season on which we predict the winner of each matchup. The remaining teams from the dataset are not considered as they have participated irregularly across the IPL seasons. When LSG or GT participates, their impact score is considered along with the difference with the corresponding opposition. If this difference is greater than 3.5, (value chosen through trial and error) then the team with a higher score is expected to win.


## Results:
* Various Machine Learning Models were trained and tested and the results were reported as follows
![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/6a4c25e2-74f1-49f4-a480-91c938b99f1f)  
* The AdaBoost Classifier has the highest accuracy with the following Classification Report and an F1 score of 0.59.
![image](https://github.com/sameerprasadkoppolu/portfolio/assets/40263744/b16c293b-7411-4b75-babc-7a22a5497430)
