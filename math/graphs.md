# Math about graphs
#net

See also: [[la_net]] - linear algebra about graphs

# Sorting and searching
**Topological order** (aka topological sort): any sequence of vertex ids, such that for no directed edge A→B, vertex B is given in the sequence before vertex A. Is only possible in a DAG (in a graph without directed cycles), as obviously a directed cycle makes a sequence like that impossible to construct.

In general, 2 main ways to explore graphs:
* **DFS**, aka **Depth-First Search**
    * Recursive: often easier to code.
    * Takes less memory than BFS (no need to store a queue)
    * Naturally creates **topological sorting**: the order in which it _leaves_ nodes (after processing them and all their dependencies) is a valid topological order (that's assuming that you add elements at the beginning of your list, as in stack, rather than at the end. It's called **reveresed postorder**).
    * _According to [this quora response](https://www.quora.com/What-are-the-advantages-of-using-BFS-over-DFS-or-using-DFS-over-BFS-What-are-the-applications-and-downsides-of-each), also has some other benefits that I don't quite understand yet: finding bridges, connected components, articulation points (aka cutvertices), planarity test._
* **BFS**, aka **Breadth-First Search**
    * Naturally finds a **shortest path** from the root node (while DFS can first arrive at a vertex in a very roundabout way)
    * Creates a more balanced tree (with more similar distances from the root to the furthest points in each branch)

