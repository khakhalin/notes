# Model validation

#classification

Parent: [[03_Classification]]
Related: [[confusion]], [[roc]]

Canonical system of names for **Validation and Testing**:

* **Training dataset** - obviously
* **Validation dataset** - used to calculate validation error; monitored alongside training dataset. The difference between error on validation and training sets is used for model selection, regularization, hyperparameters tuning.
* **Testing dataset** - ideally, whould never be seen by the model until the very end. But at the same time, as models are eventually evaluated based on their performance on the testing dataset, arguably, it indirectly participates in hyperparameter tuning and model selection. Still, try to minimize that.

**Cross-vallidation**: Instead of setting apart one validation dataset, generate lots of those by random splits. Recommendations for split are around 70/30 or 80/20, although extreme versions of 50/50, or "**leave one out**" also exist. Note that repeating random split 10 times is different from **10-fold cross-vallidation**, as there will be overlaps between random splits, while "repeated holdout" carefully cycles through hiding from the model each of the possible equally-sized validation blocks.

What's the current best practice? Some opinions:
* k=3 for big (slow, expensive) models, leave-one-out for small ones - [ref](https://medium.com/@george.drakos62/cross-validation-70289113a072?)
* k=5 or 10 just because feels like reasonable numbers - [ref](https://machinelearningmastery.com/k-fold-cross-validation/)

For unbalanced datasets and multi-class classification, it is proper and honest to used **stratified cross-validation**, when each of the strata is split separately, and then mixed back, to avoid class imbalance.