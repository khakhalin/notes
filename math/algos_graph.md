# Graphical algorithms

#algo #networks

Famous algorithms:
* [[dijkstra]] - Dijsktra's shortest path algorithm (from source to all nodes)
* [[a-star]] - A* : an improvement upon Dijsktra's
* [[floyd_warshall]] - shortest distance from all nodes to all nodes
* [[min_span_tree]] - minimal spanning tree (Prim and Kruskall)

See also:
* [[graph_intro]] - basic introduction to graphs
* [[la_net]] - linear algebra about graphs
* [[graph_th]] - graph theory (more of gropu theory kind)
* [[09_Graphs]] - ANNs for graphs

# Basic graph exploration

In general, 2 main ways to explore graphs:
* **DFS**, aka **Depth-First Search**. Benefits:
    1. Recursive: often easier to code.
    2. Takes less memory than BFS (no need to store a queue)
    3. Naturally creates **topological order** (see below).
    4. _According to [this quora response](https://www.quora.com/What-are-the-advantages-of-using-BFS-over-DFS-or-using-DFS-over-BFS-What-are-the-applications-and-downsides-of-each), also has some other benefits that I don't quite understand yet: finding bridges, connected components, articulation points (aka cutvertices), planarity test._
    5. Works great on shallow trees, but gets risky on linear linked lists, due to recursion depth.

* **BFS**, aka **Breadth-First Search**. Benefits:
    1. Naturally finds a **shortest path** from the root node (while DFS can arrive at a vertex in a very roundabout way)
    2. Creates a more balanced tree (with more similar distances from the root to the furthest points in each branch)
    3. Works great on long linked lists, but takes lots of memory on shallow trees.

**Time complexity** is O(V+E) for both. **Space complexity** for a tree, is tree height for DFS, and tree width for BFS. In the worst-case scenario, it's O(V) in both cases, just worst-case scenarios are different. On a balanced tree, it would be O(V) for BFS (because last layer has ~V/2 leafs), and O(log V) for DFS (tree depth).

# Topological order

**Topological order** (aka topological sort): any sequence of vertex ids, such that for no directed edge A→B, vertex B is given in the sequence before vertex A. Is only possible in a **DAG** (**Directed Acyclic Graph**: a graph without  cycles), as obviously a directed cycle cannot be linearized.

One can find topological order with **DFS**, if they consider the order in which DFS exploration **leaves** nodes (after processing them and all their dependencies). Except that, to get a final list, one needs to add elements to the beginning of a running list, as in a stack, rather than at the end (or add to the end, and then reverse once the run is over). It's called **reveresed postorder**: reversed because you add to the beginning of the list (aka **prepend**, instead of appending, which equivalent to writing to the end, then reading it backwards), and postorder because you do it as you leave each node, rather than as you enter them.

> If a graph is not a DAG, DFS still finds something similar to topological order, at least for DAG-like parts of it. Which is useful in many applications (see below), but is referred to awkwardly as **reversed postorder**.

Here's an illustration. Imagine a graph with edges `(1,3)(2,3)(1,2)`. A good topological order for this graph is `1,2,3`, but DFS will explore the nodes in a sequence of `1,3,2` (aka pre-order). However, if you consider that DFS is recursive, and so 1 won't be left until all its branches are explored, the order of _leaving_ nodes is 3 (after it was explored), then 2, then 1 (once all its dependencies are explored). So `3,2,1` is the postorder, and `1,2,3` is a reverse postorder, and also appropriate topological order. 

> How to find a good node to start? Unfortunately, it seems that the only way to guarantee it with a DFS-algorithm only, is by starting with a yet different node every time you haven't covered the entire graph when starting from the current node. Until you find a good node to start. That is, assuming that you know that the graph is connected, and so ordering is possible. Which is of course not ideal, as it is super-slow, and doesn't work if a graph is not fully connected.

> I am not quite sure it is true though, so this is something to be checked. #todo

An alternative to DFS is a **Kahn's algorithm**. First of all, notice that a DAG has to have at least one node with in-degree 0. (Proof: no cycles ⇒ all paths are finite ⇒ ∃ max path ⇒ starting node of this path has in-degree 0, or one could have extended the path at this end □). The algorithm ([ref](https://www.geeksforgeeks.org/topological-sorting-indegree-based-solution/)):
1. Calculate in-degree of all nodes. Create an empthy queue of "nodes to visit". Create an empty list of "visited nodes".
2. Add all nodes with in-degree of 0 to the queue.
3. Pick an element V from the queue, and mark it as "visited".
4. Reduce in-degrees of all elements to which V is connected by 1.
5. Go to "2", and repeat until the queue is empty.
6. If at the very end you haven't visited all nodes, topological sort is impossible.
How does it work?

# Strongly connected component

A **Strongly Connected Component** (SCC for the purposes of this section) of a directed graph is a maximal (largest) set of nodes for which each node is reachable from each other node.

Every cycle begets a SCC.

**Condensation**: represent each SCC with a single "super-node". Any graph can be presented as a DAG of nodes and "supernodes", standing for SCCs.

## Kosaraju-Sharir algorithm
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