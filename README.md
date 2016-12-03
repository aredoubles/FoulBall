# Foul ball
In baseball, what kinds of pitches are fouled off?
We can try to use PitchFX data to find out!

## Possible approaches
1. Apply a classification algorithm (ex: Random forest classification) to a training set of every single pitch, train the algorithm, assess accuracy, and look into the feature importance to see what leads to fouled-off pitches
2. Look only at foul balls, and use an unsupervised k-means clustering algorithm to explore the different kinds of pitches that are fouled off

Both approaches make sense to me, it just depends on the specific question that's being asked. I'll start with the first approach (classification) for now, but will definitely try the second as well!

## Current progress
* PitchFX data scraped from MLB's site using [this amazing parser](https://github.com/johnchoiniere/pfx_parser).
    * Some small changes needed to be made to the `pfx_parser_csv.py` script, hence my version in this repo.
* For now, I've scraped every pitch thrown on Jun 1–3 2016. Will expand this more down the road.
* Categorical variables (ex: pitcher/batter handedness, pitch type, etc.) successfully encoded using `pd.get_dummies()`.
* Applied a Random forest classification to predict pitch results (called/swinging strike, ball, foul, in-play, etc.)
    * Current model has an accuracy of ~0.5, which is definitely better than random chance (1/5 possible outcomes = 0.2)
        * Data is unbalanced though, need to look into this more.
        * Model is currently predicting all five of the possible outcomes. Could limit this to just foul balls, and not-foul balls.

## Issues
* I have no idea how to deal with the pitch sequence data
    * Ball/strike sequence (strike-ball-foul-strike-out etc.)
    * Pitch type sequence (fastball-slider-fastball etc.)
    * Really had to encode these kinds of sequences
        * Seems like it would require a graph of some kind

## Footnotes
* Roger Shaw
* Currently a fellow at Insight Health Data Science, transitioning from academia into a career in data science
* This is just a side project, for playing around with PitchFX data, classification and clustering algorithms, and encoding categorical features.
