# A rubric for ml production readiness

#advice

Breck, E., Cai, S., Nielsen, E., Salib, M., & Sculley, D. (2017, December). The ml test score: A rubric for ml production readiness and technical debt reduction. In 2017 IEEE International Conference on Big Data (Big Data) (pp. 1123-1132). IEEE.

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