# A critique of pure learning, Zador 2019

Zador, A. M. (2019). A critique of pure learning and what artificial neural networks can learn from animal brains. Nature communications, 10(1), 1-7.
https://www.nature.com/articles/s41467-019-11786-6?fbclid=IwAR27VnUXg_liKxASBWnncj8WM2hn9tA0Qq6lKBcZyxIuvN9bAEEZJU8eLAc

Parent: [[deepneuro]], [[neurodevelopment]]
Related: [[Richards2019deepneuro]]

Animals are so good in learning quickly because they are hardwired (at least tentatively) for the task. Aka, "most of what young animals do is not learning, but is encoded in the genome".

> Why doesn't it seem that way for humans? Is it just because humans are born too early? Or are we slightly different? Do we go for deeper, harder-to-wire networks, as a payback for them being more expressive? But then of course, even in human learning, once it starts, is quite rapid. For example, in language learning, after about 1.5-2 years of procrastination, kids do first about 2 words a day, and then, at peak (around early school years) - up to 10 words a day! Every day!

Computers are less smart than a mouse (his examples: building a nest, stalking prey, loading a dishwasher). Human logic isn't hard; it's simple tasks that we take for granted that are truly hard. The good news however that once mouse intelligence is achieved, human intelligence is not that far.

**Genomic bottleneck**: history of evolution, compressed into developmental code, somehow encoded in the genome. ANN however are largely "team learning" (in terms of Nature / Nurture debate), and train networks that are "born" totally as a tabula rasa.

> Would transformers be a notable deviation from this pattern? [[transformers]]

A nice visual, metaphorical explanation of why large (and presumably more powerful) networks fare much worse on small data: polynomial overfitting + a quotation from the Spiderman. A back-of-envelope calculation of what would have happened if children learned like ANNs: assuming one "What is it?" question a second, a child would need to ask a question every second for a year, without sleep, and always get answers, to get enough labels to learn to discern images (1e7 labels). Obviously not what's happening.

Cites Lecun's "cake", but then contrasts it with animals that clearly come to this world at least somewhat hardwired.

In biological world, hard-wired-ness and ability to learn form a trade-off. A period of helplessness is a payback for higher behavioral flexibility at a level of a species (and thus broader adaptation, and lower vulnerability to environmental changes), but also potentially better performance at the level of an individual (because coding connectivity in the genome is hard).

> A bit of an exploration exploitation dilemma, huh? Having to learn more is like forcing a certain amount of exploration on every individual. See [[game]].

Moreover, it's not an either/or situation: in many cases biological networks are ready to learn, but either need, or may still learn something new. It is the propensity, the scaffolding that is innate. Because it is impossible to code the brain in the DNA explicitly (provides an order-of-magnitude calculation).

It's hard to compare ANNs to organisms though, as for ANNs **evolution** (which may be considered a form of supervised learning) and individual **learning** are kinda fused. And in a way, it's almost more similar to evolution than to individual learning.

The limited capacity of genome (**information bottleneck**) may act as a **regularizer** [[regularization]], selecting for networks that can be described via simple repeated patterns.

Proposes a **research program for ANNs** that may be phrased as "see convolutional networks? ([[convnet]]) We need more pre-wired, constrained, pattern-based stuff like that". But also, that we to build good networks we should be looking for **rules that build networks**, similar to how genome doesn't encode connections directly, but encodes rules that, if followed, create connections.