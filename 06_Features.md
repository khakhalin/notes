# Feature engineering and selection

#features

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

## Variable ranking

