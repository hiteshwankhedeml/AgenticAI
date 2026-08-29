# Middleware

* A way to more tightly control what happens inside the agent
* Tracking agent behavior with logging, analytics, and debugging.
* Transforming prompts, [tool selection](https://docs.langchain.com/oss/python/langchain/middleware/built-in#llm-tool-selector), and output formatting.
* Adding [retries](https://docs.langchain.com/oss/python/langchain/middleware/built-in#tool-retry), [fallbacks](https://docs.langchain.com/oss/python/langchain/middleware/built-in#model-fallback), and early termination logic.
* Applying [rate limits](https://docs.langchain.com/oss/python/langchain/middleware/built-in#model-call-limit), guardrails, and [PII detection](https://docs.langchain.com/oss/python/langchain/middleware/built-in#pii-detection).
* Add middleware by passing them to [`create_agent`](https://reference.langchain.com/python/langchain/agents/factory/create_agent):
* Middleware exposes hooks before and after each of the steps
*

    <figure><img src=".gitbook/assets/image (13).png" alt=""><figcaption></figcaption></figure>
