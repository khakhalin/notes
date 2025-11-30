# RL interviews

Parents: [[job_search]], [[rl]]
See also: [[ml_questions]]

#interview #rl


# Part 1 - Basics

> Define reinforcement learning and explain how it differs from supervised and unsupervised learning

Listen for clarity on the feedback loop, agent-environment interaction, and long-term reward maximization. Weaker candidates conflate RL with other paradigms; strong ones explain the sequential decision-making aspect

> Define the core components: agent, environment, state, action, reward, and transition dynamics

Transition dynamics may be deterministic or non-deterministic. Also it may be model-based (we know transition rules) or model-free (we have to learn the value of policies from experience).

> For a job-shop schedulig task, define the state, the actions, and the reward

State: The current configuration of the shop, including the status of all jobs (which tasks are pending, in progress, or completed), the status of all machines (idle, busy, or broken), and the allocation of resources to jobs.​

Actions: The decision to assign a specific job to a specific machine, or to move a job from one machine to another, or to start or stop a job on a machine.​

Reward: A signal that reflects the quality of the scheduling decision, such as minimizing the makespan (total time to complete all jobs), reducing idle time, or penalizing constraint violations (e.g., missing deadlines or resource conflicts). Typically, episode-level reward at the end of the episode.

> "What is the Markov property, and why does it matter for RL?"

Strong candidates link this to state representation quality and explain the trade-off between state simplicity and information loss.

# Part 2 - Details

"Explain the value function V(s) and action-value function Q(s,a). What's the relationship between them?" Expect candidates to describe V as expected cumulative reward from a state under a policy, and Q as the same but for state-action pairs. Probe: "How would you estimate these in practice?"​

> "Derive or explain the Bellman expectation equation and Bellman optimality equation." This separates deep understanding from surface-level knowledge. You're not demanding a derivation from scratch, but they should explain the recursive structure and why it enables iterative algorithms.

The Bellman optimality equation expresses the value of a state (or state-action pair) under an optimal policy as the maximum expected return achievable from that state. Recursive Structure: Similar to the expectation equation, but now the value is the maximum over all possible actions, reflecting the optimal policy. Iterative Algorithms: This recursive structure enables algorithms like value iteration and Q-learning to iteratively improve value estimates and converge to the optimal policy.

"What's the relationship between the discount factor (γ) and the problem's time horizon?" Strong answers connect γ to planning depth—a candidate should mention that higher γ emphasizes distant rewards, which matters for long-horizon scheduling problems.

"Explain the exploration-exploitation dilemma. How do ε-greedy, UCB, and Thompson Sampling handle it?" Look for understanding of the risk-return trade-off and when each method is appropriate. A strong answer mentions the regret bound or efficiency considerations.​

"Compare model-free and model-based RL. What are the pros and cons?" Model-free (Q-learning, policy gradients) trades sample efficiency for simplicity. Model-based trades computational cost for sample efficiency. For an NP-hard scheduling problem, this trade-off is critical.​

"Explain Q-Learning and SARSA. Why is Q-learning off-policy and SARSA on-policy?" Weaker candidates memorize the algorithm steps; strong ones explain why off-policy learning enables more efficient use of exploration trajectories, and why that matters for sample-constrained environments.

# Part 3 - Specific questions

> "What is reward shaping, and how could you use it to guide learning?" This is especially relevant for your scheduling domain. Strong candidates discuss potential-based reward shaping and intrinsic motivation. Probe: "If your scheduling problem has very sparse rewards (only at episode end), how would you design intermediate rewards without biasing the optimal policy?"​

Small intermediate rewards, e.g. for job completion, making the queue shorter etc. Can make learning much faster, but lead to suboptimal solutions.

> "Describe a scenario where naive reward design fails. What went wrong?" Listen for understanding of reward specification challenges—candidates familiar with real projects often mention unintended behaviors or local optima.

"If you had 100,000 simulator samples to train on, would you choose model-free or model-based? Why?" This forces trade-off reasoning. A strong answer considers the problem's complexity, discount factor, and whether you can afford the computational overhead of planning.

"Mention policy gradients (REINFORCE, Actor-Critic) and compare them to value-based methods." Explain that policy gradients directly optimize the policy, work naturally with continuous action spaces, and have better convergence properties but higher variance. Actor-Critic combines both for reduced variance.

# Part 4 - Practical implementation

State and Action Spaces: "What information would the agent need in its state? How many actions are feasible at each step?" Strong candidates ask about observation dimensionality, temporal dependencies, and whether the state space is fully observable or partially observable.​

Reward Structure: "How would you define success? What are the business metrics?" Push on sparse vs. dense rewards, the impact of delayed feedback, and whether a single reward signal suffices or if multi-task learning is needed.​

Scale and Constraints: "How many facilities? How far into the future must we plan? What's the tolerance for computation time—real-time decisions or batch overnight?" Strong candidates use these constraints to inform algorithm choice immediately.

Feasibility vs. Optimality: "Is finding any feasible schedule valuable, or must we optimize? Is there a gap between feasible and optimal that matters for the business?" This tests understanding of NP-hardness and pragmatism.​

"Given the problem's characteristics, what RL algorithm family would you choose: value-based (Q-learning, DQN), policy-based (policy gradient, PPO), or actor-critic?" Strong candidates reason through: Is the action space discrete or continuous? Do we need off-policy learning for sample efficiency? Is the reward signal sparse?​

For scheduling, discrete action spaces and sparse rewards often favor policy gradients with reward shaping or actor-critic methods over Q-learning.

"Would you use model-based RL? Why or why not?" Model-based has sampling advantages but computational overhead. For an NP-hard problem where the transition dynamics are complex but deterministic (given current state), a learned model might be valuable—or it might be a waste if the real environment is fast to simulate.​

"What about hybrid approaches—using RL with other optimization techniques?" Strong candidates mention combining RL with constraint programming, mixed-integer programming, or local search heuristics. This is crucial for NP-hard problems where pure RL alone may struggle.​

"For NP-hard problems, should we aim for optimal solutions or good-enough heuristic solutions?" The pragmatic answer is context-dependent. Candidates who recognize that RL naturally provides anytime algorithms (can always use the current policy's output) are showing systems thinking.

Training Data: "Where do you get training data? Simulator, offline logs, or active interaction?" For scheduling, simulators are usually available. Strong candidates discuss sample efficiency and data augmentation strategies.​

Feature Engineering and State Representation: "How would you encode the scheduling state efficiently? What features matter most?" This tests domain intuition. For oil rig scheduling: production status, resource availability, demand forecast, equipment state, etc. Can this be represented as a fixed-size vector, or do you need more complex encodings (graphs, attention)?​

Model Training Cadence: "How often would you retrain? Offline nightly batch, or continuous online learning?" Strong candidates discuss distribution shift, model staleness, and business requirements. For scheduling, overnight batch training after a production cycle often makes sense.​

Inference Serving: "What's the latency requirement? Does the system need to recommend schedules in real-time, or is it okay to run overnight?" This drives infrastructure decisions (edge model vs. central service).​

Monitoring and Feedback Loops: "How would you detect if the learned policy is degrading in production? What metrics would you track?" Listen for understanding of reward shift, performance drift, and the importance of logging action-outcome pairs for offline evaluation and retraining.​

"How would you handle the complexity if the problem size is large (e.g., 1000 facilities)?" Options: decompose into subproblems, use hierarchical RL, combine RL with constraint relaxation, or sample-based approximation algorithms

"Would you use a learned heuristic (RL) as part of a broader solver?" For instance, RL could learn a heuristic policy that generates an initial feasible solution, which a local search or branch-and-bound then refines. This hybrid approach is pragmatic and shows systems maturity.

"How would you measure whether the RL solution is better than the current baseline?" They should discuss offline metrics (solution quality, feasibility), online metrics (impact on production KPIs), and A/B testing if applicable.

 "Implement a basic Actor-Critic agent for a discrete action scheduling environment. Assume state is a vector, action is an index, and you have a small simulator. Show me the training loop." They don't need TensorFlow—pseudo-code in Python with NumPy is fine. Look for:​ **Correct Bellman backup**,  Gradient computation (or at least the intent), Exploration strategy, Testing on a toy problem

"Suppose generating a valid schedule (trace) is computationally expensive or rare. How would you balance exploration and exploitation in this setting to avoid wasting samples on invalid or low-quality traces?" (Tests understanding of sample efficiency and smart exploration strategies. Strong answers mention guided exploration, curriculum learning, or injecting prior knowledge.)​

"How would you design an exploration strategy that leverages heuristics (e.g., greedy rules, local search) to guide the agent toward promising regions of the state-action space, while still allowing for the discovery of novel solutions?" (Probes hybrid approaches: combining heuristics with stochastic exploration. Look for candidates who mention epsilon-greedy with heuristic bias, or UCB with heuristic priors.)​

"If you had access to a set of pre-existing (suboptimal) schedules, how would you use them to warm-start your RL agent or pre-train its policy/value network? What risks might this introduce?" (Tests awareness of warm-starting and pre-training. Strong answers discuss imitation learning, behavioral cloning, or initializing with heuristic policies, and mention the risk of policy collapse or overfitting to suboptimal traces.)​

"In what ways could you use offline data (e.g., historical schedules) to improve the sample efficiency of your RL agent, especially if online interaction is limited or costly?" (Probes understanding of offline RL, batch RL, or hybrid offline-online learning. Listen for ideas like reward shaping, dataset aggregation, or offline policy evaluation.)​

"How would you design your reward function to encourage the agent to discover valid schedules early in training, while still incentivizing long-term optimality? Could you use intrinsic rewards or curriculum learning?" (Tests creativity in reward design and curriculum strategies. Strong answers mention shaping rewards for feasibility, or gradually increasing problem complexity.)​

"What are the trade-offs between using a completely random initialization for the policy and starting from a heuristic-based initialization (e.g., a rule-based scheduler)?" (Probes understanding of initialization strategies. Strong answers discuss faster convergence vs. risk of local optima, and the importance of escaping from heuristic biases.)​

"How would you handle the case where the environment is partially observable or the simulator occasionally produces invalid or infeasible traces? Could you adapt your exploration strategy or use robust RL methods?" (Tests robustness and adaptability. Look for answers that mention safe RL, constraint satisfaction, or filtering invalid traces.)​

"If you wanted to combine RL with constraint programming or other optimization techniques to generate traces, how would you structure this hybrid approach, and what role would RL play?" (Probes systems thinking and integration with other methods. Strong answers mention RL as a meta-heuristic, or using RL to guide search in a larger optimization framework.)​