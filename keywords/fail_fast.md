# Fail-Fast Principle, aka Circuit Breaker Pattern

Parents: [[software_design]], [[design_patterns]]
See also: friction, technical_debt

#design


The idea is to introduce **intentional friction**, potentially even "catastrophic friction" of sorts at key points, to make sure that in the long-term the proces doesn't degrade, as issues are not ignored. Intentionally creating a pain point, prioritising the long-term longevity of the process to the efficiency of each individual run of the process.

For example, when bad data is coming in, one can silently fix it, or fix it with a warning, or crash very loudly. Opting for stability means that over the time the data will degrade, as people will either not notice the problems, or will ignore them, or will deprioritize them. On the contrary, building in an intolerance of invalid states may annoy people and endanger individual runs of the process, but would guarantee that the process doesn't degrade in the long-term.

An opposite of **Graceful Degradation**, prevents error accumulation and techical debt.