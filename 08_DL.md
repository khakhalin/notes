# Deep learning

## Backpropagation
Neat video: [Backprop calculus by 3blue1brown](https://www.youtube.com/watch?v=tIeHLnjs5U8&list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi&index=4)

Essentially a chain diff rule, for cost function C by weights w.
Consider a network of several layers, eventually all convering on one ouptut element.
For a squared loss, C=(a-y)^2, and where a = out = h(z) = h(sum_i w10_i a1_i).
(Output layer is kinda layer 0, receiving inputs from first layer, counting backwards.)
dC/dw10_i (actually partial derivative of course) is (and I can drop i here):
dC/dw10 = dC/da da/dz dz/dw10 = 2(a-y) h'(z) a1,
where a1 is the  activation a1_i arriving from the layer 1, that gets multiplied by w10_i.
So like Hebbian rule: if (a-y) is large, and a1_i is large, then w10_i matters.
(Similar formula to bias, as technically it's h(wa+b): so we just get 1 instead of a_1 in the formula.)

Now we can go deeper, to the yet prev layer 2->1, with weights w21_j.
No need to sum yet, as w21_j only affects one element in the 1st layer: a1_i:
Again, we have:
dC/dw21_j = C/da1_i da1_i/dw21_j.
dC/da1_i can be calculated as above = 2(a-y) h'(z) w10_i,
and da1_i/dw2_j = da1_i/dz1_i dz1_i/dw2_j = h'(z1_i) a2_j (activation a2_j from yet prev layer).
So dC/dw21_j = 2(a-y) h'(z) w10_i h'(z1_i) a2_j.

Now consider 3->2. Again chain rule, only now w32_k coming from a3_k affects one a2_j, which in turn affects all of the a1_i. So index i is no longer fixed, but we have to sum through it.
dC/dw32_k = ∑_i dC/da1_i da1_i/dw32_k = 
2(a-y) h'(z) ∑_i w10_i da1_i/dw32_k = 
2(a-y) h'(z) ∑_i w10_i h'(z1_i) w21_j h'(z2_j) a3_k.

And so on. What's important is that we essentially took the end error gradient 2(a-y), scaled it by h'(z), and then multiplied by same vector of  w10 that were used on the forward pass, to get "error-effects" at the previous layer 1. Then same way we took the vector of "error-effects", scaled each element by h'(z1) multiplied it by matrix w21, and got "error-effects" at layer 2. Backprop!

And the problems are immediately felt: if a certain w_ji=0, it kills the effect of all weights converging on element j. Next, if the value of z_j is such that it drives h'(z_j) to zero (say, for all sigmoids, it will happen for very high or very small z), it will also kill the gradient (vanishing gradient). Conversely, for a deep network, gradients can grow arbitrarily large (explosion). I'm assuming it would cauze weights deep in the network jump like crazy instead of converging.

## Convolutional networks

## Batch normalization

Ioffe, S., & Szegedy, C. (2015). Batch normalization: Accelerating deep network training by reducing internal covariate shift. arXiv preprint arXiv:1502.03167.
[https://arxiv.org/abs/1502.03167](<https://arxiv.org/abs/1502.03167>)
Main paper on batch norm (with like 30k references)

## Types of units

https://en.wikipedia.org/wiki/Gated_recurrent_unit