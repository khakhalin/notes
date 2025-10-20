# Job-Shop Scheduling

Parents: scheduling, optimization
See also: [[rl]], [[petri_nets]], [[np-complete]], [[constraint_programming]]

#scheduling #np-hard 


.

Ways to solve include:
* [[petri_nets]] - all JSS-specific papers that use Petri Nets are given there
* [[gen_alg]] - genetic (evolutionary) algorithms
* [[deep_graphs]] - graphical networks
* attention networks (see [[transformers]]) and [[llm]]-like approaches
* Hierarchical Task Decomposition, Hierarchical Task Networks

# Representation

.

# Evolutionary algos

Helliwell, T. J., Morgan, B., Vincent, A., Forgeoux, G., & Mahfouf, M. (2021). Reconfigurable Scheduling as a Discrete-Event Process: Monte Carlo Tree Search in Industrial Manufacturing. In IN4PL (pp. 151-162).
https://pdfs.semanticscholar.org/4334/cf2778f7c4a7977ec6a7d4c81ab4351bc5fb.pdf
Start with a fun statement that there's rich theory behind scheduling, but it's not used in practice, as everything is too slow and too formal. Suggest that symbolic reasoning and maybe even [[petri_nets]] are a promising way forward, as we need to be able to generate scenarios effortlessly without costly search for solutions. Mention Markov Processes, Finite State Automata and Finite State Machines. Criticize genetic algorithms [[gen_alg]] as too unaware of objectives behind the process. (But I guess they have an ax to grind, as they want to promote Petri Nets). use Monte-Carlo Tree Search to explore trajectories. An absolutely lovely figure with trajectory mutation: they pick the best trajectory (they call it an Elite Trajectory) and bifurcate it. They don't do RL, and don't really discuss representation.

🔥 Open questions:
* Allegedly GenAlgos can be used to bootstrap policy. How, in practice?

# RL for JSS

Echeverria, I., Murua, M., & Santana, R. (2024). Offline reinforcement learning for job-shop scheduling problems. arXiv preprint arXiv:2410.15714.
https://www.semanticscholar.org/reader/a4061661eb2dbef4ce7853fe422838a5a52592a1

Zhang, C., Song, W., Cao, Z., Zhang, J., Tan, P. S., & Chi, X. (2020). Learning to dispatch for job shop scheduling via deep reinforcement learning. Advances in neural information processing systems, 33, 1621-1632.
https://www.semanticscholar.org/reader/d594764273c02b5a3bbdc4a8d49979a23ad1f125
PDR: priority dispatching rule, a heuristic of decision-making instead of full strategic optimization.

Song, W., Chen, X., Li, Q., & Cao, Z. (2022). Flexible job-shop scheduling via graph neural network and deep reinforcement learning. IEEE Transactions on Industrial Informatics, 19(2), 1600-1610.
https://ink.library.smu.edu.sg/cgi/viewcontent.cgi?params=/context/sis_research/article/9200/&path_info=2022_TII_songwen.pdf

R. Chen, W. Li, H. Yang, A deep reinforcement learning framework based on an attention mechanism and disjunctive graph embedding for the job-shop scheduling problem, IEEE Transactions on Industrial Informatics 19 (2) (2023) 1322–1331.doi:10.1109/TII.2022.3167380.

Gebreyesus, Goytom and Fellek, Getu and Farid, Ahmed and Fujimura, Shigeru and Yoshie, Osamu, Gated–attention model with reinforcement learning for solving dynamic job shop scheduling problem, IEEJ Transactions on Electrical and Electronic Engineering 18 (23) (2023) 932–944.