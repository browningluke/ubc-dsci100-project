# UBC DSCI 100 Project: Predicting Tennis Tournament Levels from Player Stats

<!-- #region -->
## Introduction
The ATP is a tennis organization that organizes and runs various tennis tournaments. Each tournament is categorized in the following way:
- G: Grand slams
- M: Master 1000s
- D: Davis cup
- F: tour finals + other ending events
- A: other events

We wish to determine if there is a connection between the tournament level and the statistics of the game and players. This leads us to the question: 

> Can we predict the level of a tennis tournament from game length and the players’ ages and heights?

The dataset contains the game results and player information for the Top 500 players from 2017-2019.  It contains stats from both the winner and loser of a match at a tournament level. It was compiled [here](https://github.com/JeffSackmann/tennis_atp) by Jeff Sackmann.

### Distribution of tournament levels problem

In our preliminary analysis, we found that the distribution of tournament levels in our dataset was not equal. We saw that the D and F tournament levels had many orders of magnitude fewer observations than others, and G and M tournament levels had noticeably fewer observations than A. We had three possible solutions to this:


1) Upsample our entire dataset to match the number of observations of A,  
2) Drop D and F and upsample the remaining dataset to match A,  
3) Drop D and F and do **not** upsample the dataset.  

After experimenting with the performance of the model for each of these possible solutions, we decided to choose solution 3. Upsampling the entire dataset was never a feasible option, as our training dataset only contained 56 D and 15 F observations, so to upsample this to around 1000 would be meaningless. Dropping (D)avis Cup & (F) tour finals + other ending events from our data set results in a higher accuracy, since attempting to fit our model with so few of these observations would only result in our model performing worse overall, and by narrowing our approach, we can have a more successful and useful model. 

From here, we had the option of upsampling the data with the remaining tournament levels (G, M & A). We found that, since we were effectively doubling the number of G and M observations with zeroed data, the accuracy dropped substantially for all values of K. For completeness sake, the upsampled tuning results are shown alongside the non-upsampled ones in the **Method** analysis below. Since we wanted a model that performed as best as possible, we decided to choose solution \#3, as it gave us the highest accuracy.

### The Model Outline

This is a classification problem, where we will use the K-nearest neighbors algorithm. To determine the optimal value of K for our model, we will try *15* values of K from 1 to 15, cross-validating with 5 folds. This will give us our K to use for our final model.

Since we only care about the variables which physically describe the players and the game we chose the following variables:
- Tournament level (G, M or A)
- Winner and loser height (centimeters)
- Winner and loser age (years)
- Game length (minutes)

We dropped columns in the dataset which were irrelevant to us, such as the players’ name and country.

The split percentage we chose is 75-25 train-test. This is because we want as much of the dataset to be used to train the model as possible, since the test dataset is only used to calculate the accuracy of the model. A higher percentage in the train dataset (within reason) results in the model having more data to fit on, increasing the accuracy.

If we count the number of empty values in our dataset (NAs), we find that there are 2196 in the winner height column, 2718 in the loser height column and 116 in the minutes column. Since we are using these columns to predict the tournament level, these rows with missing values contribute nothing and can be dropped from the dataset. 
<!-- #endregion -->

## Analysis

See [project notebook](https://github.com/browningluke/ubc-dsci100-project/blob/main/tennis.ipynb).


## Methods
This is a classification problem, therefore we will use the k-nearest neighbors algorithm. To determine the optimal value of k for our model, we will try **10** values of k, cross-validating with 5 folds. This will give us our k to use for our final model.

Since we only care about the variables which physically describe the players we chose the following variables: 
- Tournament level (Being classified)
- Winner and loser height (centimeters)
- Winner and loser age (years)
- Game length (minutes)

We dropped columns in the dataset which were irrelevant to us, such as the players’ name and country. We will also need to drop quite a few rows to ensure that the distribution of levels is roughly equal, which it is not currently as seen in the plot above.

### Visualization

We will visualize the results as follows: 
- accuracy table: since we are mainly concerned as to whether or not we are able to predict the level, we will need to determine if the accuracy of our model is above a certain threshold (better than 60%)
- confusion matrix: this will let us see which levels the model having difficulties predicting, thus allowing us to make targeted improvements.  


## Importance
From looking at the plots, we can see that there are slightly less younger players that lose, and that D and G tournaments have longer matches than other levels. We expect that we will be able to predict tournament levels. However, after looking at the plots, we do not expect that the players’ height will be a very useful predictor in the end.

Since most of which are characteristics of the players, it could signify that there are characteristics more prevalent and/or suited in certain tournaments. For example, we could find that an older and/or taller player is less likely to be in the higher tier tournaments than a younger and/or shorter player. This could give players ideas as to how they’re likely to progress as they age and to what extent they’re limited by their height.

Future questions this could lead to include: 
- What is the optimal age for a tennis player in each level?
- Is there an optimal height for each level?


## Discussion

After conducting the analysis, we found that the results differ substantially from what we expected. We found that we are unable to accurately predict the tournament level based on the players’ age, height, and the length of the game. We were only able to get around a 55% prediction accuracy (only slightly better than chance) as seen in the accuracy table, and the confusion matrix shows that the model had trouble correctly predicting G and M level tournaments.

We can get some insight as to why our model failed to predict the data by looking at the initial visualization (player age vs. length & player height vs. length). There is not a lot of variance between the three levels. Tournament levels A and M are very similar, and G is slightly more spread out. This provides an explanation as to why the model was able to predict G slightly better than M. Furthermore, there are a lot more data points for the A level than the others, giving it more weight in the model. We would not have had any better results if we had chosen to upsample the data, since we would have effectively been doubling the other levels with zeroed data, which would not help the model predict the differences. The similarities between each tournament level coupled with the tournament level number imbalances give rationale as to why the model was not able to accurately predict each level.

In going into this analysis, since most of the predictor variables are characteristics of the players, we assumed that these variables could be used to signify that there are traits more prevalent and/or suited in certain tournaments. For example, we thought we would find that an older and/or taller player is less likely to be in the higher tier tournaments than a younger and/or shorter player. If this was true, this could give players ideas as to how they’re likely to progress as they age and to what extent they’re limited by their height as it would be able to determine their future performance. However, since our model failed to reach even a moderately high level of accuracy (only slightly better than chance), we cannot determine if these traits of the players are representative of the tournament level they play in. We would need more data that is evenly distributed between the tournament levels (removing the need to do upsampling) to determine if our model would ever be able to accurately predict the tournament level of a player.  

We found that others have similarly devised models to predict factors of a tennis match. For example, Lisi and Grigoletto developed a model to predict the length of a tennis match based upon the features of the players. They found that they were able to predict the length with reasonable accuracy. The fact that they were successful in getting a model to predict accurately indicates that there is a connection between the players’ characteristics and the factors of the game. On the other hand, Kramer and Huijgen discussed the prediction of current and future tennis performance based on factors such as age, maturation and physical fitness. They found that, while there was a correlation between tennis performance and the factors they were measuring, they were unable to predict tennis performance three years later based upon the measurements. What we can deduce from these two studies is that statistics about the players in a tennis match can be used to determine factors about the game but not necessarily the players’ current and future performance. The direction of effect might be one such that the players’ characteristics affect the game’s statistics and tournament level instead of vice versa, which is what we were measuring.

From these two papers, along with the results from our analysis, it is reasonable to conclude that we cannot accurately determine the tournament level in which a player is in. One theory as to why this is, taking into consideration the limitations of our model, is that the tennis tournament levels are abstractly created; and not divided based upon the players skills. Therefore, trying to determine this arbitrary category by the players’ skills is not feasible. Another theory is that the characteristics of the players we chose have very little connection to the players’ performance and thus cannot successfully be used to predict the tournament level.

Future questions this could lead to are:
- “Are any of the players’ characteristics able to predict the tournament level of a match?“
- “Is there a correlation between a match’s length and the tournament level of the match?” 
- “Do any players’ characteristics predict their future performance?”
- “Do the characteristics of the two player affect the length of the match?”


## Bibliography
J. Sackman, tennis_atp (2015), https://github.com/JeffSackmann/tennis_atp 

F. Lisi, M. Grigoletto, Modeling and simulating durations of men’s professional tennis matches by resampling match features (2021), https://content.iospress.com/articles/journal-of-sports-analytics/jsa200455 

T. Kramer, B.C.H. Huijgen, M.T. Elferink-Gemser, C. Visscher, Prediction of Tennis Performance in Junior Elite Tennis Players (2017), https://www.ncbi.nlm.nih.gov/pmc/articles/PMC5358024/ 
