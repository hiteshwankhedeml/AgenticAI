# Guardrails

* Validating and filtering content at key points in your agent’s execution
* There are multiple ways to implement it, middleware is one of the way of doing it
* They can detect sensitive information, enforce content policies, validate outputs, and prevent unsafe behaviors before they cause problems.
* Usecase:
  * Preventing PII leakage
  * Detecting and blocking prompt injection attacks
  * Blocking inappropriate or harmful content
  * Enforcing business rules and compliance requirements
  * Validating output quality and accuracy
* Approaches to implement:
  * Deterministic:
    * Use rule-based logic like regex patterns, keyword matching, or explicit checks.&#x20;
    * Fast, predictable, and cost-effective, but may miss nuanced violations.
  * Model based guardrails:
    * Use LLMs or classifiers to evaluate content with semantic understanding.&#x20;
    * Catch subtle issues that rules miss, but are slower and more expensive.
* Built in guardrails: PII and HITL
  * Detect common PII types like emails, credit cards, IP addresses, and more
  * Strategy:
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

**Custom Guardrails:**

* Custom middleware that runs before or after the agent executes
* Before Agent:
  * To validate requests once at the start of each invocation.&#x20;
  * This is useful for session-level checks like authentication, rate limiting, or blocking inappropriate requests before any processing begins.
* After Agent:
  * To validate final outputs once before returning to the user
* You can stack multiple guardrails by adding them to the middleware array.
