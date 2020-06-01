# Wave function collapse

#generative #graphics

General idea (by Maxim Gumin, Moscow): have a sample as an input; use it to calculate probabilities of different patterns. For the output, init with all patterns being equally probable at each point. Go through a loop:

1. Pick a pixel (or a region?) with lowest Shannon entropy, and collapse it: the state that was most probable will now become "certain".
2. Update probabilities (coefficients) for  states at all neighboring pixels, based on the template.

Repeat until converged. If some pixel's probabilities all become 0, the process has run into a contradition. It happens rarely, but seems to be a characteristic feature of this greedy approach.

How does the probabilities update happen? Karth 2017 only describe matching for consistency (setting to 0 probabilities that are now in contradiction, and recalibrating remaining ones). Solub's Python implementation is similar. So it seems that the classic WFC doesn't use Bayesian updates or even linear superpositoin of probabilities, but just looks at what patterns are **compatible** with all pixels in the surrounding area of each vacant pixel, and then resolves a pixel with the lowest numbers of possibilities (as lowest entropy in this case is just the lowest number of open possibilities).

Alternatively to just looking at each pixel and its immediate neighbors, one can use a **sliding window**, and use several pixels inside it as a pattern, and the border around the inside as the context that needs to be matched. This is more complex though, as we're getting more "valencies" (constraints) to satisfy (one for every ovelapping pixel in the border, and then, for corner pieces, also an overlap with windows located diagonally).

A 3D version is just a change in adjacencies. Can be expented to non-rectangular and irregular grids too (just add some logic for how to define valencies).

Strengths of this method:
* Doesn't care about the content of pixels; can encode whole tiles as numbers, and it will still work

Weaknesses:
* Cannot "blend" tiles
* All "colors" (pixel types) are equially distant

# References

Main reference: https://github.com/mxgmn/WaveFunctionCollapse

Maxim Gumin references two dissertations that informed his work:
* http://graphics.stanford.edu/~pmerrell/thesis.pdf (by Paul Merrell)
* http://logarithmic.net/pfh-files/thesis/dissertation.pdf (by  Paul F. Harrison)

Maxim Gumin's twitter: @ExUtumno

Nice Twitter-thread discussing the method, by Elie Michel:
https://twitter.com/exppad/status/1267045322116734977

Implementation in Python, by user solub (?):
https://discourse.processing.org/t/wave-collapse-function-algorithm-in-processing/12983

Demo by Oskar Stålberg:
* http://oskarstalberg.com/game/wave/wave.html

A follow-up paper that contains some pseudocode (_but is it complete? Doesn't seem complete somehow..._)
Karth, I., & Smith, A. M. (2017, August). WaveFunctionCollapse is constraint solving in the wild. In Proceedings of the 12th International Conference on the Foundations of Digital Games (pp. 1-10).
https://adamsmith.as/papers/wfc_is_constraint_solving_in_the_wild.pdf