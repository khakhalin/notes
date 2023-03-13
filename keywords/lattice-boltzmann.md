# Lattice-Boltzmann Model for Fluid simulations

#simulation

Alternative to **Navier-Stokes**.

Lattice-Boltzmann is a statistical mechanics -derived method that I don't understand. Allegedly it processes flow around small features better (complex boundaries; microscopic interactions), and is well parallelizable. Can also "naturally" support suspensions and bubbles. The math seems much less intuitive.

The origin of this approach is simple, and is called **Lattice Gas Automata** (LGA). So, there's a lattice with several possible discrete directions of movement: 6 on a triangular lattice, somehow 9 (why not 8?) on a nearly rectangular lattice with diagonals (aka D2Q9, where the 2nd number refers to the number of possible speeds). Each point on the lattice may be occupied by up to one "particle" moving along one of the vectors (so the lattice node may be either empty, or have one particle). If more than one particle arrives at a lattice node at some particular time step, these particles collide according to some rule. The simulation consists of alternating **streaming** (propagation) and **collision** steps.

But what LBM is adding here is that instead of a boolean (0 or 1) particles at each node, we go to the average expected number, and instead of discrete algorithmic collisions we go to statistical collisions.

# Refs

* Potentially a good collection of references (including practical applications in biology and what not), but need to be looked through: https://palabos.unige.ch/publications/

* https://en.wikipedia.org/wiki/Lattice_Boltzmann_methods

* Some implementation (in C, with Cuda) that seems to work: https://github.com/DigitCell/LBM_physics_experimnts/blob/main/LBM_physics_exp/mainloop.cpp