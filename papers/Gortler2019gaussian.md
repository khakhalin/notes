# A Visual Exploration of Gaussian Processes
Jochen Görtler, Rebecca Kehlbeck, Oliver Deussen
https://distill.pub/2019/visual-exploration-gaussian-processes/
Interactive introduction.

#ml #timeseries

A random process x(t) (note: not one trace, but the whole "process", understood as all possible traces), such that if you pick a vector of random times t_i, and then consider a random vector {x(t_i)}, you find that it is has a multivariate normal distribution. 

> Wait, this part on wiki sounds as if x(t) was univariate, but it kind of doesn't make sense for it to be univariate. Something is off with this definition.

By definition, a multivariate Gaussian is fully defined by a vector of means + covariance matrix. Which makes life easier. The correlation matrix is symmetric and positive semi-definite (_Is it easy to prove?_). The coolest thing about Gaussians is that conditional distributions (lower-dim transects of a multivariate Gaussian) are also Gaussians in their respective dimensions. Moreover, an integral over a few dimensions (aka **marginal distribution**) is also a Gaussian! (_That's probably a direct consequence of the fact that a sum of normals is a normal, as it survives through integration, right?_)

In practice, they are used to estimate the joint distribution of training and testing data P_X,Y so that we could guess test data X from training data Y, under assumption that P_X,Y is a multivariate normal distribution with dim(X)+dim(Y) total dimensions. And so "Guessing X" becomes a task in estimating Bayesian conditional probability P(X|Y). 

> This is confusing. Why would anybody call something you're trying to learn X, and something you know - Y? At first I thought these "distill.pub" people have a typo there, but no, they are consistent, so it's clearly by choice.

"In Gaussian processes we treat each test point as a random variable."

> What? So even for a univariate in->out, and, say, 100 points in the training set, and 50 in the testing, then we have one 150-dimensional vector? And this vector is the _only point_ from the supposedly Gaussian distribution that we have? Why is it even helpful?
> I actually think they have a mistake here, and what they actually mean is that they treat each point as a snapshot of some random variable in time x(t_i). So the entire vector X is assumed to be made of {x(t_i)} where x is a random variable, but if we imagine going from one instantiation of x to another, all points at all t_i would move together. Kinda. What they write later makes more sense if you assume that.

Then what they call a "clever step": we assume a certain behavior of x time, which means that x(t_i) will all be linked ot each other in some fashion. As a Gaussian process is fully defined by its covariate matrix, assuming (or estimating) this covariate matrix locks all points in our dataset to some pattern. This covariate matrix is called a **kernel**. Say, we can assume that each point is similar to the previouis point, and this similarity decreases with time (RBF kernel = gaussian(-delta_time^2)). Or maybe it's periodic, and the system oscillates. Or maybe it's non-stationary, which gives a linear kernal (???). In all these cases, we usually center and normalize the variables to assume that all means are equal to 0. The kernel apparently comes from "domain knowledge". Essentially, the kernel function sets the smoothness, and other properties of the process.

> So does it mean that our training data really actually has to come from past observations of the system, at equal intevals? Looks like that.

Wikipedia describes certain properties a  Gaussian process may have, but does a very  lousy job definint them:
* Stationarity - whether it's bound in the long-term? Brownian motion for example is apparently non-stationary.
* Homogeneity - whether changes in both directions have same properties (something like temporal symmetry?)
* Smoothness
* Periodicity

Now we introduce training points. They constrain the subset of functions that satisfy the kernel, as they have to pass near  the training points (assuming some additive unexplained noise).

**See also:**
* https://en.wikipedia.org/wiki/Gaussian_process
* http://katbailey.github.io/post/gaussian-processes-for-dummies/
* https://blog.dominodatalab.com/fitting-gaussian-process-models-python/