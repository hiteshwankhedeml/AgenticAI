# 🟢 Middleware

* <mark style="color:purple;background-color:purple;">**A way to more tightly control what happens inside the agent**</mark>
* The same concept is known as callback in google adk
* Also called as hooks
* Tracking agent behavior with logging, analytics, and debugging.
* Transforming prompts, [tool selection](https://docs.langchain.com/oss/python/langchain/middleware/built-in#llm-tool-selector), and output formatting.
* Adding [retries](https://docs.langchain.com/oss/python/langchain/middleware/built-in#tool-retry), [fallbacks](https://docs.langchain.com/oss/python/langchain/middleware/built-in#model-fallback), and early termination logic.
* Applying [rate limits](https://docs.langchain.com/oss/python/langchain/middleware/built-in#model-call-limit), guardrails, and [PII detection](https://docs.langchain.com/oss/python/langchain/middleware/built-in#pii-detection).
* <mark style="color:purple;background-color:purple;">**Add middleware by passing them to**</mark> [<mark style="color:purple;background-color:purple;">**`create_agent`**</mark>](https://reference.langchain.com/python/langchain/agents/factory/create_agent)<mark style="color:purple;background-color:purple;">**:**</mark>
* <mark style="color:purple;background-color:purple;">**Middleware exposes hooks before and after each of the steps**</mark>
*

    <figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>
