# Classification

## Basic classification problem
Simplest approach: **Nearest Neighbor**. Just pick closest training case. This is an example of a **lazy learning** approach: instead of generating estimations upfront (that would be called **eager**), we only generate them at retrieval. A slightly better approach: **K nearest neighbors** (aka KNN).

Classic example: [MNIST](http://yann.lecun.com/exdb/mnist/index.html) (link to a library of all thinkable and unthinkable approaches to classification, as applied to this dataset, each given with a test error rate)

## SVM

Widest street approach: find a line, so that if you have a band around the line, it separates the positive from the negative examples as best as possible.

How to use the line? Describe it with a vector ω ⊥ line. Now how far a point $u$ is in the line can be described as uω. Let's say that $b$ is a good projection value ($u$ lies right smack on the central line), so we have $u\cdot\omega + b \geq 0$.

To find the best line, let's insist that for samples we don't just get above 0, but get $x\omega+b$ above 1 for positive samples, and -1 of negative samples. 