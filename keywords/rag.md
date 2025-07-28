# RAG: Retrieval-Augmented Generation

Parent: [[llm]]
See also: [[knowledge_graph]], [[langchain]]

#llm #text


RAG is an approach and a practical goal, while [[knowledge_graph]] and [[langchain]] are tools to reach this goal. Reaching out and retrieving context before responding to a user. Limits confabulations.

Typically includes storing pieces of text, with their semantic embeddings put in a vector database, for thematic retrieval. In the simplest case, the pieces of texts are achieved using "chunking" - which is almost a random splitting of text into fragments, with some overlap.

After retrieving a draft set, we can try to use an LLM to weed false-positives out (aka "Late-interaction Search").

Alternatively we can use a pre-computed [[knowledge_graph]] to link topics.

# Footnotes

* https://en.wikipedia.org/wiki/Retrieval-augmented_generation