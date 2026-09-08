# MCP Architecture

* Client server architecture

**Participants:**

* **MCP Host**: The AI application that coordinates and manages one or multiple MCP clients
* **MCP Client**: A component that maintains a connection to an MCP server and obtains context from an MCP server for the MCP host to use
* **MCP Server**:&#x20;
  * A program that provides context to MCP clients
  * It provides Tools, Resources and Prompts
  * Resources like file content, documentation

**Example:**

* Visual Studio Code acts as an MCP host.&#x20;
* When Visual Studio Code establishes a connection to an MCP server, the Visual Studio Code runtime instantiates an MCP client object that maintains the connection to the Sentry MCP server.
* When Visual Studio Code subsequently connects to another MCP server the Visual Studio Code runtime instantiates an additional MCP client object to maintain this connection.

**Layers:**

* **Data layer**:&#x20;
  * Defines the JSON-RPC based protocol for client-server communication, including capability and version discovery, and core primitives, such as tools, resources, prompts and notifications.
* **Transport layer**:&#x20;
  * Defines the communication mechanisms and channels that enable data exchange between clients and servers, including transport-specific connection establishment, message framing, and authorization.
  * Two transport mechanism:
  * **Stdio transport**:&#x20;
    * Uses standard input/output streams for direct process communication between local processes on the same machine, providing optimal performance with no network overhead.
  * **Streamable HTTP transport**:&#x20;
    * Uses HTTP POST for client-to-server messages with optional Server-Sent Events for streaming capabilities.&#x20;
    * This transport enables remote server communication and supports standard HTTP authentication methods including bearer tokens, API keys, and custom headers.&#x20;
    * MCP recommends using OAuth to obtain authentication tokens.
