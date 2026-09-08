# 🟢 Prebuilt Middleware

* <mark style="color:purple;background-color:purple;">**LangChain and**</mark> [<mark style="color:purple;background-color:purple;">**Deep Agents**</mark>](https://docs.langchain.com/oss/python/deepagents/overview) <mark style="color:purple;background-color:purple;">**provide prebuilt middleware for common use cases**</mark>
* <mark style="color:purple;background-color:purple;">**Below work with all LLM**</mark>

<mark style="color:purple;background-color:purple;">**ToolError:**</mark>

* <mark style="color:red;background-color:purple;">**Catch exceptions raised during tool execution and convert them into error**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**`ToolMessage`**</mark><mark style="color:red;background-color:purple;">**s that the model can see and recover from and retry with corrected arguments**</mark>
* <mark style="color:purple;background-color:purple;">**We define a function on\_error inside which we format the error message and this function is passed inside ToolErrorMiddleware to the LLM**</mark>
* <mark style="color:purple;background-color:purple;">**So that the LLM can retry**</mark>&#x20;

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

<mark style="color:purple;background-color:purple;">**Tool retry:**</mark>

* <mark style="color:red;background-color:purple;">**Automatically retry failed tool calls with configurable exponential backoff incase of network errors**</mark>
* <mark style="color:purple;background-color:purple;">**middleware=\[  &#x20;ToolRetryMiddleware(  &#x20;max\_retries=3,  &#x20;backoff\_factor=2.0,  &#x20;initial\_delay=1.0,  &#x20;),  &#x20;]**</mark>

<mark style="color:purple;background-color:purple;">**LLM Tool Emulator:**</mark>

* <mark style="color:red;background-color:purple;">**Emulate tool execution using an LLM for testing purposes, replacing actual tool calls with AI-generated responses**</mark>
* <mark style="color:red;background-color:purple;">**Testing agent behavior without executing real tools.**</mark>
* <mark style="color:red;background-color:purple;">**Developing agents when external tools are unavailable or expensive.**</mark>
* <mark style="color:purple;background-color:purple;">**Prototyping agent workflows before implementing actual tools.**</mark>

```python
from langchain.agents import create_agent
from langchain.agents.middleware import LLMToolEmulator

agent = create_agent(
    model="gpt-5.5",
    tools=[get_weather, search_database, send_email],
    middleware=[
        LLMToolEmulator(),  # Emulate all tools
    ],
)
```

<mark style="color:red;background-color:purple;">**Model retry:**</mark>**&#x20;**<mark style="color:purple;background-color:purple;">**Retry failed model calls**</mark>

<mark style="color:red;background-color:purple;">**Model Fallback:**</mark>&#x20;

* <mark style="color:red;background-color:purple;">**Automatically fallback to alternative models when the primary model fails**</mark>
* <mark style="color:purple;background-color:purple;">**We can have multiple fallback**</mark>
* <mark style="color:purple;background-color:purple;">**middleware=\[  &#x20;ModelFallbackMiddleware(  &#x20;"gpt-5.4-mini",  &#x20;"claude-3-5-sonnet-20241022",  ),  ]**</mark>

<mark style="color:red;background-color:purple;">**Summarization:**</mark>&#x20;

* <mark style="color:red;background-color:purple;">**Automatically summarize conversation history when approaching token limits, preserving recent messages while compressing older context**</mark>
* <mark style="color:red;background-color:purple;">**It needs LLM**</mark>&#x20;
* <mark style="color:purple;background-color:purple;">**We need to specify either the number of tokens or no of messages when summarization needs to be done**</mark>
* <mark style="color:purple;background-color:purple;">**We need to specify no. of token to be kept after summarization and no. of last messages to be retained**</mark>
* <mark style="color:red;background-color:purple;">**Recent messages retained by**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**`keep`**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**still include their original multimodal blocks**</mark><mark style="color:purple;background-color:purple;">**, while older multimodal messages that are summarized are represented only by the generated text summary.**</mark>&#x20;
* <mark style="color:red;background-color:purple;">**We can also pass summary prompt**</mark>

```python
middleware=[ SummarizationMiddleware( model="gpt-5.4-mini", trigger=("tokens", 4000),
keep=("messages", 20), )
```

<mark style="color:purple;background-color:purple;">**Human in the loop:**</mark>

* <mark style="color:red;background-color:purple;">**requires a**</mark> [<mark style="color:red;background-color:purple;">**checkpointer**</mark>](https://docs.langchain.com/oss/python/langgraph/checkpointers#checkpoints) <mark style="color:red;background-color:purple;">**to maintain state across interruptions.**</mark>
* <mark style="color:purple;background-color:purple;">**If we specify False then for that tool it won't interrupt**</mark>
* <mark style="color:red;background-color:purple;">**The decisions for which we want interrupt needs to be specified**</mark>
* <mark style="color:red;background-color:purple;">**The human can have below options:**</mark>
  * <mark style="color:red;background-color:purple;">**Allow**</mark>
  * <mark style="color:red;background-color:purple;">**Reject**</mark>
  * <mark style="color:red;background-color:purple;">**Edit ⇒ Edit the tool arguments before execution**</mark>
  * <mark style="color:red;background-color:purple;">**Respond ⇒ Return the human message directly as a syntehtic tool result**</mark>
* <mark style="color:red;background-color:purple;">**Human input will be passed by using the same config and in invokes**</mark>
* <mark style="color:red;background-color:purple;">**graph.invoke(Command(resume={"decisions": \[{"type": "approve"}]}),config=config)**</mark>

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

<mark style="color:red;background-color:purple;">**Model Call Limit**</mark>

<mark style="color:red;background-color:purple;">**Tool Call Limit:**</mark>

* <mark style="color:red;background-color:purple;">**Either globally across all tools or for specific tools**</mark>

<mark style="color:red;background-color:purple;">**PII Detection:**</mark>

* <mark style="color:red;background-color:purple;">**There are lot of PII types which are available by default**</mark>&#x20;
* <mark style="color:red;background-color:purple;">**For custom we can either write a regex or also create detector function in which we will pass the content and it will return the detections**</mark>

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

<mark style="color:red;background-color:purple;">**To-do list:**</mark>

* <mark style="color:red;background-color:purple;">**Equip agents with task planning and tracking capabilities for complex multi-step tasks.**</mark>

```python
from langchain.agents import create_agent
from langchain.agents.middleware import TodoListMiddleware

agent = create_agent(
    model="gpt-5.5",
    tools=[read_file, write_file, run_tests],
    middleware=[TodoListMiddleware()],
)
```

<mark style="color:purple;background-color:purple;">**LLM Tool Selector:**</mark>

* <mark style="color:red;background-color:purple;">**Select relevant tools before calling the main model**</mark>
* <mark style="color:purple;background-color:purple;">**Agents with many tools (10+) where most aren’t relevant per query.**</mark>
* <mark style="color:purple;background-color:purple;">**Reducing token usage by filtering irrelevant tools.**</mark>
* <mark style="color:red;background-color:purple;">**LLMToolSelectorMiddleware(  &#x20;model="gpt-5.4-mini",  &#x20;max\_tools=3,  &#x20;always\_include=\["search"],  &#x20;)**</mark>

<mark style="color:purple;background-color:purple;">**Provider Tool Search:**</mark>

* <mark style="color:purple;background-color:purple;">**Defer selected tools behind model providers’ server-side tool search, so the model discovers them on demand instead of receiving every tool schema up front**</mark>
* <mark style="color:purple;background-color:purple;">**Requires a model with server-side tool search support: Anthropic (Claude Sonnet 4+/Opus 4+/Haiku 4.5+) or OpenAI (gpt-5.5+).**</mark>
* <mark style="color:purple;background-color:purple;">**ProviderToolSearchMiddleware(searchable\_tools=\["lookup\_order"])**</mark>

<mark style="color:purple;background-color:purple;">**Shell Tool:**</mark>&#x20;

* <mark style="color:red;background-color:purple;">**Expose a persistent shell session to agents for command execution**</mark>
* <mark style="color:red;background-color:purple;">**Agents that need to execute system commands**</mark>
* <mark style="color:purple;background-color:purple;">**Development and deployment automation tasks**</mark>
* <mark style="color:purple;background-color:purple;">**Testing and validation workflows**</mark>
* <mark style="color:purple;background-color:purple;">**File system operations and script execution**</mark>
* <mark style="color:red;background-color:purple;">**Use appropriate execution policies (**</mark><mark style="color:red;background-color:purple;">**`HostExecutionPolicy`**</mark><mark style="color:red;background-color:purple;">**,**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**`DockerExecutionPolicy`**</mark><mark style="color:red;background-color:purple;">**, or**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**`CodexSandboxExecutionPolicy`**</mark><mark style="color:red;background-color:purple;">**) to match your deployment’s security requirements.**</mark>
* <mark style="color:purple;background-color:purple;">**Persistent shell sessions do not currently work with interrupts (human-in-the-loop). We anticipate adding support for this in the future.**</mark>
* <mark style="color:purple;background-color:purple;">**We can treat it as a tool, which is called by AI, it will access the file system where it's running**</mark>

```python
from langchain.agents import create_agent
from langchain.agents.middleware import (
    ShellToolMiddleware,
    HostExecutionPolicy,
)

agent = create_agent(
    model="gpt-5.5",
    tools=[search_tool],
    middleware=[
        ShellToolMiddleware(
            workspace_root="/workspace",
            execution_policy=HostExecutionPolicy(),
        ),
    ],
)
```

