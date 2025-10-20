# Loss functions

#loss #dl

Parents: [[regression]]
See also: [[regularization]]

Subtopics
* [[l2]] - the main one ever (aka MRSE)
* [[logreg]] - log-loss, which is in fact [[cross-entropy]]
* [[01loss]] - indicator loss
* [[perceptual_loss]]
* Hinge loss - used in [[svm]]
* [[mape]] - beloved in business

For [[time-series]], we can use "standard" loss (probably [[l2]]), but we have to modify the way it is interpreted (validated) using [[rolling_cross_validation]]
 
# Huber Loss

**Huber loss**: a compromise between MSE loss that is tolerant to small noise (behaves nicely around 0, as x² ≪ x for small x), but is super-sensitive to outliers, and linear loss that is the other way around. Essentially, just a parabola with two linear arms smoothly grafted to it, to keep growth for large x linear. Smooth in terms of f(x) and f'(x).

Formula: L = 1/2∙x² for x<d, but (abs(x)-d/2)∙d for x>d. 

$\displaystyle L = \begin{cases} \frac{1}{2} x^2 & \text{for} & x<d \\ d(|x|-d/2) & \text{for} & x>d \end{cases}$

Ref: https://en.wikipedia.org/wiki/Huber_loss

# Refs

* [by Daniel Godoy](https://towardsdatascience.com/understanding-binary-cross-entropy-log-loss-a-visual-explanation-a3ac6025181a) - a visual explanation of Binary Cross-Entropy. Good intuition for why -log(1-prediction) makes sense: if you were very certain, and got it wrong, that's a more important learning point compared to a case when you were pretty lukewarm about it to begin with.
* [Wiki for cross-entropy](https://en.wikipedia.org/wiki/Cross_entropy)
* [A list of losses supported by Keras](https://keras.io/losses/)