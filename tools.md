# Tools

* Tools extend what [agents](https://docs.langchain.com/oss/python/langchain/agents) can do—letting them fetch real-time data, execute code, query external databases, and take actions in the world.
* They are just callable function with @tool decorator
* The function’s docstring becomes the tool’s description that helps the model understand when to use it
* Type hints are required as they define the tool’s input schema.
* @tool("calculator", description="Performs arithmetic calc ⇒ To give custom name and description
* config, runtime ⇒ cannot be used in parameters in tool ⇒ It will give when the tool gets called by agent
* Tools are most powerful when they can access runtime information like conversation history, user data, and persistent memory ⇒ This can be access using ToolRunTime which has below fields
  *

      | Component          | Description                                                                                                                 | Use case                                                                                                                                                |
      | ------------------ | --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------- |
      | **State**          | Short-term memory                                                                                                           | It includes the message history and any custom fields you define in your [graph state](https://docs.langchain.com/oss/python/langgraph/graph-api#state) |
      | **Context**        | Immutable configuration passed at invocation time (user IDs, session info)                                                  | Personalize responses based on user identity                                                                                                            |
      | **Store**          | Long-term memory - persistent data that survives across conversations                                                       | Save user preferences, maintain knowledge base                                                                                                          |
      | **Stream Writer**  | Emit real-time updates during tool execution                                                                                | Show progress for long-running operations                                                                                                               |
      | **Execution Info** | Identity and retry information for the current execution (thread ID, run ID, attempt number)                                | Access thread/run IDs, adjust behavior based on retry state                                                                                             |
      | **Server Info**    | Server-specific metadata when running on LangGraph Server (assistant ID, graph ID, authenticated user)                      | Access assistant ID, graph ID, or authenticated user info                                                                                               |
      | **Config**         | [`RunnableConfig`](https://reference.langchain.com/python/langchain-core/runnables/config/RunnableConfig) for the execution | Access callbacks, tags, and metadata                                                                                                                    |
      | **Tool Call ID**   | Unique identifier for the current tool invocation                                                                           | Correlate tool calls for logs and model invocations                                                                                                     |

```python
from langchain.tools import tool

@tool
def search_database(query: str, limit: int = 10) -> str:
    """Search the customer database for records matching the query.

    Args:
        query: Search terms to look for
        limit: Maximum number of results to return
    """
    return f"Found {limit} results for '{query}'"
```

```python
from pydantic import BaseModel, Field
from typing import Literal

class WeatherInput(BaseModel):
    """Input for weather queries."""
    location: str = Field(description="City name or coordinates")
    units: Literal["celsius", "fahrenheit"] = Field(
        default="celsius",
        description="Temperature unit preference"
    )
    include_forecast: bool = Field(
        default=False,
        description="Include 5-day forecast"
    )

@tool(args_schema=WeatherInput)
def get_weather(location: str, units: str = "celsius", include_forecast: bool = False) -> str:
    """Get current weather and optional forecast."""
    temp = 22 if units == "celsius" else 72
    result = f"Current weather in {location}: {temp} degrees {units[0].upper()}"
    if include_forecast:
        result += "\nNext 5 days: Sunny"
    return result
```

**Headless Tools:**

* Use them when the work depends on the **environment, device, or UI** that only exists on the client.
* A headless tool is schema-only, with no in-process implementation.
* Instead of running locally, the graph pauses
* **Resume** the graph after your app, another service, or a human step performs the action.
