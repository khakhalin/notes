# Chatbot in prod (productionalizing llm interfaces)

Parents: [[text]], [[llm]]
See also: [[rag]], [[langchain]], [[mcp]]

#llm #systems


Let's assume that we were asked develop a chat-based (LLM-based) automation system for an existing software solution. The automation should support three key use cases:

1. **Troubleshooting**. The client wants to be able to "chat" with their system, interrogating it about its recent performance, especially when the system failed one way or another.
2. **Initial setup**. The clients wants to be able to jump-start their tuning of the system via chat. They want to be able to describe the situation to the chat, and get a working set of configs / json files to bring their product supply system to a state that represents a simplistic, but working solution, that they would then fine-tune manually.
3. **Migration and updates**. The client wants to automate the most boring parts of their work, such as "repeat the same simple change for all 30 almost-identical elements that we have set up in the system".

Let's sketch an "Agentic AI workflows with multi-agent orchestration" to solve this type of problems.

As if Aug 2025, for a reasonable model (not the best, not the worst) we get about 25c per 1M tokens, and about 100k tokens context window, so about 2c per full "saturated" request.

# Techstack

* Backend: Python, FastAPI, Celery+Redis, Postgres, MongoDB.
* This backend will be leveraging LLM orchestration (LangChain. MCP).
* RAG on a vector DB (e.g. Pinecone, Weaviate or ChromaDB).
* Pydantic for validation of inputs to the system (jsons, yamls).
* Integration: Docker+Kubernetes, Prometheus/Graphana for monitoring, LangSmith/Langfuse for observability.

If we go full async, we'll have about 5-10 microservices:

1. Document ingestion: monitor inputs, parse multiple file formats, queue for processing
2. RAG Embedding and indexing: get document from a queue, chunk, embed, store. In a pinch, can be combined with (1)
3. RAG Query and retrieveal
4. LLM Orchestration: connect to provider, generate context, assign tasks, validate structured outputs
5. Chat API
6. All specific interfaces for the solution we are trying to automate (files, calls)
7. Monitoring and observability

# Knowledge artifacts

If there are technical docs, annotated logs, cheatsheets, summaries of "best practices" etc, then our life is simple, we just need to cut them into [[rag]]s. But what if documentation doesn't exist? What is the ideal documentation, for a project like that, if we are creating it for automation purposes to begin with, and not for a human to leaf through?

We want modular "knowledge atoms". A small encyclopaedia? Entries should be about chunk-long, so (as of 2025), 500-1000 tokens, or (in English) about 400-800 words. It's about a screen of text (3-4 paragraphs). Use [[markdown]], add metadata. We want an entry for:

* every error - what it means, what usually causes it, how to resolve
* step-by-step procedures with numbered steps
* playbook action plan  for every type of a problem
* every input of every interface
* troubleshooting tips
* config template
* common pitfalls (what not to do, and why)

Each knowledge atom should include:
* one-liner (or close to that) description of purpose and context
* if appllicable, prerequisites (role, system state, inputs)
* if applicable, examples (log snippets, code examples)
* Metadata (in a header, in yaml format):
  * title
  * author
  * atom type (e.g. categories from the list above)
  * creation date
  * version
  * a list of tags
  * a list of related titles

Socratic dialogues are in vogue again! One great format to use are **conversational FAQ pairs**: either include them at the bottom of every document, or as a separate genre of atomic documents. For example:
> Q: Why are some items shown in red?
> A: For these elements travel_time is smaller than sleep_period, which may indicate disease.

The best way to extract this information is via long recorded structured interviews. It's about 2-3 people working for about 2 months.