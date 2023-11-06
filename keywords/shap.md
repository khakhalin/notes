# SHAP (and Shapley) values

Parents: [[interpretability]] / [[attribution]]
See also: [[LightGBM]]

#interpretability


SHAP values are the best (mathematically optimal) way to attribute effects of model inputs on the model output. Apparently SHAP and Shapley values are two slightly different things, and people mostly use SHAP (they are slightly better), but often refer to them as Shapley.

The values satisfy the following requirements:
1. Intuitively, they characterize the effect an addition of each variable has on the output of the model. If the SHAP value is positive, then large positive values of this variable are associated with larger outputs, and vice versa. In this respect they are like marginal contributions of the variable, or a univariate model based on this variable, but better.
2. The way they are "better" is that they take for account the entire model (unlike a univariable model), are not undercut by codependency (as in the case of marginal effects, when the value is the last one to be included), but also don't make any assumptions about the "meaningful" sequence in which variables should be plugged in (as in the case of sequential sum of squares). They achieve this by considering **all possible subsets of variables**, and calculating marginal contributions in regard to this subset. Then weighted-averaging them. Why weighted-averaging, and not simply averaging? To achieve some more nice qualities:
4. If the variable is absent (constant), its contribution should be 0
5. The sum of all contributions should be equal to the actual model output (or rather, the difference between the output and the global average, which is considered a starting point)

# Refs

The most awesome, clear and complete, explanation from Christoph Molnar:
https://christophm.github.io/interpretable-ml-book/shap.html

https://arxiv.org/pdf/2102.05799.pdf
Moehle, N., Boyd, S., & Ang, A. (2021). Portfolio performance attribution via Shapley value. arXiv preprint arXiv:2102.05799.