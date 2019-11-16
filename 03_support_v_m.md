# Support vector machines

Decision boundaries. Older (simpler) approaches: Nearest Neighbor, Decision Trees.

Widest street approach: find a line, so that if you have a band around the line, it separates the positive from the negative examples as best as possible.

How to use the line? Describe it with a vector $\omega \perp$ line. Now how far a point $u$ is in the line can be described as $u\omega$. Let's say that $b$ is a good projection value ($u$ lies right smack on the central line), so we have $u\cdot\omega + b \geq 0$.

To find the best line, let's insist that for samples we don't just get above 0, but get $x\omega+b$ above 1 for positive samples, and -1 of negative samples. 