# Genetic Algorithms

#bib #evolution #gen_alg

Parent: [[13_MC]]

Applications covered
* [[Ryoo2019video]] - Architecture search for video processing
* [[Kowalski2020arena]] - Deckbuilding

# Quality-Diversity
An approach to evo algorithms in which you don't just pick individuals based on top fitness, but to to optimize the diversity of solutions. Apparently also known as **illumination**.
https://quality-diversity.github.io/

List of papers: https://quality-diversity.github.io/papers

One of the productive algorithms in this family: Multi-dimensional Archive of Phenotypic Elites (MAP-Elites). The idea is that on top of a objective function (target to maximize or minimize) we introduce dimensions (like, different aspects, properties of the solution). Then each dimension is divided into bins, and thus the entire space is sections into cubes. The algorithm looks for a good solution within each cube, for each intersection of bins, and retains top performances in each cube. If the cube is empty, so be it.

Mouret, J. B., & Clune, J. (2015). Illuminating search spaces by mapping elites. arXiv preprint arXiv:1504.04909. https://arxiv.org/pdf/1504.04909.pdf