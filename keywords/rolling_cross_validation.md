# Rolling cross-validation

Parents: [[time-series]], [[validation]]
See also:

#timeseries


With rolling coss-validation you progressively increase the window on which the model is trained (so the window is not sliding, but the left edge is always at the beginning of relevant data, while the right edge is rolling), and then you keep trying to predict the next (or the k-th) value. Then you just average all of the loss values.

# Refs

Sliding window, expanding window, and windows with a gap, illustrated:
https://forecastegy.com/posts/time-series-cross-validation-python/

Lee, T. H. (2007). Loss functions in time series forecasting. Unicersity White Paper, University of Califoria, Dept. of Economics.
Lots of formulas, way too many, too nerdy, can't process it.
http://www.faculty.ucr.edu/~taelee/paper/lossfunctions.pdf