# Local Linear Regression, or LOESS

Parents: [[regression]], [[smoothing]], dsp (digital signal processing)
Related: [[kernels]]

#regression #dsp #smoothing


**LOESS** stands for **Locally Estimated Scatterplot Smoothing**, and it is the most common way of smoothing time series. But essentially it is local linear regression. Is also sometimes called the **Savitzky–Golay filter**", after a famous dsp implementation from 1964.

The general idea: let's assume that our approximating function h behaves kinda linearly near every point x_i, so that $h(x) ≈ θ_0 + θ(x-x_0)$. But then instead of assuming that θ is the same across the entire space, we will believe  that each x_0 has its own θ, that is valid in the vicinity of x_0. We will define this vicinity using a kernel (making the importance of fitting each (x,y) pair fall off if they are further away from x_0), and we'll also pick a finite kernel, so that we don't have to include every possible point in each calculation. But note ⚠️: in this notation, x_0 is not a learning point (not one of the points in the dataset we're working with, it's the point for which we want to calculate the estimator!)

> That's a huge problem with all these explanations of local regression: they use the **notation** that doesn't match the notation for any other method! Usually you have data (x_i, y_i), and a new point x, and you're trying to estimate h(x), so that it h(x_i)~y_i. But for LOESS we estimate h for each point using a "virtual" linear regression, anchored at this point, that we call x_0 (just because that's the standard notation for linear regression). And then while describing this linear regression we kinda have a "virtual" variable (x) that runs in the vicinity of our anchoring point (x_0). So instead of a standard notation where h(x) approximates points (x_i, y_i), we have h(x_0) approximating (x_i, y_i) by assuming that for each x_0 there's some x that lives in its vicinity, and $θ_0+θ(x-x_0)$ approximates those y that can be found in this vicinity. With ever increasing tolerance (increasingly relaxed attitude), as you go further from x_0 (because kernels).

So, in practice, for every point x_0 for which we want to build an estimation (for each x point on our graph, for example, if we are making a plot) , we find the best set of parameters θ, to build a good linear estimator $h(x) = θ_0 + θ(x-x_0)$.  But to show that only local y points matter, we calculate L2 loss ([[l2]]) with a discounting term called a **kernel** $K$ that quickly drops as we move away from x_0:

$\displaystyle L = \sum_i K(||x_i - x_0||/d)(y_i-h(x_i))^2$, where, again, $h(x) = θ_0 + θ(x-x_0)$, and θ is unique for this $x_0$ in particular. $K$ is a kernel function with quick drop-off, and $d$ is a spatial scaling factor that is typically called **bandwidth**.

The most popular kernels include:
* **Gaussian** (but not for loess)
* **Epanechnikov** kernel: $K(u) = \frac{3}{4}(1-u^2)$ for $|u|<1$, and 0 otherwise.
* **Tri-cubic**: $K(u) = (1-|u|^3)^3$ for $|u|<1$, and 0 otherwise. It looks kinda like a fat hat; almost like a smoothed transition between 3 linear functions: growing, plateau, and symmetrically decreasing.

# Refs

The best practical introduction (in R), with cool intuitive animations:
https://rafalab.github.io/dsbook/smoothing.html

Some reasonable lecture that explains math well:
https://aswani.ieor.berkeley.edu/teaching/SP17/265/lecture_notes/ieor265_lec6.pdf

Reasonable blog post:
https://towardsdatascience.com/loess-373d43b03564

List of popular kernels:
https://en.wikipedia.org/wiki/Kernel_(statistics)#Kernel_functions_in_common_use

Wiki; not too helpful (kinda hard to read):
https://en.wikipedia.org/wiki/Local_regression

https://en.wikipedia.org/wiki/Savitzky%E2%80%93Golay_filter

A pdf of a nice lecture:
http://pj.freefaculty.org/guides/stat/Regression-Nonlinear/Nonparametric-Loess-Splines/Nonparametric-1-lecture.pdf