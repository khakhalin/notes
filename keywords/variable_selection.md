# Variable selection and construction

Parent: [[04_Features]]
See also:

#features

 
Basic checklist for variable selection:

1. Use domain knowledge
2. Normalize variables (where appropriate)
3. If variables aren't independent, construct **conjunctive features**, e.g. a simple average, a manually constructed score, or the 1st [[pca]] component.
4. If features need to be pruned for some external reasons (compute, memory, biasing) create **disjunctive** features. For continuous variables, one can use upper dimensions of a [[pca]]-style factor analysis, or [[clustering]]. For dummy variables, clustering them based on the values of other features may help (say, going from 7 days a week to weekdays and weekends, or from a list of individual categories to a few category types). In either case, the goal is to keep some information about which points differed, and which points were similar, even if the exact values of these features will be removed from the dataset.
6. Get a baseline by assessing each of the features independently
7. Detect and handle outliers and dirty data
8. Start with the simplest predictor (such as linear regression)
9. Benchmark different ideas
10. Check solution stability by bootstrapping (cross-validation)

# Refs

[[Guyon2003variable]] - review on feature selection (and a bit on construction)