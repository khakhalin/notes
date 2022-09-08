# Holt-Winters triple exponential smoothing method

Parents: [[time-series]]
See also: [[arima]]

#timeseries #stats


A way to predict time-series data using local info (current point), global info (trend) and seasonality.

**Single-smoothing**: just replacing values of $y$ with a running exponential average:
`l[i] = αy[i]+(1-α)l[i-1]` 
At extrapolation, just continue the last value.

**Double-smoothing**: same, but also average the trend, and then at extrapolation continue last value + trend
`b[i] = β(y[i]-y[i-1]) + (1-β)b[i-1]`
It makes sense to make β quite small, so smoothen out the fluctuations.

**Triple-smoothing**:  value, gradient, and seasonality.
For seasonality we we do a running average across seasons, and cancel out the current level of slow trend:
`s[i] = g(y[i]-l[i]) + (1-g)s[i-p]` where p is seasonality period
For level we cancel out seasonality:
`l[i] = α(y[i]-s[i]) + (1-α)l[i-1]`
And the trend is calculated on levels, and not on raw values:
`b[i] = β(l[i]-l[i-1]) + (1-β)b[i-1]`
At extrapolation we just sum all three: `l[last] + b[last]*i + s[last - p + (i mod p)]`
In practice it's easier to do it recursively:
```python
b[i] = b[i-1]
l[i] = l[i-1] + b[i]
s[i] = s[i-p]
```

# Refs

Nice introduction in 3 parts (by Gregory Trubetskoy)
* https://grisha.org/blog/2016/01/29/triple-exponential-smoothing-forecasting/
* https://grisha.org/blog/2016/02/16/triple-exponential-smoothing-forecasting-part-ii/
* https://grisha.org/blog/2016/02/17/triple-exponential-smoothing-forecasting-part-iii/

Some hints about optimizing when the period of seasonality is not known:
* https://orangematter.solarwinds.com/2019/12/15/holt-winters-forecasting-simplified/	

General intro to exp smoothing (except that it has multiplicative rather than additive seasonality)
https://en.wikipedia.org/wiki/Exponential_smoothing