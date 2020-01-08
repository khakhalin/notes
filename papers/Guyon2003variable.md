# An introduction to variable and feature selection
Guyon, I., & Elisseeff, A. (2003). An introduction to variable and feature selection. Journal of machine learning research, 3(Mar), 1157-1182.

#review #features

14000 citations!

Typical for modern (as of 2003) solutions to work with >10^6 variables (genes, words), as opposed to old-school papers that were in the dozens. The problem: build useful features, and also avoid redundancy. 

Nice checklist (summarized in my main chapter)

## Variable ranking

Cites Fisher’s criterion: `var_between / var_within` , but it's unclear what their judgement is, in terms of using or not using it.

Simple: correlate each input var x_i with y; then sort (aka rank) by $r^2$. It may be OK to be generous here, and transform the variables, or use non-linear fits.

Claim that `cor(Y,x_i)` can be easily extended to nominal Y, citing this, but don't give the formula, citing (Furey 2000) instead.

Alternative: use a classifier (set a threshold for both x_i and y, and just see how well you can predict). For threshold for y, use median; for x_i, use the one that brings false-positive and false-negative rates to the same value: FPR=FNR. If many variables allow full separation, either rank by margin, or switch to correlation. _Not clear to me why anyone would use a classifier-based ranking of variables when they can do correlation and look at r squared. It's even computationally harder._

Fancy option: Information criteria. Bin both x_i and y, calculate a standard double integral of $p(x_i,y) \log( p(x_i,y) / (p(x_i)p(y)) )$. How to bin best? They cite "non-parametric Parzen windows" (Torkola 2003)

But the problem (visible even for simple examples of 2 variable) is that sometimes both variables are useless by their own, but allow perfect separation of classes together. So a more interesting topic is selecting a subset of variables!

## Subset selection

Three approaches: 
* **Wrappers**: define goal, try all possible combos, pick the best
* **Filters**: pre-process variables before training, standard and domain-specific tools
* **Embedders**: learn a subset during training

For **Wrappers**, need to 1. Define model performance (usually, validation set, or cross-validation), and 2. Define a strategy for samplinging variable subsets (unless you really do all possible subsets). Alternatively: **greedy algorithms**. Either **forward selection** (add features to the list), or **backward elimination** (exclude noisy features). Downside: not all data is used for training (as validation set is necessary). _Note: apparently, the use of greedy algorithms is hugely controversial, as they are always inferior to other approaches (see [[04_Regression]])._

**Embedders**: Nested subset methods: define the objective function as J(s) where s is the number of chosen variables. _Then they list some topics I don't understand without a context (and in the paper, they are given without a context):_
* Quadratic approximation of the cost function
* Kernel methods
* Optimum Brain Damage - an old paper by LeCun from 1990 (ref below) where they did something like pruning, but using an approximation for the objective function (is it because they couldn't calculate it directly)

**Direct objective optimization**: Take the goodness of fit, and add punishment for model complexity, then optimize it directly. Similar to shrinking (regularization).

Another weird method: use all variables for an SVM, then rescale all variables by multiplying them by the absolute values of the weight vector. Repeat until convergence. It will shrink weak variables to nothing (Weston 2003). 

But in most cases direct optimization introduces a hyperparameter (the cost of complexity).

**Filters**: faster, as happen entirely before training. May be linear or non-linear (?). Markov blankets (?) and other information-based criteria. _Overall, they seem to cite lots of contemporary papers from the same issue, but probably by now we have better methods, don't we?_

## Feature construction and dim reduction

**Clustering**: may be both unsupervised and supervised (based on examples). Claims that it's linked to **information theory**, as it's about finding a proxy ξ for existing x→y, such that mutual information I(ξ , x) is minimized, while I(ξ , y) is preserved. Can be optimized with a Lagrangian multiplier J = I(ξ,x) - β∙I(ξ,y). _Not sure I can intuit this at this point._

But this definition above apparently opens a window to use methods like **KL divergence** for information-based clustering.

Another method (_Isn't it way more popular now?_) is **matrix factorization**.

**Supervized clustering**. Reference some methods I never heard about (e.g. "Parzen windows"). _Skip for now; it feels both scary and oldschool, which makes it gangerous._

## Validation

Basic info on cross-validation.

Finishes with "Advanced Topics and Open Problems" that I mostly skipped.

## References

Furey, N. Cristianini, Duffy, Bednarski N., Schummer D., M., and D. Haussler. Support vector
machine classification and validation of cancer tissue samples using microarray expression data.
Bioinformatics, 16:906–914, 2000.

LeCun, Y., Denker, J. S., & Solla, S. A. (1990). Optimal brain damage. In Advances in neural information processing systems (pp. 598-605).
http://yann.lecun.com/exdb/publis/pdf/lecun-90b.pdf
3k citations

Torkkola. Feature extraction by non-parametric mutual information maximization. JMLR, 3:
1415–1438, 2003.

J. Weston, S. Mukherjee, O. Chapelle, M. Pontil, T. Poggio, and V. Vapnik. Feature selection for
SVMs. In NIPS 13, 2000.