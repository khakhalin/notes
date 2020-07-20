# Loss functions

#loss #dl

Parents: [[06_DL]]

Subtopics
* [[perceptual_loss]]
* Hinge loss - used in [[svm]]
 
# MSE, ara L2, aka Eucledian

Obvious choice for continuous output: **Eucleadian distance** (aka **Mean Squared Error**, or **MSE**). 

See [[02_Regression]]

# Cross-entropy

For classification: **Cross-Entropy Loss**. The ground-truth is one-hot encoded vector; the prediction is a vector of probabilities. Definition: H(p,q) = -∑ p_i ∙ lg(q_i) where p_i are probabilities of different classes in the training set, and q_i are probabilities predicted by the model.

See: [[03_Classification]]

Motivation: essentially, average log-likelihood. Assume that the predictions of the model follow a true correct distribution, with q_i encoding P(x_i). Then what it the probability of observing n_i cases of x_i? Assuming that test cases are independent, P = ∏P(each observation) = ∏q_i ^ n_i . If we lg it (to replace products with sums), we get the total log-likelihood: L = ∑ n_i ∙ lg(q_i) . If you average it by dividing by the total number of test cases, and denote p_i = n_i / N, we get loss = ∑ p_i ∙ lg(q_i) =-H(p,q) . And then you are trying to minimize this value by making the model make predictions q_i such as the observed values (from P) are "minimally strange". Feels Bayesian, huh?

# Huber Loss

**Huber loss**: a compromise between MSE loss that is tolerant to small noise (behaves nicely around 0, as x² ≪ x for small x), but is super-sensitive to outliers, and linear loss that is the other way around. Essentially, just a parabola with arms that smoothly transition to linear at some point, and continue like that. Formula: L = 1/2∙x² for x<d, but (abs(x)-d/2)∙d for x>d. ([wiki ref](https://en.wikipedia.org/wiki/Huber_loss))

# Refs

* [by Daniel Godoy](https://towardsdatascience.com/understanding-binary-cross-entropy-log-loss-a-visual-explanation-a3ac6025181a) - a visual explanation of Binary Cross-Entropy. Good intuition for why -log(1-prediction) makes sense: if you were very certain, and got it wrong, that's a more important learning point compared to a case when you were pretty lukewarm about it to begin with.
* [Wiki for cross-entropy](https://en.wikipedia.org/wiki/Cross_entropy)
* [A list of losses supported by Keras](https://keras.io/losses/)