# Machine Learning Interviews, Huyen 2019

by Chip Huyen, self-published brochure. 2019.

#book #lifehack #interview

See also: [[job_search]], [[ml_lore]]

https://github.com/chiphuyen/machine-learning-systems-design/blob/master/build/build1/consolidated.pdf

Research environment is focused on training and ensembles (fractions of % in performance), while industry is interested in reliable pipelines (serving).

That's why Kaggle doesn't represent real life for example: the data is cleaned and nicely annotated, and also multiple testing situation, so the best model would not necessarily translate to other (even if similar) tasks.

When describing a mock ML pipeline, consider:
* **Goals** (including several possible reformulations). What are we trying to achieve? User experience. Relative costs of false-positives and false-negatives. Social biases. Desired amount of personalization.
* **Evaluation**: in the sprit of "test-driven design", we can as well put it upfront.
* **Data sources**: how to collect it
* **Constraints**: 
    * storage
    * whether all data is available at once, fits into memory. 
    * user data and privacy
    * necessary data structures
* **Data pre-processing** and representation
* After describing several solutions, and possibly converging on one likely winner, carefully write down the **assumptions** made. Brainstorm if any of them may be violated. Bonus points if we know how to notice it.
* Use [[ml_lore]] advice to show how the solution can be developed gradually, with incremental increases in complexity.

Baselines:
* Random
* Human
* Intentionally simple heuristic
* If working on a deep network - some classic ML algorithms

Some comments on working with **very big datasets** (when even one case doesn't fit into  memory: Gradient Checkpoint), parallel computing (Asynchronous SGD). (_Some refs provided_)

Clickable case studies here: https://github.com/chiphuyen/machine-learning-systems-design/blob/master/content/case-studies.md

Nice list of references to well documented ML cases on the web, followed by a [list of sample interview questions](https://github.com/chiphuyen/machine-learning-systems-design/blob/master/content/exercises.md) (or rather, prompts for creative ML scenario exploration).