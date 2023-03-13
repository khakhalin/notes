# Boolean Sensitivity

#logic #complexity

A measure of boolean complexity.

For a boolean function of many inputs, **Sensitivity** is a measure of how many inputs needs to be flipped for the function to change its output value. Defined as the **largest number of sensitive bits** achieved, acros all possible inputs to the function. So if there exists an input (something like 010110111) that produces 0, but there are **k different bits** that, if flipped, turns the output to 1, then the sensitivity is k.

> If I got it right, it means that for a function $(-1)^{\sum x}$  the sensitivity is n (every bit always flips the output);  for a function `mode(x)` the sensitivity is about n/2 (there exists one "middle situation" where about half of the bits switch the output, while for a function `f(x) = x_1` the sensitivity is 1 (whatever you do, all bits but one are irrelevant, so even at the most sensitive, there's only one bit that can flip it).

There was some **sensitivity conjecture** about how boolean sensitivity relates to other measures of boolean complexity that stayed unproven for a long time.

# Refs

https://www.wired.com/story/a-decades-old-computer-science-puzzle-was-solved-in-two-pages/