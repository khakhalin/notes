# RAG: Retrieval-Augmented Generation

Parent: [[llm]]
See also: [[knowledge_graph]], [[vectordb]], [[bm25]], [[chatbot_in_prod]]

#llm #text


RAG is an approach and a practical goal: before responding to the prompt in any way, reach out and retrieve some relevant context. 

In practice people often use the team "RAG" when they actually mean running a [[vectordb]] to store pieces of text together with their semantic embeddings, for thematic retrieval. And in the simplest case, the pieces of texts are achieved by "chunking": splitting of relevant texts (manuals, conversations) into pieces that are 500-1000 tokens long, with some 100-200 tokens overlap. If we compare it to the size of the context window (about 100-200k tokens), and given that RAG results are supposed to take up only a part of the context, we can afford to have some 15-25 chunks in the context. Also studies show (apparently) that going about 20 chunks doesn't improve the result.

After retrieving a draft set of chunks based on vector similarity, we can try to use an LLM to weed false-positives out (aka "Late-interaction Search" or "Multi-stage retrieval pipeline"). On top of a basic cosine distance in the embedding space, we can use keyword search, keyword similarity distance [[bm25]], a boost of recency etc. 

But in theory, this [[vectordb]] scenario is by far not the only option! One could both create pieces of text to be stored in RAG via a different process, and retrieve them with a different process. One alternative is a [[knowledge_graph]]. In this paradigm, you would project the prompt into a set of keywords, then follow all edges, checking what these records (keywords) are connected to, and retrieve hese records. On how to generate original (curated) material for a RAG, read here: [[chatbot_in_prod]].

Also, one can use **functional APIs**: for example, if you need to know the current state of the system, you can call some diagnostic, and put the output in the context, as one of the "pieces of text". Or run a database query. Or looking at some metadata (when working with documents).

# Learning-to-Rank

When deciding which snippets to use, one can also do "Learning-to-Rank": use vector similarity, bm25, snippet metadata, tag overlap (assuming that the request was also transformed into tags by a sub-process) and other context features (depths into conversation, qusetion length etc.) as features, and train a lightweight model (e.g. LightGBM ranker) to _predict_ rankings for each snippet. Use accumulated user feedback to train this model. 

One can also user contextual [[multi_armed_bandit]] such as **LinUCB** (Linear Upper Confidence Bound). You start with a hand-crafted relevance score. Then you give it to the bandit as reward prediction. The bandit calculates the score of each snippet that includes both our current belief about this score, and how "well explored" this snipped is, with a goal to maximize the feedback from the user. Then it updates both the exploration history, and the snippet score boost. 🔥 _I'm not sure I get it_

Footnotes:
* A Reliable Contextual Bandit Algorithm: LinUCB https://truetheta.io/concepts/reinforcement-learning/lin-ucb/
* Contextual Bandits Analysis of LinUCB Disjoint Algorithm - deep dive https://kfoofw.github.io/contextual-bandits-linear-ucb-disjoint/
* Contextual Bandits - official docs with some tutorials https://contextual-bandits.readthedocs.io/

# Context formatting in a prompt

In the prompt (context) the chanks are marked up with something like `RETRIEVED CONTEXT (20 chunks)`, then chunks one after another with some clear separator (potentially titles), then `END OF RETRIEVED CONTEXT`. it's also nice to group chunks by type, e.g. `RELEVANT CONFIGURATION FILES`, `SYSTEM LOGS`, `DOCUMENTATION`, `PREVIOUS SOLUTIONS` etc. Whatever markup is used to point at chunks, a similar markup should be used to point at themost recent user query, and at any other structural parts of the context.

# Ongoing curation

Typically, we would init a RAG with high-quality material (chunked documentation, or maybe even some custom hand-written FAQs), but we also want it to evolve and learn, so over time it will collect more low-quality data (e.g. low-quality session transcripts). How to avoid content degradation? On the flip side, at some point documentation will obviously go obsolete. Can we detect that and update the documentation without a manual review?

* One cheap method is to mark "ephemera" with a different tag, and deboost them based on recency more aggressively than supposedly "high quality" data.
* We can keep track of retrieveal stats (and maybe rejection stats if we do a 2-stage retrieval), and prune wost performers.
* Keep track of feedback (user satisfaction), and "backpropagate error", updating "acceptance scores" for these snippets. We can also try to use implicit feedback (e.g. meaningful follow-ups). As a simple approach, just change all scores. Or use their "retrieval ranking" that we should remember, and distribute proportionally. Or train a lightweight model (see **Learning-to-Rank** above).
* Keep a special tag for the "core" or "gold" subset, and review it manually.
* Agentic Crawlers - but that's an ongoing area of research I'm sure!

# Footnotes

* https://en.wikipedia.org/wiki/Retrieval-augmented_generation