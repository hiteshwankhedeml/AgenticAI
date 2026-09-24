# 🟢 Build an MCP Server

<mark style="color:purple;background-color:purple;">**Logging:**</mark>

* <mark style="color:purple;background-color:purple;">**Never write to stdout.**</mark>&#x20;
* <mark style="color:purple;background-color:purple;">**Writing to stdout will corrupt the JSON-RPC messages and break your server.**</mark>&#x20;
* <mark style="color:purple;background-color:purple;">**The**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`print()`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**function writes to stdout by default, so keep it out of a STDIO server entirely.**</mark>
* <mark style="color:purple;background-color:purple;">**For http server stdout logging is fine**</mark>
* <mark style="color:purple;background-color:purple;">**Use the standard library**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`logging`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**module, which writes to stderr.**</mark>

```python
import logging

logger = logging.getLogger(__name__)

# ❌ Bad (STDIO)
print("Processing request")

# ✅ Good (STDIO)
logger.info("Processing request")  # writes to stderr
```

* <mark style="color:purple;background-color:purple;">**Project Setup:**</mark>

```python
# Create a new directory for our project
uv init weather
cd weather

# Create virtual environment and activate it
uv venv
source .venv/bin/activate

# Install dependencies
uv add "mcp[cli]"

# Create our server file
touch weather.py
```

* <mark style="color:purple;background-color:purple;">**To create tools:**</mark>
  * <mark style="color:purple;background-color:purple;">**@mcp.tool()**</mark>    \ <mark style="color:purple;background-color:purple;">**async def get\_alerts(state: str) -> str:**</mark>
* <mark style="color:purple;background-color:purple;">**Run the server:**</mark>
  * <mark style="color:purple;background-color:purple;">**if name == "main":**</mark>&#x20;
  * <mark style="color:purple;background-color:purple;">**mcp.run(transport="stdio")**</mark>
  * <mark style="color:purple;background-color:purple;">**uv run weather.py**</mark>
* <mark style="color:purple;background-color:purple;">**Add in mcp config json to check**</mark>

```python
{
  "mcpServers": {
    "weather": {
      "command": "uv",
      "args": [
        "--directory",
        "/ABSOLUTE/PATH/TO/PARENT/FOLDER/weather",
        "run",
        "weather.py"
      ]
    }
  }
}
```





