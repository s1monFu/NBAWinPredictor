# NBAWinPredictor

## Group Members:
Simon Fu, Heidee Xiong, Jack Wallenta, Clayton Goter

## Introduction:
NBA basketball is a sport loved by many throughout the world. Games are played by teams in different states, each with their own unique set of players and skills that change every season. Most fans are excited to see which team will come out victorious in their respective divisions at the end of the season and go as far to make bets on which team will win. Because of this, fans are itching to know what makes a team a winning team to predict the reigning champion. In this report, we examine the different statistics that are collected from the NBA games recorded to see if there is a leading variable that can predict the outcome of the game. We examine every game played in each division and seek to find any patterns over time.

The motivation for our analysis is to figure out what variables give an NBA team a greater chance of winning a basketball game. Our report will be covering NBA games from the 2003-2004 season all the way through the 2019-2020 season. Since last season was odd with an abnormal amount of games and zero fans plus the current season (2021-2022) is still ongoing, we will not be including their games in the data. Even though coaches and players vary each season, we will only be looking at the game statistics in this report. Our questions of interest examine whether the location of a game, such as home or away, influences the chance of a team to win, and whether the conference that a team belongs to is more likely to win. We also will compare different game statistics to help us predict their relationships to a winning team. In order for an NBA team to win a game, it is more favorable for a team to belong in the Western conference, while having higher 3-point and field goal percentages.

 
## Background:

A variety of statistics from each NBA game have been recorded and published since the introduction of the League. For this project, we picked data from every NBA game dating from the 2003-2004 season all the way up to the 2019-2020 season. This data was collected and organized by data scientist Nathan Luaga, which he then published on kaggle here: https://www.kaggle.com/nathanlauga/nba-games. Nathan scraped the data from the NBA’s stat webpage from https://www.nba.com/stats/ to create the csv files used in this report. The dataset is still being updated weekly with games from the present NBA season. Due to the large amount of variables collected, he has divided the data into five different data sets. The datasets we specifically used are games.csv for individual game data, rankings.csv for conference information, and teams.csv for team identification information.

An unusual factor that may affect the interpretation of the results is that the team’s players and reputations can change every season/year, meaning that there is no conclusive consistency for figuring out which team is the “best” or “worst.” We will only be able to conclude what statistics may increase a team’s chances of winning.

Another unusual factor that will affect our interpretation of results is that there is missing data from some of the earlier NBA games (all from 2003). We decided to filter out these values so as to not count these games when we analyze the data, which turned out to be 106 games. This will change the number of games played from the 2003-2004 season, however, we will only be looking at the game data and not season totals. 

Using these statistics, we can answer our questions of interest. For example, we could look at the numbers of total wins for home teams between 2004 and 2020 as well as the total wins for away teams, compare the trends, and determine if home teams win more often than away teams. A less obvious example would be grouping by team, taking each value of "three_pct_home" and "three_pct_away" and plotting them with the percentage on the y axis and team on the x axis and creating a different colored line that plots the the win percentage of a team on the y axis and the team on the x axis. With these lines we can likely determine if there is a correlation between three point percentages and winning.
 
The remainder of the report will examine the following questions about trends in NBA games:
Does the location of a game influence the chance of a team to win?
Do teams in the Western Conference win more often than teams in the Eastern Conference?
Does the team with the higher three point percentage between the two (per game) win more often than the team with the lower three point percentage? Do teams with higher three point percentages win more often overall?
What is the most important statistic that determines whether or not a team wins a game?



```{r, echo=FALSE}
nba_vars = tibble(
  Name = c("SEASON", "Home_team", "Home_team_conf", "Home_Team_ID", "HOME_TEAM_WINS", "AWAY_TEAM_WINS", "Away_team", "Away_team_conf", "Away_Team_ID", "PTS_home", "FG_PCT_home", "FT_PCT_home", "FG3_PCT_home", "AST_home", "REB_home", "PTS_away", "FG_PCT_away", "FT_PCT_away", "FG3_PCT_away", "AST_away", "REB_away"),
  Description = c("The season (year) of the game",
                  "The home team",
                  "The home team's conference",
                  "The home team's unique ID number",
                  "If the home team won (1 = yes, 0 = no)",
                  "If the away team won (1 = yes, 0 = no)",
                  "The away team", 
                  "The away team's conference",
                  "The away team's unique ID number",
                  "The home team's points",
                  "The home team's field goal percentage",
                  "The home team's free throw percentage",
                  "The home team's three point percentage",
                  "The home team's # of assists",
                  "The home team's # of rebounds",
                  "The away team's points",
                  "The away team's field goal percentage",
                  "The away team's free throw percentage",
                  "The away team's three point percentage",
                  "The away team's # of assists", 
                  "The away team's # of rebounds"
                  ))

nba_variables = nba_vars %>% 
  kable(caption = "Key Variables from the NBA Data Set") %>% 
  kable_styling(position = "left", full_width = FALSE,
                bootstrap_options = c("striped"))

nba_variables

```

Using these statistics, we can answer our questions of interest. For example, we could look at the numbers of total wins for home teams between 2004 and 2020 as well as the total wins for away teams, compare the trends, and determine if home teams win more often than away teams. A less obvious example would be grouping by team, taking each value of "three_pct_home" and "three_pct_away" and plotting them with the percentage on the y axis and team on the x axis and creating a different colored line that plots the the win percentage of a team on the y axis and the team on the x axis. With these lines we can likely determine if there is a correlation between three point percentages and winning.
 
The remainder of the report will examine the following questions about trends in NBA games:
Does the location of a game influence the chance of a team to win?
Do teams in the Western Conference win more often than teams in the Eastern Conference?
Does the team with the higher three point percentage between the two (per game) win more often than the team with the lower three point percentage? Do teams with higher three point percentages win more often overall?
What is the most important statistic that determines whether or not a team wins a game?


## Analysis

## 1.Do teams in the Western Conference win more often than teams in the Eastern Conference?

<img width="675" alt="Screen Shot 2021-12-28 at 12 35 29 PM" src="https://user-images.githubusercontent.com/49095933/147596681-c47ccb97-4c6b-4732-94db-4d6f30f0ff6b.png">

We infer that Western Conference teams win more often than teams in the Eastern conference. To explore this question, we sorted each team by their winning percentage then created a bar graph, setting the variable (“NICKNAME) to the x-axis, and (“win_pct”) to the y-axis. We then color coded each bar to its respective conference of East or West. We also made sure to include a horizontal line on the graph at the 50 mark to help visualize the teams whose winning percentages were greater than 50%. We also calculated the overall winning percentage for each conference and added horizontal lines for each conference to visualize the difference, with the Western conference having an overall winning percentage of 52.06%, and the Eastern conference with an overall winning percentage of 47.75%. While these numbers are close, teams in the Western conference are more likely to win than teams in the Eastern conference. Additionally, based on the bar graph we made, it can be concluded that teams in the Western conference win more often than those in the Eastern, with 10 Western teams and 6 Eastern teams having above a 50% winning percentage. This also means that 9 Eastern teams and 5 Western teams have below a 50% winning percentage. 


## 2.Does the location of a game influence the chance of a team to win?

![image](https://user-images.githubusercontent.com/49095933/147596823-7f912ed5-f7b2-4e8e-bc95-c4f41a29e755.png)


We infer that a team is more likely to win if they are playing at home rather than away. To answer this question, we made a bar comparing each team to its percentage of wins that were home games, setting the team on the y-axis, and the percentage on the x-axis. We also added a vertical dashed line on the bar graph at 50% to indicate where the total percentage of wins that were home games were more than half for each team. Based on the bar graph, all 30 teams had over 50% of total wins that were home games, meaning that the location of a game does influence the chance of a team to win and that a team is more likely to win a game if they are playing at home.

## 3.Do teams with higher three point percentages win more often overall?

As a group, we predicted that teams with higher three point percentages overall win more than teams with lower three point percentages. To visualize our prediction, we plotted each team’s win percentage (y-axis) vs. their three point percentage (x-axis), adding dashed lines for the NBA averages to easily see which teams are above or below average for each statistic. Lastly we added a regression line to show the relationship between the two variables. We see this line is positive, meaning that as a team’s three point percentage increases we can expect their win percentage to increase too . It is worth noting that we did not have three-pointer-attempts or three-pointers-made data, so the three_pct variable is an average of the home and away percentages. The win percentage is accurate as it was made using the number of wins and number of total games for each team.

## 4.What is the most important statistic that determines whether or not a team wins a game?



Based on our last graph we naturally inferred that three-point percentage would be the most important statistic in determining whether a team wins. To test which game statistics had the most effect on team wins, we grouped our data by team and by season, totaled the in-game statistics provided in the data sets, and calculated each team's winning percentage. The transformed data now contained points per game, rebounds per game, assists per game, three-point percentage, and win percentage for each season in our dataset. We then ran a linear regression for each of the season statistics on a team's win percentage. Finally, we plotted the correlation coefficient of each of these regression variables onto a bar graph to easily compare the regression models. At first glance, we noticed that three-point percentage had the highest correlation to win percentage with an R-squared value of 0.2069. On top of this, we noticed that the statistic with the lowest correlation to win percentage was rebounds per game with an R-squared value of 0.0463. However, none of the statistics had a higher correlation than 0.21.


# Discussion

## 1.Does the location of a game influence the chance of a team to win? Do teams in the Western Conference win more often than teams in the Eastern Conference?
Based on the graph from the NBA games between 2004-2020, we can see that on average, a team in the Western conference wins more often than a team in the Eastern conference. A potential short-coming of this analysis is that it only covers the time period from 2004-2020. While this analysis may be true, it is only based on more recent data and does not take into consideration any of the games from the 20th century. However, this poses the question of whether what conference a team belongs to is associated with a particular advantage, and also begs the question as to why the teams in the Western conference tend to win more often. It would be interesting to look at data that maps out certain trends between the two conferences, like looking at which conference the best NBA players tend to play in, to refine our understanding. Overall, according to our graph, it seems that being on a team in the Western conference will correlate to a higher winning percentage.
 

## 2.Does the location of a game influence the chance of a team to win?
As shown on the graph, an NBA basketball team is more likely to win if the team is playing at home rather than away. A short-coming of this analysis is that it does not answer the question of why this is, nor does the dataset contain any statistics to answer this question. However, it does provide us with future directions for additional research that could help us define what variables would contribute to this finding. For example, we could look at ticket sales for each home game to see if the number of fans at each home game contribute to a positive environment for the players, allowing them to perform better. We could also collect data on players’ level of unease, or uncertainty, at away games to see if that also affects their performance. We think there is a lot of potential here for future work that would be interesting to understand why teams do better if they are playing at home rather than away. All in all, each team in the NBA has a better chance of winning a home game, rather than an away game.

## 3.Do teams with higher three point percentages win more often overall?
In our graph of win percentage vs. three point percentage we can see a positive regression line. This linear regression line supports our hypothesis as it shows higher three point percentages tend to lead to higher win percentages. Using coef(), we can see that this line has a slope of 4.81. This means for every 1% increase to a team’s three point percentage we can expect their win percentage to increase by 4.81%.
 
## 4.What is the most important statistic that determines whether or not a team wins a game?
By comparing the correlation coefficients of each variable, we can see that three-point percentage is the statistic that is most correlated with win percentage. Because it has the highest R-squared value, we now know that a team improving their accuracy at the three-point line will be more likely to have a positive effect on their winning percentage when compared to the other statistics such as points, assists, and rebounds per game. We also discovered that a team improving their rebound rate is the least likely to see an improvement in winning, due to the very low R-squared value for the rebound regression model. However, because the correlation coefficients for all the variables are very small values, the statistics in our dataset all have weak but positive correlations. To further answer this question, in the future, we can find a dataset with more unique and advanced statistics. As the correlation values were all very low, there are most likely certain statistics that are more important to winning a game than those provided in our dataset. Another interesting way we can further test this question is by splitting the data up by home vs away games so we can see how these statistics differ depending on the location of the game.

# References
We are using data from Kaggle found here: https://www.kaggle.com/nathanlauga/nba-games

This data set was created by Nathan Lauga who scraped the data from the NBA's stat webpage: https://www.nba.com/stats/ to create csv files.
