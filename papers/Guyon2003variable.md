# An introduction to variable and feature selection

Guyon, I., & Elisseeff, A. (2003). An introduction to variable and feature selection. Journal of machine learning research, 3(Mar), 1157-1182.

#review #features #inprogress

14000 citations!

Typical for modern (as of 2003) solutions to work with >10^6 variables (genes, words), as opposed to old-school papers that were in the dozens. The problem: build useful features, and also avoid redundancy.

Nice checklist (summarized in my main chapter)

## Variable ranking

Cites Fisher’s criterion: `var_between / var_within` , but it's unclear what their judgement is, in terms of using or not using it.

Simple: correlate each input var `x_i` with `y`; then sort (rank) by `r^2`. It may be OK to be generous here, and transform the variables, or use non-linear fits.

Claim that `cor(Y,x_i)` can be easily extended to nominal Y, citing this, but don't give the formula, citing this instead:
Furey, N. Cristianini, Duffy, Bednarski N., Schummer D., M., and D. Haussler. Support vector
machine classification and validation of cancer tissue samples using microarray expression data.
Bioinformatics, 16:906–914, 2000.

Alternative: use a classifier (set a threshold for both `x_i` and `y`, and just see how well you can predict). For threshold for y, use median; for x_i, use the one that makes FPR=FNR. If many variables allow full separation, either rank by margin, or switch backc to correlation.

> Not clear from the text why anyone would use a classifier-based ranking when they can do correlation. Wouldn't it be inherently worse? And if you need to find FPR=FNR point (estimate the ROC),  then it's not even computationally faster, no?

Fancy option: Information criteria. Bin both `x_i` and `y`, calculate a standard double integral of `p(x,y)log[p(x,y)/(p(x)p(y))]`. How to bin best? Cite "non-parametric Parzen windows":
Torkkola. Feature extraction by non-parametric mutual information maximization. JMLR, 3:
1415–1438, 2003.