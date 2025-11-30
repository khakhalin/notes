# Proximal Policy Optimization

Parents: [[rl]]
See also: 

#rl


PPO is the most popular RL method ever.

The policy is typically represented by a [[dl]] network. We start with generating a bunch of traces, following the decisions produced by the **actor** network (the one generating the policy). We then compute **advantages** that measure how much each action from every state was better or worse compared to the average action from this state. Then we calculate the update signal for the actor network (we will use this update signal in the same way in which loss is used in supervised DL). Critically, PPO does some fancy and very specific thing when calculating the update signal, called "clipping", which is similar to limiting gradients, and was just shown to work really well in practice, for some magic statistical reasons. We then do standard [[backprop]] through the network with this update signal. Note that we're updating the network not after every step, and not even after finishing the trajectory, but after making a bunch of trajectories, as we need some material to draw from.

But how do we calculate the advantage during agent's action, how do we know the value of an action? We look to which state they lead, and we use another network (a **critic** network) to predict values of these states. The values we are hoping to use here, and that we are trying to predict with a network, are the "true values" of states, defined as sorta "rewards", blurred backwards in time: $V = E(\sum_t γ^t R_{t+1})$, or the average probability of eventually getting rewards from this state, assuming the current policy π, and with rewards discounted by γ per step. But to truly calculate V we need an infinite number of trajectories, so instead we are predicting these values. After collecting a batch of trajectories, we use the actual returns (computed from the rewards and the estimated values of subsequent states) to update the critic network. The update is done by minimizing the mean squared error between the critic’s estimated value of a state and the actual discounted return observed from that state over this collection of trajectories. This process allows the critic network to learn value estimates, which are then used to compute the advantages for the actor network.

In practice, actor and critic networks often share first layers (the embedding part), but have different heads.

# Refs

https://en.wikipedia.org/wiki/Proximal_policy_optimization