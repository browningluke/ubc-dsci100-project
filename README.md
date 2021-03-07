<!-- #region -->
# UBC DSCI 100 Project: Predicting Tennis Tournament Levels from Player Stats


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


## Preliminary Exploration

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
<!-- #endregion -->
