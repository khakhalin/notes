# Constraint Programming

Parents: [[linprog]]
See also: 

#linprog


This problem is easier to introduce through an example. Imagine a bunch of agents that need to do things. If you try to describe this system as a vector of variables, and then create a trajectory (valid timeline) for this bunch of agents, you'll notice that not all things go, as there are a bunch of constraints that have to be true for this trajectory to be valid (to be possible irl):
* Only one agent can be at any given spot at a given time
* When agents interact directly, they need to be close to each other
* Agents can't move infinitely fast
* Some actions have to be performed in sequence, etc.

How to formalize that? Once you defined a vector of variables to describe your system, you suddenly notice that
1. Some values are limited in a 0 < x_i < max_value style, defining an arithmetical constraint
2. Some variables can only take a list of values (existential constraint)
3. Some are logical: e.g. all coordinates should be different, or at most one agent can be doing something etc.

Typically, we want to find a solution to satisfy these constraints. When time is involved, a solution is often a trajectory that satisfies these constraints.

Similar to annealing and splitting in [[clustering]], there are two main strategies for building a solution:
* **Refinement** - you start with a situation where all variables are free, and then improve it step by step by limiting values of variables and propagating the changes (aka "constraint propagation")
* **Perturbation** - you start with a situation where all variables are given the same value (?), and then change them one by one, also propagating the change. _Not sure I get it. Wouldn't a proper "perturbation" be if you find a trivial solution, and then deviate it in a way that still satisfies the constraint, thus getting different solutions? But that's not what Wikipedia says, as "all variables are given the same value" is not a solution, almost certainly, in most cases_

How to find a solution?
* **Backtracking** - exploring the tree with depth-first traversal. Propagate constraints forward until you get a contradiction, in which case return to the last fork and try the next solution. Enumerating your option at each fork becomes an intersting sub-problem here (to be able to iterate). See also: [[algos_trees]]
* **Local search** - try to wiggle the solution in the vicinity of each unsatisfied constraints. Afaik, no guarantee?
* Dynamic programming - _not sure I immediately see how to use it here?_

For morphing an existing solution **local consistency** becomes a cool tool. If you can change the solution locally in a way that preserves at least some constraints, it can help with perturbation method and local search. _Would be nice to have some more practical algo sketches here._ Simple cases: node consistency (only making sure that all variables of a single node are agreeable), arc consistency (making sure that nodes that are connected are still coordinated), path consistency (the same, but for a whole path within a graph). 

The complexity of constraint optimization problems isn't known apriory and depends on the task. Some are polynomial and can be solved with [[linprog]], but some are NP-hard.

Interesting keywords:
* Temporal concurrent constraint programming

# Refs

CP-SAT solver, written in C, runnable from Python, supported by Google.
* https://developers.google.com/optimization/cp/cp_solver
* https://en.wikipedia.org/wiki/OR-Tools

Relevant wiki pages:
* https://en.wikipedia.org/wiki/Constraint_programming
* https://en.wikipedia.org/wiki/Constraint_satisfaction_problem
* https://en.wikipedia.org/wiki/Local_consistency
* https://en.wikipedia.org/wiki/Backtracking