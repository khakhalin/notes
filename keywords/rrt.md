# Rapidly Exploring Random Tree (RRT)

#algo #monte-carlo #graphs

Stems: [[algos]] / [[algos_graph]] / [[algos_trees]]

A way to quickly build a space-filling tree, or find a path around an obstacle (e.g. in motion planning). Relies on sampling points from the space randomly, and then trying to reach it from the closest point in the existing tree, which makes is related to **Voronoi** regions.

Parameters:
* Initial point
* Step `s`

Algorithm:
```
tree = original point
while not happy:
    q = random point in the search space
    w = closest point in the existing graph
    branch = []
    x = w
    while not colliding(x,q) and 
          not colliding(x, tree) and 
          not colliding(x, forbidden_areas):
        x = x + step(length=s, direction=q-w)
        branch.append(x)
    tree.add(branch)
```
where `colliding()` checks for collisions, and `step()` is some imaginary function that calculates the direction, normalizes it, and then scales it down to a small enough step. Also, if we actually reached the point q at the end, it may be nice to add q itself to the tree at the end.

**How to detect collisions** between the potential branch and the existing tree, as well as with forbidden areas?
* If the step is small enough, it may be enough to make sure points are not closer to each other than step/sqrt(2). Simple Eucledian distance-based threshold
* For forbidden areas, some actual intersection algorithms may be needed.

According to wiki, is a basis for very many RTT-based methods, from more obvious (add heuristic to random sampling) to fancy. For robotic movement planning, for example, it is good to keep track of the **angle** of "movement" from each point to the next, and only allow them to change in relatively small increments (from current angle to target angle), to avoid impossible turns.

# Refs:

* https://en.wikipedia.org/wiki/Rapidly-exploring_random_tree	
* http://msl.cs.uiuc.edu/rrt/about.html