# ADAM

#dl #tools

ADAM comes from **Adaptive Moment estimation**. First introduced in this paper with 43k citations: Kingma, D. P., & Ba, J. (2014). Adam: A method for stochastic optimization. arXiv preprint arXiv:1412.6980.

An alternative to  SGD (Stochastic Gradient Descent). Inherits to Adaptive Gradient (AdaGrad) and RMSProp. Often (but apparently not always) works better than SGD.

Main differences with SGD:
* Uses exponential running average of gradients, which makes it a momentum-based method
* Instead of a single learning rate, separate learning rates for each parameter
* These individual rates are adjusted during the training, based on the exponential average of second moments of gradients m_2. Namely, rate ~ $1/(\sqrt{m_2} + ε)$.

Parameters (that arguably are not designed to be fine-tuned)
* alpha	 - something like "initial learning rate" (typical value of 0.001), but it is corrected later anyways. 
* beta1 - decay rate for averaging gradients (like 0.9) 
* beta2 - decay rate for averaging 2nd moment of gradients (like 0.999)
* epsilon - just a small safety value to avoid 1/0 (typical 1e-8)

As the actual update step is essentially m1/m2 (first moment of the gradient over it's 2nd moment), the step size doesn't depend on the gradient value, but only on its noisiness. Which helps to navigate both parts with large gradients and those with small gradients (saddles, ravines), where SGD typically stalls.

> So my understanding is that because it's essentially mean/sd, we have more optimistic convergence when all gradients are pointing in the same direction, and slower movement when we reach conflicting points. It almost feels like switching to SGD when it's safe, and reverting to batch descent when it's not.

At the same time, for some tasks (image classification) SGD with momentum seems to find lower minima, as ADAM fails to converge (see Wilson 2017, and a review by Bushaev 2018). ADAM is great for early convergence, but at the end SGD may work better.

ADAM with [[weight_decay]] is apparently a promising method (Loshchilov 2017)

# References

Gentle introduction to ADAM, by Jason Brownlee
https://machinelearningmastery.com/adam-optimization-algorithm-for-deep-learning/

Overview of ADAM from 2018, by Vitaly Bushaev
https://towardsdatascience.com/adam-latest-trends-in-deep-learning-optimization-6be9a291375c#:~:text=Adam,itself%20like%20SGD%20with%20momentum.

A generally good recommended review:
Ruder, S. (2016). An overview of gradient descent optimization algorithms. arXiv preprint arXiv:1609.04747.
[[Ruder2016descent]]

Wilson 2017 (that ADAM doesn't always work that well):
Wilson, A. C., Roelofs, R., Stern, M., Srebro, N., & Recht, B. (2017). The marginal value of adaptive gradient methods in machine learning. In Advances in Neural Information Processing Systems (pp. 4148-4158).

On ADAM with weight decay:
Ilya Loshchilov, Frank Hutter. Fixing Weight Decay Regularization in Adam. 2017. arXiv:1711.05101v2