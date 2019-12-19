# Breaking down hierarchies of decision-making in primates

Hyafil, A., & Moreno-Bote, R. (2017). Breaking down hierarchies of decision-making in primates. eLife, 6, e16650.

#neuro #decisions

A follow-up on the earlier paper, with a new analysis of old data:
Lorteije, J. A., Zylberberg, A., Ouellette, B. G., De Zeeuw, C. I., Sigman, M., & Roelfsema, P. R. (2015). The formation of hierarchical decisions in the visual cortex. Neuron, 87(6), 1344-1356.
https://www.sciencedirect.com/science/article/pii/S0896627315007096

The gist of it is that they have lots of behavioral data from a 2-state decision process (monkeys took an action consisting of 2 choices one after another, with all information available upfront). They try to figure out whether the decision process was flat (all options considered together, and then a path implemented once), or hierarchical (first large-scalle choice, then smaller-scale choice). In that older paper they said "based on model fit, it's definitely hierarchical". In this new paper they are saying "nah, flat model actually works great as well, so we cannot claim hierarchical"

The most interesting thing about it all is the method they used. It's a nice case of using decision theory, and a curious introduction to models they use.

Experiments: described in Lorteije 2015. They had a binary tree 1>2>4 drawn on the screen, and colors of branches, at branches points, slowly randomly drifted. A monkey needed to move its gaze along the tree, on command, always picking a lighter branch. Which means that it had to take 2 decisions in a row: first bifurcation, and then the 2nd (one in each branch). They looked at psychophysigological curve (probability of choosing a branch as a function of the difference in lightness), and some other stats that I don't understand.

Model: better described in Lorteije 2015, but then here they claim that the old model is actually wrong. Originally they had each target's score = ∫ over time of k(∆luminance) for relevant branches, and then it drifts until one of the scores wins (crosses the threshold). And that's how in 2015 they compared flat (all targets are running against each other) and hierarchical (first one decision, then another one) models. Now they introduce leak (each score leaks, by additing -av to the formula for dv step), replace ∆luminance with abs(∆luminance), and add concurrent inhibition (each v_i inhibits other v_j by being present as bv_i in their dv_j).

And somehow they claim now their flat model fits the data just as well as the hierarchical model used to fit it.

That's all really strange, and I don't understand how one can make these sorts of claims using these sorts of techniques, but it's a curious window into the world of decision-making models :)