# Runtime

* LangChain’s [`create_agent`](https://reference.langchain.com/python/langchain/agents/factory/create_agent) runs on LangGraph’s runtime under the hood.
* Runtime has below info:
  * Context:&#x20;
    * static information like user id, db connections
    * When invoking the agent, pass the `context` argument with the relevant configuration for the run

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
  * Store: a [BaseStore](https://reference.langchain.com/python/langchain-core/stores/BaseStore) instance used for [long-term memory](https://docs.langchain.com/oss/python/langchain/long-term-memory)
  * Stream writer: an object used for streaming information via the `"custom"` stream mode
  * Exeuction info: identity and retry information for the current execution (thread ID, run ID, attempt number)
  * Server info: server-specific metadata when running on LangGraph Server
*
