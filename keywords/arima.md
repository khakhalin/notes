# ARIMA and ARMA

Parent: [[time-series]]
See also: [[holt-winters]]

#timeseries


**ARIMA** (Autoregressive Integrated Moving Average) is a generalization of **ARMA** (Autoregressive Moving Average), which in turn consists of **AR** (Autoregression) and **MA** (Moving average).

# AR

Necessary definition: **stationary process** (weak stationarity): the mean of process values and covariances between process values with lags k (k<K) are both constant in time (don't change with time).

**AR** (Auto-regression): run a [[linear_regression]], expressing the value of x at point t through mean μ and deviations of previous p values from this mean (y-μ). Note that this reference to μ means that the process has to be stationary (μ should exist).

While we can simply use last p values, it's actually better to place these inputs strategically, as for seasonally changing data for example last few points (for a short-term trend) + some matching seasons in the past (for seasonality and long-term trend) may work way better. How to choose these points? For example, use [[pacf]] (**Partial Auto-Correlation Function**), and only include lags that give statistically significant partial correlations. Some tutorials seem to claim however that if you don't use consecutive past values, it's already SARIMA, aka "Seasonal" model.

> A question here, why not just use [[regularization]]? L1 if we want a sparse representation?

A covariance matrix that you have defines whether the process is stationary. If eigenvalues are real and <1, we have a relaxation. If some are complex, we have an oscillation. If real part is >1, it diverges and the solution isn't stationary.

> Why do we have to shift everything by μ, why wouldn't we just do a linear regression of y[t] from past y[t-i]? Wouldn't it be both more expressive, and way more obvious; like a primitive single-linear-layer convolution network? Or is the problem here is that we'll get diverging series all the time?

> How often are parameters of AR or ARMA models updated? Do we update them at every time step, kinda like in exponential models? Is it the difference vs convolutional approach; that training and inference are blended together in one time-loop? Or is it not the case?

# MA

**MA** (Moving average): calculates a linear combination of last k errors (errors = observed values minus predicted auto-regressive estimations): `e = y - AR`. These "errors" are also sometimes called **innovation** or **shock**. The intuition seems to be that it's a different product that sits "on top" of a slower product, and has its own covariance matrix (probably something relatively simple, like an exponential relaxation). 

MA models are stationary (in a weak sense), as for a good AR model, errors are normally distributed around 0, and so MA model is a linear combination of a finate number of stationary distributed random values. In practice coefficients for the MA model are fit using max likelihood. To estimate the number of lags that need to go into MA model, use ACF (rather than PACF).

> Why? Why ACF here, but PACF for AR?

# ARMA

**ARMA(p, q)** uses p last predictions for an autoregressive model (AR), and q last errors ($ε = y-\hat{y}$)  for a moving average-like (MA) model of small perturbations. So essentially we have a combination of two stationary models: one is an auto-regressive process that can support oscillations (seasonality) etc; and another faster process that models relaxation of perturbations.

In more mathy intros, these models are described using **backshift operator** and polynomials. So instead of writing $\sum x_{t-i} φ_i$  we write $φ(B)x$, where φ is a polynomial $\sum φ_i x^i$, and B is defined as $B^k x_t = x_{t-k}$. It is not immediately obvious to me how this notation helps, but that's what they seem to mean.

# ARIMA

AutoRegressive Integrated Moving Average = ARMA + non-stationary drift. Three parameters: ARIMA(p, d, q), where p and q are the same values as in ARMA, and d is how many times you differentiate the y sequence before feeding it to ARMA, killing long-term derivatives (trends in case of d=1, gradual accelerations of trends in case of d=2 etc.).

# Refs

Nice lecture slides: http://hedibert.org/wp-content/uploads/2016/04/ar-ma.pdf

Mathy lecture notes on ARMA, from Wharton College: http://www-stat.wharton.upenn.edu/~stine/stat910/lectures/08_intro_arma.pdf

Building ARIMA from scratch in Python: https://laptrinhx.com/arima-model-from-scratch-in-python-3262984784/

Wiki
* https://en.wikipedia.org/wiki/Autoregressive%E2%80%93moving-average_model	
* https://en.wikipedia.org/wiki/Autoregressive_integrated_moving_average

Some videos (lecture series); doesn't really describe the motivation / intuitions, so only somewhat helpful:
* Autoregressive model: https://www.youtube.com/watch?v=5-2C4eO4cPQ
* Moving average model: https://www.youtube.com/watch?v=voryLhxiPzE
* ARMA: https://www.youtube.com/watch?v=HhvTlaN06AM
* ARIMA: https://www.youtube.com/watch?v=3UmyHed0iYE