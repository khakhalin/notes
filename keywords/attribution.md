# Attribution and Shapley (SHAP) Values

Parents: [[interpretability]]
See also: [[LightGBM]], [[glm]]

#interpretability


Subtopics:
* [[shap]] - shap values

There are several ways to talk about attribution:
* In science, attribution is mostly about explaining the variance, so we can directly go to [[glm]] and ANOVA.
* For predictive models, attribution is about looking what variables have more predictive power (as in tree-based models for example [[05_Ensembles]]). The best approach - [[shap]] values
* In business, we typically see that a certain KPI changed, and we want to understand what were the driving factors. Exactly the same, for portfolio performance (lots of refs on this topic). Here we can do either sequential model reconciliation (equivalent to glm with interactions), or Shapley values.

# Mix Reconciliation

Let's add one explanatory parameter at a time, and see how much of a change it offers (how much of a difference it explains). The sequence matters in this case, and I think it's good to start with confounding factors first (to prepare the field), then important factors (big changes), then smaller and weirder factors. After all factors are incorporated, we'll also have the "other", or "unexplained" part.

If we assume that the KPI stays the same at the most narrow cell of the final model (an intersection of all factors), and only the mix of events changes, then a reconciliation model is equivalent to a sequential [[glm]] with interactions. But instead of building a model and calculating predicted values, one can also build sequential `groupby` in [[pandas]], and build a series of approximations that way. It may be a bit more straightforward. Any changes of within-smallest-cell would be left in "Unexplained".

# Refs

