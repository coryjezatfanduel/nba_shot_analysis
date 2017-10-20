![alt text](https://espngrantland.files.wordpress.com/2014/02/microstats.jpg "Logo Title Text 1")


## Goals of this Analysis
As I'm putting this together on the eve of the NBA 2017 season tipoff, my goal is to use Machine Learning to create a metric to better understand the offensive efficiency of NBA players - specifically related to their shot selection. Traditional metrics that we read in the box score are often result-based, counting metrics. While these metrics are easy to track, they often do a poor job of crediting the "inputs" of a given "outcome" during a play. 

For example, if a defender forces a bad shot, they don't get credit for a block, but they created a much lower probability of success for the offense. Similarly, if a shooter takes quality shots - i.e. those with a high expected value - but does not make the shot, traditional metrics might discourage the player from shooting that high quality shot again. 

Our model attempts to create a metric for each player in 2016 - EVDelta (Expected Value vs. Actual Value). If successful, this metric will take into account all quantifiable aspects of a shot taken in the NBA, and allow us to understand players who overperformed their expectations (got lucky) or underperformed their expectations (were unlucky). 

## Data 
The data used for this analysis is available at nbasavant.com. It contains information on every shot taken since 2014. 

## Model
The key to this analysis is coming up with an accurate probability of each shot being made. This is the crux of our EvDelta metric. In choosing a model to run this classification problem, we need a model that 1) will handle several variables that are correlated, 2) show us which features (variables) are most important to the model and 3) will not be prone to over-fitting this data (since we will use it to predict future performance it must translate to unseen data). For these reasons, we've chosen to use the Random Forest algorithm, which has many of these advantages, and yet is still relatively straightforward to implement. 

Instead of just running one model, we'll actual run several (even hundreds!), and choose the best one. Luckily for us, with a few lines of python, we can test several different models using an approach called GridSearch CV (searching many of the parameters for model like the RF):
``` python
param_grid = dict(max_depth=max_depth_range, min_samples_leaf=leaf_range,n_estimators=n_estimators_range)
grid = RandomizedSearchCV(rf_grid, param_grid, cv=10, scoring='neg_log_loss')
grid.fit(X_train, y_train)
```

## Model Performance
In order to ensure our model is accurate, we will choose the "best" model based on logloss. It is important to choose a proper error metric, as we will choose our best model based on performance against this error metric. Log Loss is suitable for a couple of reasons: 
- Logloss penalizes incorrect which are confident in an incorrect classification (i.e. if our model says a player is 90% to make a shot, but he misses, the model will be penalized more than if the model says he was 55% to make a shot). 
- We could just choose "accuracy" (how many shots did the model get right), but if a sample of shots are made 70% of the time, we could just guess "YES" every time and have 70% accuracy! 

After testing our hundreds of models, we take the model with the best performance on the test data (data is was blind to during model creation). Our best model has a Logloss of XXX:             . As a way to further understand the model's performance, it was XX% accurate in correctly classifying shots as well

Additionally, random forests give us "feature importances" which allow us to understand which features in the model had the most predictive impact (positive or negative). One note, because a RF is not a linear model, we do not receive a "formula" associated with other types of models - we trade off this interpretability for improved accuracy in our mmodel. 

The 5 most important features of our model are: 


# Initial Analysis
Now that we have a solid model to determine shot probabilities, we can do a lot of analysis on understanding player performance based on the input - how likely they were to make a given shot, as opposed to the result - the number of shots they made (which are inevitably a function of statistical variance, often seen as slumps or hot streaks).

The first thing we will do in looking at the 2017 data, is create a metric for each players' Actual Value and Expected Value per shot. Actual value per shot can be calculated as: `(((2P FGM /2P FGA)*2)*(2P FGA / (2P FGA + 3P FGA)) + ((3P FGM / 3P FGA)*3) * (3P FGA / (2P FGA + 3P FG)))




