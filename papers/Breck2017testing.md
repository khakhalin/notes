# A rubric for ml production readiness

Breck, E., Cai, S., Nielsen, E., Salib, M., & Sculley, D. (2017, December). The ml test score: A rubric for ml production readiness and technical debt reduction. In 2017 IEEE International Conference on Big Data (Big Data) (pp. 1123-1132). IEEE.

https://storage.googleapis.com/pub-tools-public-publication-data/pdf/aad9f93b86b7addfea4c419b9100c6cdd26cacea.pdf

#management

Tests for features and data:
1. Schema: encode tests for some obvious facts or asymptotics, end test them explicitly (and automatically). Visuals are helpful to identify and produce the schema.
2. Unhelpful features are weeded out
3. Features that are hard to compute but that add little are eliminated
4. If external requirements (e.g. legal) make certain features inacceptable, enforce that programmatically
5. Data pipeline has automated privacy control. Tight access right. Propagate data deletion.
6. Build in potential to add new features
7. Test 	new features before including them in data generation process

Tests for model development:
1. Version control + code review for all changes
2. Understand the relationship between offline (e.g. loss) and online (actual target) metrics
3. Tune all hyperparameters
4. Know the impact of model staleness. How often do you need to update?
5. Regularly test against extremely simplified models, to make sure you see the difference
6. Model quality is sufficient on all important data slices (not just on average)
7. Model was tested for social considerations, like inclusion

Test for ML infrastructure:
1. Training is reproducible (deterministic training is best. Seed random generators?)
2. Unit test model: e.g. configuration files, single step of gradient descent, restore from checkpoint. Training to overfit may be included here.
3. Test pipeline integration
4. Automated quality test that terminates a model if it's obviously off (compared to the prev one?)
5. There's a debug mode (that allows to run this model on a single example and reports all stages)
6. "Use a canary". What they mean is: make a gradual roll-out, don't remove old model immediately, compare as you go
7. Rolling back is easy and safe

Monitoring tests:
1. Monitor dependencies, updates
2. All data is tested via schema (above), alert if weird. Tune false-positives to make it meaningful
3. Monitor whether live behavior matches testing behavior (apparently in complex situations they may diverge, and it's like problematic)
4. Staleness
5. Alerts for NaNs, zero weights and the share of ReLUs returning zeros
6. Monitor compute times, and alert if they increase (technical reasons)
7. Make sure performance doesn't drop.
    * No bias, and no change in bias: 0.5 probability should be true in 50% of cases. (Definition of an unbiased model: average of predictions = average of observations. [ref](https://developers.google.com/machine-learning/crash-course/classification/prediction-bias))
    * If possible, measure or estimate actual performance
    * If not, manually generate new testing data every now and then

Other pieces advice:
Checklistgs are good. Use them.