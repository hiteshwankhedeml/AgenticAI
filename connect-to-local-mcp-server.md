# 🟢 Connect to Local MCP Server

* <mark style="color:purple;background-color:purple;">**The Filesystem Server and many other MCP servers require Node.js to run**</mark>
* <mark style="color:purple;background-color:purple;">**node --version ⇒ Required for claude desktop**</mark>
* <mark style="color:purple;background-color:purple;">**Claude Desktop ⇒ Developer Settings ⇒ Edit Config**</mark>

```json
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/Users/username/Desktop",
        "/Users/username/Downloads"
      ]
    }
  }
}
```
