---
hidden: true
---

# Context Engineering

**Why Agents Fails?**

1. The underlying LLM is not capable enough
2. The “right” context was not passed to the LLM — Mostly because of this



* Context engineering is providing the right information and tools in the right format so the LLM can accomplish a task.
* [Middleware](https://docs.langchain.com/oss/python/langchain/middleware) is the mechanism under the hood that makes context engineering practical
* Middleware allows you to hook into any step in the agent lifecycle and Update context and Jump to a different step in the agent lifecycle



**What can be controlled:**

*

    <table><thead><tr><th width="198.3333740234375">Context Type</th><th width="346.9998779296875">What You Control</th><th width="203">Transient or Persistent</th></tr></thead><tbody><tr><td><a href="https://docs.langchain.com/oss/python/langchain/context-engineering#model-context"><strong>Model Context</strong></a></td><td>What goes into model calls (instructions, message history, tools, response format)</td><td>Transient</td></tr><tr><td><a href="https://docs.langchain.com/oss/python/langchain/context-engineering#tool-context"><strong>Tool Context</strong></a></td><td>What tools can access and produce (reads/writes to state, store, runtime context)</td><td>Persistent</td></tr><tr><td><a href="https://docs.langchain.com/oss/python/langchain/context-engineering#life-cycle-context"><strong>Life-cycle Context</strong></a></td><td>What happens between model and tool calls (summarization, guardrails, logging, etc.)</td><td>Persistent</td></tr></tbody></table>
* **System Prompt:**
  * We can create dynamic prompt middleware by accessing values from state, store or context
*
  *



