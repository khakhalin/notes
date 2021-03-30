# Kosaraju-Sharir algorithm

Parents: [[09_Graphs]] / [[algos_graph]]


A linear time algorithm (O(V+E)) for finding strongly connected components in a directed graph. Boils down to the following:
* Using DFS, calculate reversed-postorder (a proxy for topological order). Call it L.
* Reverse the graph. Run DFS on this graph, initiating new DFS recursions in the order of L. Every part on which DFS propagates recursively until completion is its own strongly connected component.

> Sedgewik calls this algorithm "an extreme example of an algorithm that is easy to code, but difficult to understand". That's a great example of a comment you're grateful for, as you read a textbook!

The trick here is that reversed-postorder will always place sink nodes (dead-ends) after nodes leading to them. Curiously, it will happen both if it samples the dead-end node before the feeder node, and it it taps the feeder-node first (even if the feeder is tapped first, because of post-order, it will be left last). So either way, downstream nodes will always trigger before upstream nodes. And because of prepending (aka "reversed"), in the final list they will aways come after feeder nodes (either immediately, or after a gap, but it doens't matter). 

But this means that at the 2nd step we'll always try to reverse-DFS-propagate from upstream nodes first. Which means that once downstream nodes are tapped at the 2nd step, we won't be able to reverse-propagate from them, as all upstream nodes will be already visited. So in all unidirectional chains every node will become its own component. And also, we'll never be able to propagate upstream from SCCs, as by the time we reach any of them, all upstream nodes will be scorched (visited).

All larger strongly connected components will behave like cycles though; each node will be upstream in some way and downstream in the other, so once it is tapped, we'll propagate through it. Obviously, we won't be able to go downstream from them, as in a reversed graph you don't go downstream, you go upstream. And we won't be able to go upstream, as it was just described above. So the DFS will halt once the component is exhausted.

It is essentially a sketch of a proof, but all formal proofs I could find are extremely annoying, as they still have to go through these cases logical-tree-style, but also try to come up with a formal description of it that is more verbose than words.

> Many online explanations of this algorithm are confusing. For example [this one](https://www.geeksforgeeks.org/strongly-connected-components/) describes everything correctly, but represents the stack in reversed order (first element last), which is confusing. [Wiki](https://en.wikipedia.org/wiki/Kosaraju%27s_algorithm) is better, but doesn't call either recursive process DFS, and uses different names for them (but at least it's actionable). Also Sedgewik does 1st run on reversed graph, and 2nd run on direct, but most online sources do it the other way around. The best way to convince oneself that it works at all is to do it a few times for different graphs.

Related topic: **Reachability**. In particular, **Transitive closure**: a directed graph H is a transitive closure of a directed graph G, if it satisfies the following: H has an edge i→j iif j is reacheable from i in G. It means that H has all original edges from G, plus all loops i→i, plus for every starting node i, all j that are reachable from i. H are typically dense (have many edges). Effective algorithms for that are still a valid research area.