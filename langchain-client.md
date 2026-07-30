# 🟢 LangChain Client

* <mark style="color:purple;background-color:purple;">**Tools of MCP get converted into langchain tools**</mark>
* <mark style="color:purple;background-color:purple;">**To create a MCP client we need to configure a json in which we specify transport, command, and url or path of the MCP server**</mark>
*

    ```
    {
        "data_fetch_mcp_stdio":{
            "transport": "stdio",
            "command": os.path.join(venv_path, "Scripts", "python.exe"),
            "args": [str(mcp_server_script)]
        }
    ```

```python
from langchain_mcp_adapters.client import MultiServerMCPClient
import asyncio
import os 


# Path to the MCP server script
mcp_server_script = os.path.join((os.path.dirname(os.path.abspath(__file__))),"1_first_mcpserver_stdio.py")

# Path to the virtual environment
venv_path = os.path.join((os.path.dirname(os.path.dirname(os.path.abspath(__file__)))),".venv")


async def main():

    # Create an instance of the MultiServerMCPClient
    client = MultiServerMCPClient(

    # MCP Server Config (JSON)
    {
        "data_fetch_mcp_stdio":{
            "transport": "stdio",
            "command": os.path.join(venv_path, "Scripts", "python.exe"),
            "args": [str(mcp_server_script)]
        }

    }
    )

    # List the tools
    tools = await client.get_tools()
    print("Available tools:", tools)

if __name__ == "__main__":
    asyncio.run(main())
```

