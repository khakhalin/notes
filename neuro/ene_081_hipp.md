# Hippocampus

Plan:

* How can memory work? How can we build memory from Hebbian plasticity?
* Let's start with recognition. We need to reconstruct some sort of "whole" from a cue
* The idea of interconnected elements. Let's imagine that at consecutive time steps (in discrete time) neurons can repeatedly activate each other.
* Simple example with 3×3 grid, and two bars - horizontal and vertical. Enumerate neurons, write down which ones are connected. Then look at some scenarios:
    * Activate 2 → entire bar
    * Activate 4 → another bar. It works!
    * What if both 2 and 4? 6 and 8 get one vote, 5 gets 2 votes. At this point a new player kicks in: inhibition. Let's say it's tuned to always get 3 neurons active, and we have 7 "votes" right now. If we remove 4 at random, sometimes we'll get horizontal, sometimes vertical, and sometimes we'll remain undecided and everything will repeat until we get a stable bar. (Because we can see that a bar is stable). We got hesitation! But also ambiguity. How does it help?
    * Here's how: imagine 2, 4, and 8. This time it's much more likely that we'll recognize it as a vertical bar. We can tolerate noise now!
* So if a system like that is in the brain, how can we expect connections to look like? Recurrent connections. And behold - hippocampus!
* But aren't recurrent connections dangerous? Yep, they are, and it is a most common focus for epilepsy.
* Is hippocampus really about memory? Oh yep!:
    * LTP was discovered there
    * What happenes if you cut it out
        * Korsakoff syndrome
        * Henry Molaison
* Now, imagine a mouse in a tunnel, running around, and recognizing places. How will the activity look like? Every cue will trigger the entire pattern, and most neurons will belong to only one distinct pattern (or recognition will be unstable), so if you connect to a random neuron, you can expect it to be activated in a certain spot of the maze, regardless of how the rat arrive there, whether it saw it, or smelled it, or touched it, etc. And indeed - place fields!
* Huge thing.
* Then from there mention grid cells, but without an explanation.