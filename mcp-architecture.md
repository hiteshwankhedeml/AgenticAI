# 🟢 MCP Architecture

* <mark style="color:purple;background-color:purple;">Client server architecture</mark>

<mark style="color:purple;background-color:purple;">**Participants:**</mark>

* <mark style="color:red;background-color:purple;">**MCP Host: The AI application**</mark> <mark style="color:purple;background-color:purple;">that coordinates and manages one or multiple MCP clients</mark>
* <mark style="color:red;background-color:purple;">**MCP Client: A component that maintains a connection to an MCP server**</mark> <mark style="color:purple;background-color:purple;">and obtains context from an MCP server for the MCP host to use</mark>
* <mark style="color:red;background-color:purple;">**For connecting to every MCP server an additional MCP client will be instantiated**</mark>
* <mark style="color:red;background-color:purple;">**MCP Server**</mark><mark style="color:red;background-color:purple;">:</mark>&#x20;
  * <mark style="color:red;background-color:purple;">**Provides ⇒ Tools, Resources, Prompt**</mark>
  * <mark style="color:purple;background-color:purple;">A program that provides context to MCP clients</mark>
  * <mark style="color:purple;background-color:purple;">It provides Tools, Resources and Prompts</mark>
  * <mark style="color:purple;background-color:purple;">Resources like file content, documentation</mark>

<mark style="color:purple;background-color:purple;">**Example:**</mark>

* <mark style="color:purple;background-color:purple;">Visual Studio Code acts as an MCP host.</mark>&#x20;
* <mark style="color:purple;background-color:purple;">When Visual Studio Code establishes a connection to an MCP server, the Visual Studio Code runtime instantiates an MCP client object that maintains the connection to the Sentry MCP server.</mark>
* <mark style="color:purple;background-color:purple;">When Visual Studio Code subsequently connects to another MCP server the Visual Studio Code runtime instantiates an additional MCP client object to maintain this connection.</mark>

<mark style="color:red;background-color:purple;">**Layers:**</mark>

* <mark style="color:red;background-color:purple;">**Data layer**</mark><mark style="color:red;background-color:purple;">:</mark>&#x20;
  * <mark style="color:red;background-color:purple;">**Defines the JSON-RPC 2.0 based protocol for client-server communication**</mark>
  * <mark style="color:red;background-color:purple;">**RPC - Remote procedure call**</mark>
  * <mark style="color:red;background-color:purple;">**Host cannot speak RPC ⇒ Allows a program to execute a function on another machine as if its a local, abstracting the details of transfer of data and network communication**</mark>
  * <mark style="color:red;background-color:purple;">**Why JSON RPC? ⇒ plain, human readable, same shape over stdio and http, can be used for both side communication**</mark>
  * <mark style="color:purple;background-color:purple;">Includes:</mark>
    * <mark style="color:red;background-color:purple;">**Discovery:**</mark>
      * <mark style="color:red;background-color:purple;">**Lets clients query a server’s supported protocol versions, capabilities, and identity**</mark> <mark style="color:purple;background-color:purple;">through the</mark> <mark style="color:purple;background-color:purple;"></mark><mark style="color:purple;background-color:purple;">`server/discover`</mark> <mark style="color:purple;background-color:purple;"></mark><mark style="color:purple;background-color:purple;">request</mark>
* <mark style="color:red;background-color:purple;">**Transport layer**</mark><mark style="color:red;background-color:purple;">:</mark>&#x20;
  * <mark style="color:purple;background-color:purple;">Defines the communication mechanisms and channels that enable data exchange between clients and servers, including transport-specific connection establishment, message framing, and authorization.</mark>
  * <mark style="color:purple;background-color:purple;">Two transport mechanism:</mark>
  * <mark style="color:red;background-color:purple;">**Stdio transport:**</mark>&#x20;
    * <mark style="color:red;background-color:purple;">**Uses standard input/output streams for direct process communication between local processes on the same machine**</mark><mark style="color:purple;background-color:purple;">, providing optimal performance with no network overhead.</mark>
  * <mark style="color:red;background-color:purple;">**Streamable HTTP transport:**</mark>&#x20;
    * <mark style="color:red;background-color:purple;">**Uses HTTP POST for client-to-server messages**</mark> <mark style="color:purple;background-color:purple;">with optional Server-Sent Events for streaming capabilities.</mark>&#x20;
    * <mark style="color:purple;background-color:purple;">This transport enables remote server communication and supports standard HTTP authentication methods including bearer tokens, API keys, and custom headers.</mark>&#x20;
    * <mark style="color:red;background-color:purple;">**MCP recommends using OAuth to obtain authentication tokens.**</mark>

<mark style="color:red;background-color:purple;">**Statelessness:**</mark>

* <mark style="color:red;background-color:purple;">**MCP is a stateless protocol**</mark>
* <mark style="color:red;background-color:purple;">**Every request carries the protocol version and the capabilities relevant to that request in its**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**`_meta`**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**field, so the server can process each request on its own.**</mark>

<mark style="color:purple;background-color:purple;">**MCP servers can expose (Primitive):**</mark>

* <mark style="color:purple;background-color:purple;">Tools</mark>
* <mark style="color:purple;background-color:purple;">Resources: e.g., file contents, database records, API responses</mark>
* <mark style="color:purple;background-color:purple;">Prompts</mark>
* <mark style="color:red;background-color:purple;">**MCP clients will use the**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**`*/list`**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**methods to discover available primitives.**</mark>&#x20;
* <mark style="color:red;background-color:purple;">**For example, a client can first list all available tools (**</mark><mark style="color:red;background-color:purple;">**`tools/list`**</mark><mark style="color:red;background-color:purple;">**) and then execute them.**</mark>&#x20;

