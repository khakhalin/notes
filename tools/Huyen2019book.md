# Machine Learning Interviews
by Chip Huyen, self-published brochure
#book

https://github.com/chiphuyen/machine-learning-systems-design/blob/master/build/build1/consolidated.pdf

Research environment is focused on training and ensembles (fractions of % in performance), while industry is interested in reliable pipelines (serving).

That's why Kaggle doesn't represent real life for example: the data is cleaned and nicely annotated, and also multiple testing situation, so the best model would not necessarily translate to other (even if similar) tasks.

When describing a mock ML pipeline, consider:
* Goals (including several possible reformulations). Relative costs of false-positives and false-negatives. Social biases.
* Data sources (avaiability, collection). User experience. Privacy concerns.
* Constaints; storage, whether all available at once, fits into memory. Data structures.
* Data pre-processing and representation
* Evaluation

Baselines:
* Random
* Human
* Simplistic heuristic

Some comments on working with very big datasets (when even one case doesn't fit into  memory: Gradient Checkpoint), parallel computing (Asynchronous SGD). Some refs.

Nice list of references to well documented ML cases on the web, followed by a [list of sample interview questions](https://github.com/chiphuyen/machine-learning-systems-design/blob/master/content/exercises.md).