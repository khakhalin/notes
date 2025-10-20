# Boosting and AdaBoost

Parents: [[ensembles]]
Related: [[bagging]], [[gbm]], [[lightgbm]]

#ensemble


Boosting uses simplest decision trees possible:  for every coordinate we make a single split into 2 categories, aka **decision stumps**. But with every next series of trees, we increase the weights for those elements that weren't classified correctly by the previous generation of classifiers. (At the beginning of the process, for the very first tree, we start with equal weights). These weights may either be explicitly included in the error calculation, or be used as probabilities for each data point to appear in next training subset (mathematically, the results are the same). At the end, predictions are made by **weighted majority rule**: the average across all trees, weighted by the **accuracy of individual trees**, followed by argmax.

The archetypical approach: **AdaBoost**, aka **Adaptive Boosting**. For a case of binary classification:
1. **Start with** all points x_i having **identical weights** {W}. As $∑w_i$ = 1, we start with w_i = 1/n.
2. **For each coordinate, find the best split** (aka **stump**), with smallest total error $E = ∑w_i ϵ_i$, where $ϵ_i = 1\cdot(h(x_i)==y_i)$. Here $h(x)$ is the predictor function for this split.
3. Now **across all coordinates**, find a split with minimal [[gini]] index (one that achieves best separation of classes). For all loops after the 1st (once the weights are different), either use **weighted gini index**, or randomly resample points with draw probability P ∝ w (draw with repetition, keeping the total number of points at each split constant).
4. Calculate the **weight of this stump** as $α_k = ½ \log((1−E)/E)$ , where E is the error. This formula →+∞ for E→0 (perfect split), →-∞ for E→1 (perfectly erroneous split), and ~0 for splits that perform near chance level, when E=1-E, and so α≈log(1). In practice, to avoid ∞, a tiny ε is added to both numerator and denominator of the fraction under the log().
7. Reject trees with accuracy less than 50%. _Not all descriptions mention that._
9. **Update the final model**: produce an average of tree outputs, weighted by their α_k.
10. If all points are classified correctly, or if some pre-agreed threshold accuracy is reached, consider the task completed. This weighted average from the previous step is our model.
11. ⭐ But if we are not happy yet, **boost misclassified samples** by increasing their weights: $w_i ← w_i \exp(α_k)$; decrease weights of correctly classified samples by multiplying them by exp(−α); then normalize all weights again to $∑w = 1$. Then go to step 2 and generate a new split for our collection.

In practice, instead of changing weights for points, people often just resample the bag of points, gradually living their only points misclassified by previous models.

> Is this random resampling better in practice, compared to a weighted formula? Is it way more efficient computationally? Nobody says it openly, but if it weren't the case, why would people repeat this whole resampling story in each tutorial?

As each split is performed along a single variable (axis), every single split is || to all other variables. Because of that, the final weighted decision border looks like a combination of these planes (lines). But all of them are ⊥ or || to each other, just shifted; there are no curves or non-right angles there. So the final decision border looks kinda like a crystalline maze, brutalist architecture, or a crystal of bysmuth.

For multi-class classification, either create lots of binary classifiers (each class against all others), or encode each class as a superposition of several binary "features" that may be present or absent (say, bunnies are cute and jumpy, cats are cute but not jumpy; crickets are jumpy but not cute etc.), then use AdaBoost to identify the presence of each of these features separately ([ref](https://engineering.purdue.edu/kak/Tutorials/AdaBoost.pdf)).

In many ways, AdaBoost goes against the conventional wisdom for classification: it doesn't try to build a good classifier, but instead uses a lot of crappy ones. It does not try to identify important variables, or isolate important features using dimensionality reduction, but instead, it thrives in this high-D multi-variable space.

It is also possible to use boosting with custom loss functions: for example, one can put more weight in false-positives compared to false-negatives, or ther other way around.

## Bagging vs Boosting comparison

* Bagging tends to decrease variance, but not necessarily bias (biases of different models would just average). Boosting tends to reduce both variance and bias.
* Conversely, Bagging does not overfit (each weak learning may overfit, but these effects will cancel out), while Boosting can overfit very easily, by slicing space ever thinner around every point.
* Except for the very last step, bagging can happen in parallel, while boosting is by definition sequential, and so not easily parallelizable.
* Compared to many other ML methods, both bagging and boosting are very fast.

# Refs

* [Akash Desarda](https://towardsdatascience.com/understanding-adaboost-2f94f22d5bfe)
* https://en.wikipedia.org/wiki/AdaBoost
* [Tommi Jaakkola](http://people.csail.mit.edu/dsontag/courses/ml12/slides/lecture13.pdf) lecture note;
* [Explaining AdaBoost](http://rob.schapire.net/papers/explaining-adaboost.pdf) by Robert Schapire;
* [YouTube video by StatQuest](https://www.youtube.com/watch?v=LsK-xG1cLYA) (good)
* [Slides by Avinash Kak, Purdue](https://engineering.purdue.edu/kak/Tutorials/AdaBoost.pdf) (good).
