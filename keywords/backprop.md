# Backpropagation

Parents: [[06_DL]]
Related: [[credit]] (credit assignment, including in the brain), [[bptt]] (backprop for RNNs)

#dl


Essentially a chain differentiation rule, to differentiate loss function J by weights w.

Consider a network of several layers (full = dense = all to all), eventually all converging on one output element (just because it's easier to describe it for one output element, but the math is essentially the same if you have many). MSE loss: J=(a0-y)², where a0 = out = h(z) = $h(∑ w^{10}_i a^1_i)$ = dot product of of prev (1st) layer activations with weights from layer 1 to layer 0. (I'll be numbering layers backwards, starting from 0 for the output layer)

$a^0 = h (\sum w^{10}_ i a^1_i )$

To change one of the weights w10_i, we need to know ∂J/∂w10_i .
For this one weight we have: ∂J/∂w10_i = ∂J/∂a0 ∂a0/∂z ∂z/∂w10_i = 2(a-y) h'(z) a1_i,
because ∂J/∂a0 = 2(a0-y),
∂a0/∂z = ∂h(z)/∂z = h'(z), and
∂z/∂w10_i = a1_i. That is, activation a1_i arriving from the layer 1, that gets multiplied by w10_i.

$\displaystyle \frac{∂J}{∂w^{10}_ {i}} = \frac{∂J}{∂a^0} \frac{∂a^0}{∂z^0} \frac{∂z^0}{∂w^{10}_ i} = 2(a^0-y) h'(z^0) a^1_i = ε^0 a^1_i$

(where ε is just a new wrapper variable for 2(a-y)h'(z); something like scaled error)

The formula above is actually vaguely reminiscent of Hebbian plasticity: if the output needs to be changed (the (a0-y) is large), and the activity of some input unit a1_i is large (it deserves of listening to), and listening to this  input can actually change something (h'(z) is reasonably large), then this weight (w10_i) matters, and needs to be changed. Except that in practice, if we want to reduce J, the weight it needs to be reduced, so we go against the gradient. And that's where it stops following Hebb's idea, as we're comparing unit inputs not with its outputs, but with a new type of signal, named error, that propagates backwards through the network.

We get a similar formula for bias, as for all layers except the lsat one it's always h(w∙a+b) and not just h(w∙a), except that we just get 1 instead of a1 in the formula, as ∂z/∂b = 1. So if we had bias at this 0th layer, we'd have ∂J/∂b = ε0.

Now we can go deeper, to the yet-previous layer 2→1, with weights w21_j.
No need to sum yet, as w21_j only affects one element in the 1st layer: a1_i:
Again, we have:
$\displaystyle \frac{∂J}{∂w^{21}_j} = \frac{∂J}{∂a^1_i} \frac{∂a^1_i}{∂w^{21}_j}$.
$∂J/∂a^1_i$ can be calculated as above $= 2(a^0-y) h'(z^0) w^{10}_i$,
while $∂a^1_i/∂w^{21}_j = ∂a^1_i/∂z^1_i \cdot ∂z^1_i/∂w^{21}_j = h'(z^1_i) a^2_j$ (activation a2_j from yet prev layer).
So the full expression:

$\displaystyle \frac{∂J}{∂w^{21}_ j} = \frac{∂J}{∂a^1_i} \frac{∂a^1_i}{∂w^{21}_ j} = 2(a^0-y)h'(z^0)w^{10}_ i h'(z^1_ i) a^2_ j = ε^0 w^{10}_ i h'(z^1_ i) a^2_ j = ε^1_i a^2_j$

where $w^{21}_ j$ should actually read $w^{21}_ {j→i}$ , or $w^{21}_ {ij}$, as it is projecting from element j in layer 2 to element i in layer 1.

And similar to what we had before, $\displaystyle \frac{∂J}{∂b^1_i} = ε^1_i$.

This part about $ε^1_i = ε^0 w^{10}_ i h'(z^1_ i)$ is why this process is called **backpropagation**: the error at layer 2→1 is obtained from the error 1→0 via weights $w^{10}$. The error is propagating back through weight matrices.

Now consider layer 3→2. Again chain rule, except now while link w32_k, coming from a neuron a3_k in the 3d layer, affects only one neuron a2_j in the 2nd layer, at the next step the activity of this neuron a2_j affects **all neurons** a1_i in the first layer. And then each of them gathers on our single output. So index i for 1→0 is no longer fixed, but we have to run a sum for it:

∂J/∂w32_k = ∑_i ∂J/∂a1_i ∂a1_i/∂w32_k = 
2(a0-y) h'(z0) ∑_i w10_i ∂a1_i/∂w32_k = 
2(a0-y) h'(z0) ∑_i w10_i h'(z1_i) w21_j ∂a2_j/∂w32_k = 
2(a0-y) h'(z0) ∑_i w10_i h'(z1_i) w21_j h'(z2_j) a3_k.

$\displaystyle \frac{∂J}{∂w^{32}_ k} = \sum_i \frac{∂J}{∂a^1_i} \frac{∂a^1_i}{∂w^{32}_ k} = 2(a^0-y) h'(z^0) \sum_i w^{10}_ i \frac{∂a^1_i}{∂w^{32}_ k} =$

$\displaystyle =2(a^0-y) h'(z^0) \sum_i w^{10}_ i \frac{∂a^1_i}{∂z^1_i} \frac{∂z^1_i}{∂a^2_j} \frac{∂a^2_j}{∂w^{32}_ k}$,  where j is where $w^{32}_ k$ is projecting

$\displaystyle =2(a^0-y) h'(z^0) \left( \sum_i w^{10}_ i h'(z^1_i) \right) w^{21}_ j \frac{∂a^2_j}{∂w^{32}_ k} =$

$\displaystyle =ε^0 \left( \sum_i w^{10}_ i h'(z^1_i) \right) w^{21}_ j h'(z^2_j) a^3_k =  \sum_i ε^1_i w^{21}_ j h'(z^2_j) a^3_k = ε^2_j a^3_k$

And so on; we can now do it for all layers. What's important is that we essentially take each error, scale it by derivatives of each activation function at each activation level h'(z); then **multiply by the transposed matrix of weights**, and get error-effects of the previous layer. Backprop!

**A summary in pseudocode:**
```
# Forward:
x = input
for L in layers:
    L.x = x
    L.z = w∙x + b   # Note: Each layer should remember its x and z
    x = h(L.z)      # New x to be used at next iteraction
    
# Backwards:
e = -∇Loss
for L in reversed(layers):
    L.ε = h'(L.z) ⨀ e  # Scale the error. That's where remembered z is used.
    e  = wᵀ∙L.ε         # Remember ε, then propagate errors backwards    
    
for L in layers:        # Now update values:
    b += L.ε*α          # α is the learning speed; ε is ε_L for this layer
    w += (L.ε∙L.xᵀ)*α   # Hebb-like outer product
```

Above, `∙` stands for dot-product, `⨀` for element-wise Hadamard product, h' for dh/dz, and α for learning rate. The problem with ε is that, depending on your notation, it may weights from next layer, but h'(z) from this layer, which puts it off-kilter with layer-by-layer loop. Most tutorials call this thing δ instead of ε. I also have 2 loops for backprop, which is of course not needed if we can get access to this layer's error (for update) and previous layer's error (for further backprop) at the same time. You just cannot do it without remembering the errors, completely "in-memory". Unless you first update w then backprop, which is incorrect.

Some potential problems can be immediately deduced from this story. If a certain w_ji=0, it kills the effect of all weights converging on element j. Same if the value of z_j is such that it drives h'(z_j) to zero, which also kills the gradient (aka **vanishing gradients**). Say, for sigmoids it happens for very high or very small z; for ReLUs it happens for any z<0, and they can't recover (aka **Dead ReLUs**). And the other way around, in a deep network, gradients can grow arbitrarily large (**exploding gradients**) as you keep multiplying errors by wᵀ, allowing errors deep in the network to grow.

# Refs

* [Backprop calc by 3blue1brown](https://www.youtube.com/watch?v=tIeHLnjs5U8&list=PLZHQObOWTQDNU6R1_67000Dx_ZCJB-3pi&index=4)
* [Description by Michael Nielsen](http://neuralnetworksanddeeplearning.com/chap2.html), with slightly unusual notation
* [Flexible backprop from scratch](https://blog.zhaytam.com/2018/08/15/implement-neural-network-backpropagation/) - a confirmation for my pseudocode