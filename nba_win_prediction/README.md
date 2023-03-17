NBA Win/Loss Predictive Analysis

This Project is based on NBA game data for the 2022-2023 season. The main goal is to build a model to predict the winner of any matchup based on
past team performance from this season.

Here is a breakdown of the steps taken to complete this project

1. Collect Data

  Data was accessed using the R package 'nbastatR'. This package pulls data from NBA Stats API, Basketball Insiders,
  Basketball-Reference, HoopsHype, and RealGM

2. Clean and Explore

  Filtering for game logs from the 2022-2023 regular season, I wanted to use the plusminus score variable to predict the game outcome (Win,Loss).
  Plusminus is the sum of points for and points against for a team in each game played

3. Feature Engineering

Rolling average plusminus: To create a variable that would represent a teams relative performance leading to a new matchup, I took the rolling average
                       	of their plusminus scores over the past 30 games before the current game.

Avg_30_pm_diff : A comparison between team avg plus minus scores for matchups. Reflects relative performance

Home advantage feature: A common factor in determining team performance is wether or not the team is playing at home or away. Travel fatigue, lack of fan support and the unfamiliar environment in away stadiums can lead to reduced performance.

Back to back games: To increase the accuracy of the model, I included this binary variable that indicates if a team had a game the day before. Back to back
              	games are shown to reduce wins in comparison to non- back to back games over the past 23 years.

4. Modeling

Using the package 'xgboost' I performed a gradient boosting algorithm to predict the game outcome using the above variables. I trained the model on a
training split of the data and validated with a test split.
HyperParameters were tuned using three fold cross validation.
The fully trained model predicted the correct outcome 60.3% of the time, a significant increase from the previous model trained using only Avg_30_pm_diff (52.78%)

5. Usage

I wanted to test my model on upcoming games, using the nbastatR function current_schedule() I created a script that pulls games from the current day, formats them to match the past game logs, and runs the data through the predictive model. I am currently waiting to see how the predicted outcomes align with the actual outcomes in today's games (2023-03-17).

Thanks for reading!
Gareth

