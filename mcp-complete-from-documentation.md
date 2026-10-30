# MCP - Complete from documentation

* LangChain agents call tools defined on MCP servers through [`MCPAdapter`](https://reference.langchain.com/python/langchain/mcp/adapter/MCPAdapter), which discovers a server’s tools and adapts them into LangChain tools you can pass straight to [`create_agent`](https://reference.langchain.com/python/langchain/agents/factory/create_agent).
* [`MCPAdapter`](https://reference.langchain.com/python/langchain/mcp/adapter/MCPAdapter) is built on [FastMCP](https://gofastmcp.com/), which handles transport inference, protocol negotiation, connection management, and authentication.
* &#x20;list\_tools() ⇒ discover the server’s tools
* [`MCPAdapter`](https://reference.langchain.com/python/langchain/mcp/adapter/MCPAdapter) infers the transport from the target you provide. ⇒ Whether we provide URL or path

```python
! pip install "langchain[mcp]"

from langchain.agents import create_agent
from langchain.mcp import MCPAdapter


async def run_agent(server) -> dict:
    # Discover the server's tools, then hand them to the agent like any other
    # LangChain tools. The tools hold the client, so the agent stays usable
    # for the life of the adapter context.
    async with MCPAdapter(server) as adapter:
        tools = await adapter.list_tools()
        agent = create_agent("claude-sonnet-5", tools)
        return await agent.ainvoke(
            {
                "messages": [
                    {"role": "user", "content": "What is the forecast for Oslo?"}
                ]
            }
        )
```

* From the agent’s perspective, they behave like LangChain tools: the model chooses a tool, LangChain invokes it, and the resulting [`ToolMessage`](https://reference.langchain.com/python/langchain-core/messages/tool/ToolMessage) returns to the mode
*

    | If you need…                                                | Use                                                            | Go to                                                                                                                   |
    | ----------------------------------------------------------- | -------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------- |
    | One server                                                  | A URL, `Path`, or in-process target                            | [Transports](https://docs.langchain.com/oss/python/langchain/mcp#transports)                                            |
    | Several servers behind one connection                       | An `MCPConfig` dict                                            | [MCPConfig](https://docs.langchain.com/oss/python/langchain/mcp/connections#one-aggregate-connection-with-mcpconfig)    |
    | Several servers with separate auth, protocol eras, or pools | A [`ClientGroup`](https://gofastmcp.com/clients/client-groups) | [ClientGroup](https://docs.langchain.com/oss/python/langchain/mcp/connections#independent-connections-with-clientgroup) |
*

    | Situation                                           | Pattern                                                                                                                                                                                                              | Go to                                                                                                                    |
    | --------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
    | Script, notebook, or most agents                    | Discover inside `async with`, then exit                                                                                                                                                                              | [Connection lifecycle](https://docs.langchain.com/oss/python/langchain/mcp/connections#connection-lifecycle)             |
    | Hold one session across several tool calls in a run | Keep the adapter open around the agent call                                                                                                                                                                          | [One session per invocation](https://docs.langchain.com/oss/python/langchain/mcp/connections#one-session-per-invocation) |
    | Many concurrent runs in a deployment                | Discover per run; reuse a [shared pool](https://docs.langchain.com/oss/python/langchain/mcp/connections#shared-connection-pool) and [cache](https://docs.langchain.com/oss/python/langchain/mcp/connections#caching) | [Scale a deployment](https://docs.langchain.com/oss/python/langchain/mcp/connections#scale-a-deployment)                 |
*
