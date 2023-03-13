# Variable selection and construction

#features

Parent: [[04_Features]]
See also:
 
Basic checklist for variable selection:

1. Use domain knowledge
2. Normalize variables (where appropriate)
3. If variables aren't independent, construct **conjunctive features**
4. If need to prune for external reasons (compute?) create **disjunctive**, or weighted
5. Get baseline by assessing features independently
6. Detect and handle outliers and dirty data
7. Start with the simplest predictor (usually linear)
8. If you have better ideas, implement, then compare (benchmark)
9. Check stability by bootstrapping (cross-validation)

# Refs

[[Guyon2003variable]] - review on feature selection (and a bit on construction)