# Explaining Foul Balls
In baseball, what kinds of pitches are fouled off?
We can try to use PitchFX data to find out!

## Possible approaches
1. Apply a classification algorithm (ex: Random forest classification) to a training set of every single pitch thrown, train the algorithm, assess accuracy, and look into the feature importance to see what leads to fouled-off pitches
2. Look only at foul balls, and use an unsupervised k-means clustering algorithm to explore the different kinds of pitches that are fouled off

Both approaches make sense to me, it just depends on the specific question that's being asked. I'll start with the first approach (classification) for now, but will definitely try the second as well!

## Current progress
* PitchFX data scraped from MLB's site using [this amazing parser](https://github.com/johnchoiniere/pfx_parser).
    * Some small changes needed to be made to the `pfx_parser_csv.py` script, hence my forked version in this repo.
* For now, I've scraped every pitch thrown on Jun 1–3 2016. Will expand this more down the road.
* The column names and metadata for this PitchFX data was somewhat obtuse in the beginning, but I found a [nice glossary explaining all of the terms](https://fastballs.wordpress.com/2007/08/02/glossary-of-the-gameday-pitch-fields/), which was helped enormously.
* Logging my progress in this repo's [Jupyter notebook file](Pitch FX Exploration.ipynb)
* Categorical variables (ex: pitcher/batter handedness, pitch type, etc.) successfully encoded using `pd.get_dummies()`.
* Applied a Random forest classification to predict all pitch outcomes (called/swinging strike, ball, foul, in-play, etc.)
    * Current model has an accuracy of ~0.5, which seems better than random chance (1/5 possible outcomes = 0.2)
        * Data is unbalanced though, need to look into this more.
    * Feature importance assessed
        * Pitch location seems to matter a lot
            * Probably mostly for determing balls vs. strikes, less so for fouls
        * Other features that are somewhat important: velocity, release point (surprising), the batter involved, and the strike count
        * Interestingly, pitch types do not seem to matter! Only location. So that's interesting!
            * Also not important: batter/pitcher handedness, game situation, or park effects
* Applied another Random forest classifier to predict just fouls vs. not-fouls
    * Model re-balanced by calculating sample weights (not-fouls ~5x more frequently represented)
    * This model has a surprising accuracy of 95%!
    * Feature importance not as informative, though pitch location still matters
    * Should look into exactly how these individual features relate to pitch outcomes
    * 'Type confidence' is a very interesting feature to me. In one sense, it's a purely technical features that reflects the clustering algorithms that PITCHfx uses. But it could also affect batters, and their confidence in reacting to a pitch, or their ability to make good contact. Will definitely want to look into this specific feature more.

## Issues
* I have no idea how to deal with the pitch sequence data
    * Ball/strike sequence (strike-ball-foul-strike-out etc.)
    * Pitch type sequence (fastball-slider-fastball etc.)
    * Really difficult to encode these kinds of sequences
        * Seems like it would require a graph of some kind?
    * Too bad, because this does seem like it would be critical to the overall question...so what can be done?
    * Could this be overcome just by using an enormous dataset, and encoding it normally? I doubt it...

## Footnotes
* Roger Shaw
* Currently a fellow at Insight Health Data Science, transitioning from academia into a career in data science
* This is just a side project, for playing around with PitchFX data, classification and clustering algorithms, and encoding categorical features.
