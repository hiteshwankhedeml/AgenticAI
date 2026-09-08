# 🟢 Guardrails

* <mark style="color:purple;background-color:purple;">**Validating and filtering content at key points in your agent’s execution**</mark>
* <mark style="color:purple;background-color:purple;">**There are multiple ways to implement it, middleware is one of the way of doing it**</mark>
* <mark style="color:purple;background-color:purple;">**They can detect sensitive information, enforce content policies, validate outputs, and prevent unsafe behaviors before they cause problems.**</mark>
* <mark style="color:purple;background-color:purple;">**Usecase:**</mark>
  * <mark style="color:purple;background-color:purple;">**Preventing PII leakage**</mark>
  * <mark style="color:purple;background-color:purple;">**Detecting and blocking prompt injection attacks**</mark>
  * <mark style="color:purple;background-color:purple;">**Blocking inappropriate or harmful content**</mark>
  * <mark style="color:purple;background-color:purple;">**Enforcing business rules and compliance requirements**</mark>
  * <mark style="color:purple;background-color:purple;">**Validating output quality and accuracy**</mark>
* <mark style="color:purple;background-color:purple;">**Approaches to implement:**</mark>
  * <mark style="color:purple;background-color:purple;">**Deterministic:**</mark>
    * <mark style="color:purple;background-color:purple;">**Use rule-based logic like regex patterns, keyword matching, or explicit checks.**</mark>&#x20;
    * <mark style="color:purple;background-color:purple;">**Fast, predictable, and cost-effective, but may miss nuanced violations.**</mark>
  * <mark style="color:purple;background-color:purple;">**Model based guardrails:**</mark>
    * <mark style="color:purple;background-color:purple;">**Use LLMs or classifiers to evaluate content with semantic understanding.**</mark>&#x20;
    * <mark style="color:purple;background-color:purple;">**Catch subtle issues that rules miss, but are slower and more expensive.**</mark>
* <mark style="color:purple;background-color:purple;">**Built in guardrails: PII and HITL**</mark>
  * <mark style="color:purple;background-color:purple;">**Detect common PII types like emails, credit cards, IP addresses, and more**</mark>
  * <mark style="color:purple;background-color:purple;">**Strategy:**</mark>
  *

      | Strategy | Description                             | Example               |
      | -------- | --------------------------------------- | --------------------- |
      | `redact` | Replace with `[REDACTED_{PII_TYPE}]`    | `[REDACTED_EMAIL]`    |
      | `mask`   | Partially obscure (e.g., last 4 digits) | `****-****-****-1234` |
      | `hash`   | Replace with deterministic hash         | `a8f5f167...`         |
      | `block`  | Raise exception when detected           | Error thrown          |

```python
from langchain.agents import create_agent
from langchain.agents.middleware import PIIMiddleware


agent = create_agent(
    model="gpt-5.5",
    tools=[customer_service_tool, email_tool],
    middleware=[
        # Redact emails in user input before sending to model
        PIIMiddleware(
            "email",
            strategy="redact",
            apply_to_input=True,
        ),
        # Mask credit cards in user input
        PIIMiddleware(
            "credit_card",
            strategy="mask",
            apply_to_input=True,
        ),
        # Block API keys - raise error if detected
        PIIMiddleware(
            "api_key",
            detector=r"sk-[a-zA-Z0-9]{32}",
            strategy="block",
            apply_to_input=True,
        ),
    ],
)

# When user provides PII, it will be handled according to the strategy
result = agent.invoke({
    "messages": [{"role": "user", "content": "My email is john.doe@example.com and card is 5105-1051-0510-5100"}]
})
```

<mark style="color:purple;background-color:purple;">**Custom Guardrails:**</mark>

* <mark style="color:purple;background-color:purple;">**Custom middleware that runs before or after the agent executes**</mark>
* <mark style="color:purple;background-color:purple;">**Before Agent:**</mark>
  * <mark style="color:purple;background-color:purple;">**To validate requests once at the start of each invocation.**</mark>&#x20;
  * <mark style="color:purple;background-color:purple;">**This is useful for session-level checks like authentication, rate limiting, or blocking inappropriate requests before any processing begins.**</mark>
* <mark style="color:purple;background-color:purple;">**After Agent:**</mark>
  * <mark style="color:purple;background-color:purple;">**To validate final outputs once before returning to the user**</mark>
* <mark style="color:purple;background-color:purple;">**You can stack multiple guardrails by adding them to the middleware array.**</mark>
