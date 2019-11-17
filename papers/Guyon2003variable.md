# An introduction to variable and feature selection

Guyon, I., & Elisseeff, A. (2003). An introduction to variable and feature selection. Journal of machine learning research, 3(Mar), 1157-1182.

#review #features

14000 citations!

Typical for modern (as of 2003) solutions to work with >10^6 variables (genes, words), as opposed to old-school papers that were in the dozens. The problem: build useful features, and also avoid redundancy.

Their checklist:
1. Use domain knowledge
2. Normalize where appropriate
3. If not independent, construct conjunctive features
4. If need to prune for external reasons (compute?) create disjunctive, or weighted
5. Get baseline by assessing and ranking features independently
6. Detect and handle outliers and dirty data
7. Start with a simplest predictor (usually linear)
8. If you have better ideas, implement them, and compare (benchmark) them
9. Check stability by bootstrapping (do they mean cross-validation?)