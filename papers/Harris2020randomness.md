# Randomness vs structure in networks

Kameron Decker Harris. U Western Washington. A Zoom talk at some other uni (?), for physicists studying dynamics of fluids (?).

#networks #ticket #kernel

See also: ticket, [[complexity]] (random networks), 

How do you model a real neuroscience network: with random networks or structured networks?

First conceptual networks, historically, in neuroscience: "A logical calculus of the ideas immanent in nervous  activity", McCulloch & Pitts. 1943. Proved that for every binary function, there's a (boolean) network that computes this function. For a certain class of functions, showed that there's a network producing every possible fucntion. (Didn't show how to find them though).

Compares this to CPGs ([[cpg]]), where you know the activity of a network, and try to infer the connectivity that can produce this activity. So they are trying to do something similar for the cortex, but statistically speaking. Created a random network, very heterogeneous, with excitatory and inhibitory neurons. With a proper model, you have slow global oscillations with switching between low and high activity, even though the network is totally random.

Various random networks, including geometric models (embed nodes first, then make connections probabilistic) and stochastic block-models (communities). Random vs stereotyped is not a dichotomy; in biology, usually we're somewhere on the spectrum.

**Kernel theory**: similar to Kernel support vector machines ([[svm]]). Input vectors are represented as points x_i in space $\R^l$. A kernel function K(x1,x2) takes 2 inputs and outpus a similarity value. Continuous, symmetric, positive definite ($c ^\top K c \geq 0$). Dot products, polynomials, radial basis funcitons ($\exp(-||Δx||^2 / 2σ^2)$) are all examples.

**Mercer's theorem**: Any kernel that matches these requirements, can be decomposed as a $Σ^\infty_i λ_i ψ_i(x_1) ψ_i(x_2)$ - an eigenfunction decomposition. A special basis that kinda comes with a kernel. An infinite dimension Hilbert space; you precompute ⟨ψ_i , ψ_j ⟩, and it enables all sorts of efficient calculations.

**Kernel theory of networks**: trying to undestand why ANNs work. An ANN can be imagined as a projection from inputs x to features φ (embedding layer), which leads to a kernel (_I didn't understand that_) This embedding is not necessarily lower-dim than the input; the whole point of kernel methods is that it's wider (higher dim), which allows simpler methods for next steps (like a classic radial  kernel that projects from a 2D to 3D, and thus allows to classify the inner blob with simple thresholding). He seems to claim that in neuroscience, in actual brains, it is sometimes the case. Mushroom bodies and cerebellum in particular, but also any sensory system that has neurons with receptive fields and tuning curves. It's like having lots of kernes, and upscaling the signal to it to aid recognition.

It's equivalent to moving to a random basis. They model balancing in insects (**halteres** and their role in flying), and vision ([[mnist]]) like that: like random remapping, and show that it works great, as long as there are some constraints on these features (as opposed to truly random, white noise features). In their case - smoothness is enforced. For vision, for example, it creates lots of blob-like functions, and learning stimulus classification (recognition) from these blob-functions is ~100 times faster than from the original signal.

Smooth kernels filter out noise components.

> Does not it sound a bit similar to what people say about [[echo]] networks?

# Refs

His lab: https://glomerul.us/

Rahmi & Recht 2008. - random features

Neal 1996, Williams 1997 - work on Gaussian processes

LeCun, Juusola & Song, 2017 - structured inputs

Fairhall - tuning curves