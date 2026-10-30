# MCP Inspector

* For testing and debugging [MCP servers](https://modelcontextprotocol.io/docs/2026-07-28/learn/server-concepts).&#x20;
* It ships as a single package, `@modelcontextprotocol/inspector`, providing **three clients**
*

    | Client  | Invocation                                  | What it’s for                                                                     |
    | ------- | ------------------------------------------- | --------------------------------------------------------------------------------- |
    | **Web** | `npx @modelcontextprotocol/inspector`       | A full graphical inspector in the browser. The default, and the richest surface.  |
    | **CLI** | `npx @modelcontextprotocol/inspector --cli` | A scriptable, machine-readable client for CI, shell pipelines, and coding agents. |
    | **TUI** | `npx @modelcontextprotocol/inspector --tui` | An interactive terminal UI, for when a browser isn’t available or wanted.         |
* The package also installs **`mcpdo`**, an experimental [connection client](https://modelcontextprotocol.io/docs/2026-07-28/tools/inspector/mcpdo) that connects to a server once and keeps it open across many commands, on the same core
* Requires **Node 22.19.0 or newer**

```cmd
# Launch the web UI and connect to a local stdio server
npx @modelcontextprotocol/inspector node path/to/server/index.js

# Or launch with no target and add servers from the UI
npx @modelcontextprotocol/inspector
```

* For inspecting published servers we can use pass url&#x20;

```
npx @modelcontextprotocol/inspector --server-url https://api.example.com/mcp --transport http
```

*
