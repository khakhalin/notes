# RANSAC - Random Sample Consensus

See also: [[02_Regression]], [[anomaly]]

#stats #bootstrapping #nonparametric


A way to fight outliers while estimating parameters; and we are talking not just 1-2, but a **huge share of the dataset being outliers**. The key intuition is that, if given a chance, "inliers" will all "vote for" (will be in higher accordance with) some ground-truth model (or a family of models that are similar to it?), while outliers will be idiosyncratic, and won't be consistent with most (almost any) models.

Basic algorithm: iterate between two steps:
1. Get a random subset of the data (as small as possible, to fit it with our model. E.g. for a linear model, it would be two points only). Build a model for this data.
2. Go through all other points, and classify them into "outliers" (if they don't fit the model) and "inliers" (if they do). In practice, usually, just a fixed thereshold for maximal deviation from the model.
3. Repeat this until some set of points were classified as "inliers" often enough.
 
The idea is that reasonable models will be oversampled, and so reasonable inliers will be classified as inliers more often. Or we can add them in a greedy way, one at a time, for each "sampled model". Once we have identified enough inliers (more than a certain threshold), and once a consensus model over all inliers fits these inliers well enough, the process is over: we have found a reasonable model.

Conversely, RANSAC can also be used to detect outliers (see [[anomaly]])

The main disadvantage of RANSAC is that it doesn't guarantee a good solution, but is supposed to always improve, so there's no clear way to tell when it is done. There are some mathematical tricks to choose the number of points, based on the [[binomial]] distribution, but one needs an estimate for the ratio of outliers/inliers to use it:

$N=\frac{log(1-p)}{log(1-(1-v)^m)}$ where p is the probability that at least one subset does NOT contain an outlier (usually set at something high like 0.99), and $v$ is the rate of outliers (probability of observing an outlier). _Not sure how exactly it is helpful, given that p is such as weird input for this formula, but apparently people can relate to it?_

RANSAC was introduced in early 1880s, and seems particularly popular with the imaging community, and for compression methods.

# Related

It feels somewhat similar to Kendall-Theil, aka Theil-Sen estimator (non-parametric approach to linear regression), where you draw all possible lines through all possible pairs of points, and then find a median of them:
https://en.wikipedia.org/wiki/Theil%E2%80%93Sen_estimator

# Refs

Derpanis, K. G. (2010). Overview of the RANSAC Algorithm. Image Rochester NY, 4(1), 2-3.
http://www.cse.yorku.ca/~kosta/CompVis_Notes/ransac.pdf

Also: 
* https://en.wikipedia.org/wiki/Random_sample_consensus
* http://homepages.inf.ed.ac.uk/rbf/CVonline/LOCAL_COPIES/FISHER/RANSAC/
* http://www.cse.psu.edu/~rtc12/CSE486/lecture15.pdf