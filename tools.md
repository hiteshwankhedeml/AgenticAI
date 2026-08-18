# Tools

* Tools extend what [agents](https://docs.langchain.com/oss/python/langchain/agents) can do—letting them fetch real-time data, execute code, query external databases, and take actions in the world.
* They are just callable function with @tool decorator
* The function’s docstring becomes the tool’s description that helps the model understand when to use it
* Type hints are required as they define the tool’s input schema.
* @tool("calculator", description="Performs arithmetic calc ⇒ To give custom name and description
* config, runtime ⇒ cannot be used in parameters in tool ⇒ It will give when the tool gets called by agent
*

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
