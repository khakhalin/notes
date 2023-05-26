# Variable selection and construction

Parent: [[04_Features]]
See also:

#features

 
Basic checklist for variable selection:

1. Use domain knowledge
2. Normalize variables (where appropriate)
3. If variables aren't independent, construct **conjunctive features** (say, averages or [[pca]])
4. If need to prune the number of features for some external reasons (compute, memory, biasing) create **disjunctive** features (using upper dimensions of a [[pca]]-style factor analysis, or maybe [[clustering]], to stress which points differed on these features, even if the exact values of these features had to be lost)
5. Get a baseline by assessing features independently
6. Detect and handle outliers and dirty data
7. Start with the simplest predictor (usually linear)
8. If you have better ideas, implement, then compare (benchmark)
9. Check stability by bootstrapping (cross-validation)

# Refs

[[Guyon2003variable]] - review on feature selection (and a bit on construction)