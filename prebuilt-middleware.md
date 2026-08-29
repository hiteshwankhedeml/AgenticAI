# Prebuilt Middleware

* LangChain and [Deep Agents](https://docs.langchain.com/oss/python/deepagents/overview) provide prebuilt middleware for common use cases
* Below work with all LLM

**ToolError:**

* Catch exceptions raised during tool execution and convert them into error `ToolMessage`s that the model can see and recover from and retry with corrected arguments
* We define a function on\_error inside which we format the error message and this function is passed inside ToolErrorMiddleware to the LLM

```python
def on_error(exc: Exception, request: ToolCallRequest) -> str | None:
    if isinstance(exc, ValueError):
        return f"`{request.tool_call['name']}` failed with {type(exc).__name__}."

agent = create_agent(
    model="gpt-5.5",
    tools=[your_tools],
    middleware=[ToolErrorMiddleware(on_error)],
)
```

**Tool retry:**

* Automatically retry failed tool calls with configurable exponential backoff incase of network errors
* middleware=\[  &#x20;ToolRetryMiddleware(  &#x20;max\_retries=3,  &#x20;backoff\_factor=2.0,  &#x20;initial\_delay=1.0,  &#x20;),  &#x20;]

**Model retry: R**etry failed model calls

**Model Fallback:**&#x20;

* Automatically fallback to alternative models when the primary model fails
* middleware=\[  &#x20;ModelFallbackMiddleware(  &#x20;"gpt-5.4-mini",  &#x20;"claude-3-5-sonnet-20241022",  ),  ]

**Summarization:**&#x20;

* Automatically summarize conversation history when approaching token limits, preserving recent messages while compressing older context
* Recent messages retained by `keep` still include their original multimodal blocks, while older multimodal messages that are summarized are represented only by the generated text summary.&#x20;
* middleware=\[  &#x20;SummarizationMiddleware(  &#x20;model="gpt-5.4-mini",  &#x20;trigger=("tokens", 4000),  \
  keep=("messages", 20),  &#x20;)

**Human in the loop:**

* requires a [checkpointer](https://docs.langchain.com/oss/python/langgraph/checkpointers#checkpoints) to maintain state across interruptions.

```python
from langchain.agents import create_agent
from langchain.agents.middleware import HumanInTheLoopMiddleware
from langgraph.checkpoint.memory import InMemorySaver


def your_read_email_tool(email_id: str) -> str:
    """Mock function to read an email by its ID."""
    return f"Email content for ID: {email_id}"

def your_send_email_tool(recipient: str, subject: str, body: str) -> str:
    """Mock function to send an email."""
    return f"Email sent to {recipient} with subject '{subject}'"

agent = create_agent(
    model="gpt-5.5",
    tools=[your_read_email_tool, your_send_email_tool],
    checkpointer=InMemorySaver(),
    middleware=[
        HumanInTheLoopMiddleware(
            interrupt_on={
                "your_send_email_tool": {
                    "allowed_decisions": ["approve", "edit", "reject"],
                },
                "your_read_email_tool": False,
            }
        ),
    ],
)
```

**Model Call Limit**

**Tool Call Limit:**

* Either globally across all tools or for specific tools

**PII Detection:**

* There are lot of PII types which are available by default&#x20;
* For custom we can either write a regex or also create detector function in which we will pass the content and it will return the detections

```python
from langchain.agents import create_agent
from langchain.agents.middleware import PIIMiddleware

agent = create_agent(
    model="gpt-5.5",
    tools=[],
    middleware=[
        PIIMiddleware("email", strategy="redact", apply_to_input=True),
        PIIMiddleware("credit_card", strategy="mask", apply_to_input=True),
    ],
)

agent2 = create_agent(
    model="gpt-5.5",
    tools=[],
    middleware=[
        PIIMiddleware(
            "phone_number",
            detector=re.compile(r"\+?\d{1,3}[\s.-]?\d{3,4}[\s.-]?\d{4}"),
            strategy="mask",
        ),
    ],
)

# Method 3: Custom detector function
def detect_ssn(content: str) -> list[PIIMatch]:
    """Detect SSNs with validation."""
    matches: list[PIIMatch] = []
    pattern = r"\d{3}-\d{2}-\d{4}"
    for match in re.finditer(pattern, content):
        ssn = match.group(0)
        # Validate: first 3 digits shouldn't be 000, 666, or 900-999
        first_three = int(ssn[:3])
        if first_three not in [0, 666] and not (900 <= first_three <= 999):
            matches.append({
                "type": "ssn",
                "value": ssn,
                "start": match.start(),
                "end": match.end(),
            })
    return matches

```

**LLM Tool Selector:**

* Select relevant tools before calling the main model
* Agents with many tools (10+) where most aren’t relevant per query.
* Reducing token usage by filtering irrelevant tools.
* LLMToolSelectorMiddleware(  &#x20;model="gpt-5.4-mini",  &#x20;max\_tools=3,  &#x20;always\_include=\["search"],  &#x20;)

**Provider Tool Search:**

* Defer selected tools behind model providers’ server-side tool search, so the model discovers them on demand instead of receiving every tool schema up front
* Requires a model with server-side tool search support: Anthropic (Claude Sonnet 4+/Opus 4+/Haiku 4.5+) or OpenAI (gpt-5.5+).
* ProviderToolSearchMiddleware(searchable\_tools=\["lookup\_order"])

**Shell Tool:** Expose a persistent shell session to agents for command execution

