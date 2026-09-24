# 🟢 Context Engineering

<mark style="color:purple;background-color:purple;">**Why Agents Fails?**</mark>

1. <mark style="color:purple;background-color:purple;">**The underlying LLM is not capable enough**</mark>
2. <mark style="color:purple;background-color:purple;">**The “right” context was not passed to the LLM — Mostly because of this**</mark>



* <mark style="color:purple;background-color:purple;">**Context engineering is providing the right information and tools in the right format so the LLM can accomplish a task.**</mark>
* [<mark style="color:purple;background-color:purple;">**Middleware**</mark>](https://docs.langchain.com/oss/python/langchain/middleware) <mark style="color:purple;background-color:purple;">**is the mechanism under the hood that makes context engineering practical**</mark>
* <mark style="color:purple;background-color:purple;">**Middleware allows you to hook into any step in the agent lifecycle and Update context and Jump to a different step in the agent lifecycle**</mark>



<mark style="color:purple;background-color:purple;">**What can be controlled:**</mark>

*

    <table><thead><tr><th width="198.3333740234375">Context Type</th><th width="346.9998779296875">What You Control</th><th width="203">Transient or Persistent</th></tr></thead><tbody><tr><td><a href="https://docs.langchain.com/oss/python/langchain/context-engineering#model-context"><strong>Model Context</strong></a></td><td><mark style="color:purple;background-color:purple;"><strong>What goes into model calls (instructions, message history, tools, response format)</strong></mark></td><td><mark style="color:purple;background-color:purple;"><strong>Transient</strong></mark></td></tr><tr><td><a href="https://docs.langchain.com/oss/python/langchain/context-engineering#tool-context"><strong>Tool Context</strong></a></td><td><mark style="color:purple;background-color:purple;"><strong>What tools can access and produce (reads/writes to state, store, runtime context)</strong></mark></td><td><mark style="color:purple;background-color:purple;"><strong>Persistent</strong></mark></td></tr><tr><td><a href="https://docs.langchain.com/oss/python/langchain/context-engineering#life-cycle-context"><strong>Life-cycle Context</strong></a></td><td><mark style="color:purple;background-color:purple;"><strong>What happens between model and tool calls (summarization, guardrails, logging, etc.)</strong></mark></td><td><mark style="color:purple;background-color:purple;"><strong>Persistent</strong></mark></td></tr></tbody></table>
* <mark style="color:purple;background-color:purple;">**System Prompt:**</mark>
  * <mark style="color:purple;background-color:purple;">**We can create dynamic prompt middleware by accessing values from state, store or context**</mark>
* <mark style="color:purple;background-color:purple;">**Messages:**</mark>
  * <mark style="color:purple;background-color:purple;">**Using @wrap\_model\_call we can Inject context from State when relevant to current query**</mark>
  * <mark style="color:purple;background-color:purple;">**We can also use data from Store**</mark>
* <mark style="color:purple;background-color:purple;">**Tools:**</mark>
  * <mark style="color:purple;background-color:purple;">**Dynamic tool selection adapts the available toolset based on authentication state, user permissions, feature flags, or conversation stage**</mark>
  * <mark style="color:purple;background-color:purple;">**Enable advanced tools only after certain conversation milestones ⇒ This we can checked using State**</mark>
  * <mark style="color:purple;background-color:purple;">**We can also check preferences using Store**</mark>
  * <mark style="color:purple;background-color:purple;">**From context also we can get the context for filtering the tools**</mark>
* <mark style="color:purple;background-color:purple;">**Model:**</mark>
  * <mark style="color:purple;background-color:purple;">**Use dynamic model selection based on conversation length or context of the user logged in etc.**</mark>
* <mark style="color:purple;background-color:purple;">**Response Format:**</mark>
  * <mark style="color:purple;background-color:purple;">**When you provide a schema as the response format, the model’s final response is guaranteed to conform to that schema.**</mark>
  * <mark style="color:purple;background-color:purple;">**Using @wrap\_model\_call we can overwrite the response format**</mark>
* <mark style="color:purple;background-color:purple;">**Tools Context:**</mark>
  * <mark style="color:purple;background-color:purple;">**Tools are special in that they both read and write context.**</mark>
  * <mark style="color:purple;background-color:purple;">**It can read from state/store/context**</mark>
  * <mark style="color:purple;background-color:purple;">**It can also write to State/store**</mark>



<mark style="color:purple;background-color:purple;">**Best Practices:**</mark>

* <mark style="color:purple;background-color:purple;">**Begin with static prompts and tools, add dynamics only when needed**</mark>
* <mark style="color:purple;background-color:purple;">**Add one context engineering feature at a time**</mark>
* <mark style="color:purple;background-color:purple;">**Track model calls, token usage, and latency**</mark>
* <mark style="color:purple;background-color:purple;">**Leverage**</mark> [<mark style="color:purple;background-color:purple;">**`SummarizationMiddleware`**</mark>](https://docs.langchain.com/oss/python/langchain/middleware#summarization)<mark style="color:purple;background-color:purple;">**,**</mark> [<mark style="color:purple;background-color:purple;">**`LLMToolSelectorMiddleware`**</mark>](https://docs.langchain.com/oss/python/langchain/middleware#llm-tool-selector)<mark style="color:purple;background-color:purple;">**, etc.**</mark>





