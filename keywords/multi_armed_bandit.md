# Multi-Armed Bandit

#rl #game

Parent: [[game]]
Related: [[levy_walks]], [[Zador2019pure]]

A mathematical abstraction for **exploitation / exploration** problem. Several entities (metaphorized as gaming slot machines), each with a certain distribution of gains. The agent should decide which choices to engage with (how to allocate resources) to maximize gain. 

The exploitation / exploration problem arises from the fact that to maximize gain, the agent needs information, but harvesting information means investing in choices that yield lower gain. It's as if each choice produced 2 types of resources: gain and information, and one type (information) was needed to produce more of the other type (gain).

Is also known as **stochastic scheduling**.