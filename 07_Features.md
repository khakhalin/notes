# Feature engineering
#features

Simple feature transformations:
* **Binning** in high D: the simplest thing ever; emulates cluster analysis without actually running cluster analysis, as clusters are more likely to be covered by the same bin. May be used to transform a continuous variable into a pseudo-nominal one.
* **One-hot encoding**: the other way around, encoding nominal stuff as numbers.
* **Cross-products**: if you have a high-D situation, adding cross-products of basic features is a cheap and fust way of performing non-linear modeling with linear methods.
* **Kernel tricks**: use various functions (kernels) to calculate new synthetic features from old features. For example, distance from an well chosen point in the many-D space may be a great kernel.


## Variable selection

[[Guyon2003variable]] - review on feature selection (as well as some info on construction). 
Their checklist:
1. Use domain knowledge
2. Normalize variables where appropriate
3. If variables aren't independent, construct conjunctive features
4. If need to prune for external reasons (compute?) create disjunctive, or weighted
5. Get baseline by assessing and ranking features independently
6. Detect and handle outliers and dirty data
7. Start with a simplest predictor (usually linear)
8. If you have better ideas, implement them, and compare (benchmark) them
9. Check stability by bootstrapping (do they mean cross-validation?)