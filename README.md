![alt text](https://espngrantland.files.wordpress.com/2014/02/microstats.jpg "Logo Title Text 1")


## Goals of this Analysis
As I'm putting this together on the eve of the NBA 2017 season tipoff, my goal is to use Machine Learning to create a metric to better understand the offensive efficiency of NBA players - specifically related to their shot selection. Traditional metrics that we read in the box score are often result-based, counting metrics. While these metrics are easy to track, they often do a poor job of crediting the "inputs" of a given "outcome" during a play. 

For example, if a defender forces a bad shot, they don't get credit for a block, but they created a much lower probability of success for the offense. Similarly, if a shooter takes quality shots - i.e. those with a high expected value - but does not make the shot, traditional metrics might discourage the player from shooting that high quality shot again. 

Our model attempts to create a metric for each player in 2016 - EVDelta (Expected Value vs. Actual Value). If successful, this metric will take into account all quantifiable aspects of a shot taken in the NBA, and allow us to understand players who overperformed their expectations (got lucky) or underperformed their expectations (were unlucky). 

## Data 
The data used for this analysis is available at nbasavant.com. It contains information on every shot taken since 2014. 

## Model
The key to this analysis is coming up with an accurate probability of each shot being made. This is the crux of our EvDelta metric. In choosing a model to run this classification problem, we need a model that 1) will handle several variables that are correlated, 2) show us which features (variables) are most important to the model and 3) will not be prone to over-fitting this data (since we will use it to predict future performance it must translate to unseen data). For these reasons, we've chosen to use the Random Forest algorithm, which has many of these advantages, and yet is still relatively straightforward to implement. 

## Model Performance
In order to ensure our model is accurate, we can look at two figures, accuracy and AUC. 
