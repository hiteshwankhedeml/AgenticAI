# 🟢 Runtime

* <mark style="color:purple;background-color:purple;">**LangChain’s**</mark> [<mark style="color:purple;background-color:purple;">**`create_agent`**</mark>](https://reference.langchain.com/python/langchain/agents/factory/create_agent) <mark style="color:purple;background-color:purple;">**runs on LangGraph’s runtime under the hood.**</mark>
* <mark style="color:purple;background-color:purple;">**Runtime has below info:**</mark>
  * <mark style="color:purple;background-color:purple;">**Context:**</mark>&#x20;
    * <mark style="color:purple;background-color:purple;">**static information like user id, db connections**</mark>
    * <mark style="color:purple;background-color:purple;">**When invoking the agent, pass the**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`context`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**argument with the relevant configuration for the run**</mark>

```python
from dataclasses import dataclass

from langchain.agents import create_agent


@dataclass
class Context:
    user_name: str

agent = create_agent(
    model="gpt-5-nano",
    tools=[...],
    context_schema=Context  
)

agent.invoke(
    {"messages": [{"role": "user", "content": "What's my name?"}]},
    context=Context(user_name="John Smith")
)

# Using context inside tools
from dataclasses import dataclass
from langchain.tools import tool, ToolRuntime  

@dataclass
class Context:
    user_id: str

@tool
def fetch_user_email_preferences(runtime: ToolRuntime[Context]) -> str:
    """Fetch the user's email preferences from the store."""
    user_id = runtime.context.user_id  

    preferences: str = "The user prefers you to write a brief and polite email."
    if runtime.store:
        if memory := runtime.store.get(("users",), user_id):
            preferences = memory.value["preferences"]

    return preferences
```

*
  * <mark style="color:purple;background-color:purple;">**Store: a**</mark> [<mark style="color:purple;background-color:purple;">**BaseStore**</mark>](https://reference.langchain.com/python/langchain-core/stores/BaseStore) <mark style="color:purple;background-color:purple;">**instance used for**</mark> [<mark style="color:purple;background-color:purple;">**long-term memory**</mark>](https://docs.langchain.com/oss/python/langchain/long-term-memory)
  * <mark style="color:purple;background-color:purple;">**Stream writer: an object used for streaming information via the**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`"custom"`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**stream mode**</mark>
  * <mark style="color:purple;background-color:purple;">**Exeuction info: identity and retry information for the current execution (thread ID, run ID, attempt number)**</mark>
  * <mark style="color:purple;background-color:purple;">**Server info: server-specific metadata when running on LangGraph Server**</mark>

<mark style="color:purple;background-color:purple;">**Inside Tool:**</mark>

* <mark style="color:purple;background-color:purple;">**Access the context**</mark>
* <mark style="color:purple;background-color:purple;">**Read or write long-term memory**</mark>

```python
from dataclasses import dataclass
from langchain.tools import tool, ToolRuntime  

@dataclass
class Context:
    user_id: str

@tool
def fetch_user_email_preferences(runtime: ToolRuntime[Context]) -> str:
    """Fetch the user's email preferences from the store."""
    user_id = runtime.context.user_id  

    preferences: str = "The user prefers you to write a brief and polite email."
    if runtime.store:
        if memory := runtime.store.get(("users",), user_id):
            preferences = memory.value["preferences"]

    return preferences
```

<mark style="color:purple;background-color:purple;">**Inside Middleware:**</mark>

* <mark style="color:purple;background-color:purple;">**You can access runtime information in middleware to create dynamic prompts, modify messages, or control agent behavior based on user context.**</mark>

```python
from dataclasses import dataclass

from langchain.messages import AnyMessage
from langchain.agents import create_agent, AgentState
from langchain.agents.middleware import dynamic_prompt, ModelRequest, before_model, after_model
from langgraph.runtime import Runtime


@dataclass
class Context:
    user_name: str

# Dynamic prompts
@dynamic_prompt
def dynamic_system_prompt(request: ModelRequest) -> str:
    user_name = request.runtime.context.user_name  
    system_prompt = f"You are a helpful assistant. Address the user as {user_name}."
    return system_prompt

# Before model hook
@before_model
def log_before_model(state: AgentState, runtime: Runtime[Context]) -> dict | None:
    print(f"Processing request for user: {runtime.context.user_name}")
    return None

# After model hook
@after_model
def log_after_model(state: AgentState, runtime: Runtime[Context]) -> dict | None:
    print(f"Completed request for user: {runtime.context.user_name}")
    return None

agent = create_agent(
    model="gpt-5-nano",
    tools=[...],
    middleware=[dynamic_system_prompt, log_before_model, log_after_model],
    context_schema=Context
)

agent.invoke(
    {"messages": [{"role": "user", "content": "What's my name?"}]},
    context=Context(user_name="John Smith")
)
```
