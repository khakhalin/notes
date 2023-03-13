# Ising model

Parent: [[complexity]], [[chaos]]
Derived models: [[hopfield]], [[boltzmann_machine]]

#complexity #physics #chaos


A famous simplified model of magnetic interactions in a crystal that replicates phase transitions, and can be used as a model of practically anything.

A grid of vector magnets, each with two states (spins): parallel (+1) and anti-parallel (−1). Two tendencies that fight each other:
* Interaction between neighboring magnets organizes them (flips them parallel)
* Heat disrupts them

Energy: $\displaystyle H = -J\sum_{⟨i,j⟩}σ_i σ_j$ where $σ_i$ is a spin, and summation is taken over adjacent nodes ⟨i,j⟩. (There may also be an external field, and a term $-h\sum σ_i$, but all the fun stuff happens even without the external field).

A 1D chain of magnets fails to stay magnetized (apparently easy to prove; published in 1925). In 2D - phase transitions (proven analytically); for higher dimensions remains unproven. Moreover, well represents the statistical behavior around the transition (universality, criticality).

Most popular monte-carlo approach: **Metropolis algorithm**, although it critically slows and stalls around the critical point (and so has to be replaced with other algorithms). It flips nodes one by one, and with each flip assesses whether it increases or decreases the energy. If the flipped state is beneficial, always keep the flip. If it's not, only flip it with probability exp(-βΔH), to reflect heat.

> So why would this slow down around the critical point? Or do they mean that the _convergence_ to a reasonable statistically representative solution slows down?

# Refs

https://en.wikipedia.org/wiki/Ising_model

https://www.quantamagazine.org/the-cartoon-picture-of-magnets-that-has-transformed-science-20200624/