# Langchain

Parents: [[llm]]
See also: [[rag]], [[mcp]]

#text #llm


LangChain is a framework foor working with [[llm]]s, written in [[python]] and [[javascript]], installable as a python package. OSS core (packag, integrations), commercial platform on top.

The package standardizes LLM interfaces (via `.llms` subspace), standardizes `.retrivers` for getting data to feed to LLMs (e.g. [[knowledge_graph]] or vector-based [[rag]] implementation), and other fun tools, but most importantly, it helps to define chains of actions (control flows, orchestration, observability). A ready interface for structured output.

Offshoots: `LangGraph` for stringing possible states as a graph, for "multi-actor interactions".

As of mid-2025 there's a claim that langchain is obsolete, and [[mcp]] is the future.

# Refs

https://en.wikipedia.org/wiki/LangChain

Official stuff:
* https://www.langchain.com/
* https://github.com/langchain-ai
* https://python.langchain.com/docs/introduction/
* https://python.langchain.com/docs/concepts/
* Starting docs:
    * https://python.langchain.com/docs/concepts/why_langchain/
    * https://python.langchain.com/docs/concepts/architecture/