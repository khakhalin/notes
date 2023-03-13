# On the Bias-Variance Tradeoff: Textbooks Need an Update

#blog #variance

https://www.bradyneal.com/bias-variance-tradeoff-textbooks-update

See: [[bias-variance]]

A simple-language explanation to accompany 2 papers from 2019/2020 (to be read :)

> Upon revisiting, I'm not sure what this note means. Is it even worth it? Re-check the blog, after reading "double-descent" papers?

Classic U-shaped (or rather √-shaped) error(complexity) plot works for many methods, such KNN and kernel regression with varied kernel size. However not all measures of "modell complexity" lead to it: for DL, an increase in network width drives error down monotonously. 

Claim that (Geman 1992) is self-contradictory (words don't match graphs). Another possible effect: for boosting, variance grows exponentially slower than bias decreases, leading to net monotonous decrease in error rate (has refs). 

To explain this all, one needs to split variance into "variance due to sampling" and "variance due to optimization". It is the second term that defines whether a particular method will produce variance growing higher enough to result in a √-shaped plot. 

Some comments on "double descent", with refs that parallel this work.