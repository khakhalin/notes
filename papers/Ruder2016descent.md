# Ruder 2016: Overview of optimizers

Ruder, S. (2016). An overview of gradient descent optimization algorithms. arXiv preprint arXiv:1609.04747.
https://arxiv.org/pdf/1609.04747.pdf

This blog post is same text, but easier to read, and updated several times (at least up to 2020 by now):
https://ruder.io/optimizing-gradient-descent/

#dl #optimizers

**Related: **
* [[06_DL]], [[adam]], [[weight_decay]]
* [[Sivaprasad2019optimizers]] - claims that ADAM is usually the best one to use

# Popular optimizers:

* **Batch Gradient Descent**: gradient on the entire dataset, fixed rate, one for all matrices
* **Stochastic Gradient Descent** (SGD): one point at a time. If many points are the same (or very similar), it saves a ton of time. But the function fluctuates heavily. If the learning rate is kept constant, it never fully converges, and ends up oscillating around the minimum.
* **Mini-batch**: something in-between; ~50 to 256 points at a time. Allows parallelization; often is still called SGD. Suffers from same problems: one learning rate for all; either no decrease or scheduled decrease; hard to escape platos on saddle points, especially with local minima.
* **SGD with momentum**: Keep a running average to smoothout oscillations when going through a "ravine" (when slopes across different dimensions are too different). Typically exponential averaging with something like 0.9 decay rate.
* **Nesterov Accelerated Gradient**: calculates gradient not for the current point, but for the estimation of the next point. Essentially it's like first making another step along the gradient, then estimating new gradient, stepping back, updating running average using this "what-if" estimation, and only then making a step for real. Like some weird conterfactual. But apparently it improves performance. Note: no change for linear fragments, but a huge change, potentially even a reversal for tight non-linear landscapes, like local minima and ravines.
* **Adagrad**: adapting learning grade to parameters, depending on how frequently they are 	hit. For infrequent features, updates are larger, for frequent ones - smaller, which works great for sparse data. Here's how: for each parameter, remember the sum of squared gradients in respect to this parameter, then divide by the sqrt of this average. This can be vectorized. With this running sum it naturally slows down with time, and at different rate for diff gradients, but it can never accelerate again.
* **Adadelta** and **RMSprop**: same as above, but with a decaying average of last gradients intead of an ever-growing sum (aka RMS of gradients).
* **ADAM** (see [[adam]]): a combo of momentum and adadelta. "Heavy ball with friction rolling over error surface".
* **AdaMax**: same as ADAM, but instead of deviding on a root of a running average of gradient squares, we divide on the maximum of all absolute-value-gradients observed for this parameter's gradient so far.
* **Nadam**: ADAM + Nesterov approach.
* **AMSGrad**: (Reddi 2018) works better than ADAM for image recognition and machine translation. That happens when a large share of points creates a flat plato for ∇J, but a few points produce a meaningful gradient, yet ADAM misses their effect, as with most gradients vanishing, the second moment goes down, and we divide by it, so the step size goes up, and runs around this saddle point, preventing convergence. AMSGrad adds a simple extra rule: after the running average of second moments is calculated, we only switch to using it if it's bigger than the one used previously. Which means that the learning step never increases. Apparently, this "non-increasing step size" is really helpful for finding narrow minima at the end of a training session.

At theend, the paper also briefly mentions some distributed solutions, curriculum learning (in like 2 sentences), early stopping, and adding noise to the gradient (that makes models more robust to poor initializations).

# Selected formulas

**Nesterov Momentum**
$\displaystyle \begin{cases} g = γg + α\nabla_θ J(θ-γg) \\ θ = θ-g \end{cases}$

**Adagrade**
$\displaystyle θ = θ - \frac{η}{\sqrt{G + ε}}\odot g$, where $G_{ii} = \sum_t{g_t^2}$.

**ADAM**
$\displaystyle g = \nabla J(θ)$

$\displaystyle m = \frac{βm + (1-β)g}{1-β^t}$

$\displaystyle v = \frac{β_2 v + (1-β_2) g^2}{1-β_2^t}$ 

$\displaystyle θ = θ - \frac{η}{\sqrt{v}+ε}m$

**NADAM**
Same as ADAM, but $g = \nabla J(θ-βg)$. The paper also gives some alternative formulations.

# References

Reddi, S. J., Kale, S., & Kumar, S. (2019). On the convergence of adam and beyond. arXiv preprint arXiv:1904.09237.
https://arxiv.org/pdf/1904.09237.pdf
Gives an example when ADAM is bad, but AMSGrad works well.