# Client Best Practices

* Loading every tool definition into the model’s context window upfront wastes tokens and degrades performance

**Progressive discovery:**&#x20;

* Controls when tool definitions enter context
* The host fetches tool definitions via `tools/list` as normal, but defers injecting them into the model’s context.
* The host provides a lightweight `search_tools` meta-tool to the model.
* The host loads full definitions into context only as needed.
* Best used when tool definitions take large parts of the context window.&#x20;
* For a small set of tools with tool definitions taking up a small part of the context window, loading all tools is fine
* Implement a threshold as a percentage of the context window. For example, 1%-5%.
* **Discovery Strategy:**
  * **Keyword-based**: Keyword matching (BM25, regex). Simple and effective, particularly for descriptive tool names and descriptions.
  * **Embedding-based**: Vector-similarity retrieval over tool descriptions.&#x20;
  * **Subagent-based**: A secondary model, often a small and fast model such as Claude Haiku or Gemini Flash, selects tools for the task.
  * **Hybrid**: Combine approaches.&#x20;
* Some model providers already offer built-in tool search. For example, [OpenAI](https://developers.openai.com/api/docs/guides/tools-tool-search) and [Anthropic](https://platform.claude.com/docs/en/agents-and-tools/tool-use/tool-search-tool) support this natively



**Using Progressive Discovery:**

1. The host exposes a small set of meta-tools for searching available capabilities. A `search_tools` tool accepts a natural-language query and returns matching tool names with brief descriptions.
2. Fetches the full definition (input schema, output schema, documentation) for that tool only
3. The model calls the tool with full knowledge of its interface



**Dynamic Server Management:**

* Rather than connecting to every configured server at startup, a host can

1. Maintain a registry of available servers and their high-level descriptions.
2. Connect to a server only when the model determines it needs that server’s capabilities.
3. Disconnect servers that are no longer relevant to the current task, freeing context.



**Programmatic tool calling:**&#x20;

* Controls how tools are invoked
* When a task requires chaining multiple tools (read a document, transform it, write it somewhere else), each intermediate result passes through the model, consuming tokens and adding latency even when it has nothing to do with them.
* Instead of calling tools directly, the model writes code that calls tools. The code executes in a sandboxed environment, and only the final result returns to the model.

