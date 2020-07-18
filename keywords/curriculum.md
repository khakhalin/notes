# Curriculum learning

#curriculum

Papers:
* [[Baker2019autocurricula]] - Hide-and-seek RL from OpenAI

> Did anybody try to perform stochastic gradient descent with automated adaptive curriculum? Could there be a value in periodically looking at the directions of the gradient for “best” data points, picking those that are “most interesting”, and paying more attention to them? Like, what if we put data points in a priority queue based on their error, or abs(gradient), kinda like people do when studying foreign languages with Anki. Then do SGD. Useful points would be revisited more often, then fall back once they become useless? It kinda sounds vaguely like gradient boosting, but may also make sense geometrically: if gradients for most points point lie within a hyperplane (as we reached a saddle), but for a few points you have vectors that stick out, maybe we should follow them?