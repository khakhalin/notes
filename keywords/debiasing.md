# De-biasing

Parents: [[text]], [[embedding]], [[fairness]]
Related: [[word2vec]], [[gender]]

#text #embedding #fairness


**Core idea**: if you have a good semantic embedding, sensitive categories (such as gender or race) should be represented in your representation as one of the dimensions. So arguably you can now identify this dimension, and cancel it out (at least from the majority of your terms), by projecting unto an orthogonal dimension.

But in high-D it doesn't quite work (it's not enough), as different dimensions in high-D are not necessarily orthogonal (_but rather independent? How does it work? does a typical ANN embedding approximate a [[pca]] or an [[ica]]?_) So if there's an idk "aggressiveness" axis, it may be correlated with pure "gender" bias, but when "gender" axis is projected away, the "aggressiveness" axis will remain, and it will still be different across some "male" and "female" words. So all of these "partially correlated" axes together will be enough to build a robust classifier of "gender", even after the "gender" axis is removed.

And an interesting thought here: IRL, gender expression isn't really a line. It's not like there's one way to express gender, which means that even from this pov we can't really expect gender to be represented as a single dimension in a word embedding space.

# Refs

Nice lecture from Rasa Algorithm (from a middle of a course) about de-biasing:
https://www.youtube.com/watch?v=2ROP1QFKsqc&list=PL75e0qA87dlG-za8eLI6t0_Pbxafk-cxb