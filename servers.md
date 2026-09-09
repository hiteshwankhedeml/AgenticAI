# 🟢 Servers

* <mark style="color:red;background-color:purple;">**Client and server speak the same language JSONRPC**</mark>
* <mark style="color:purple;background-color:purple;">**Provides:**</mark>
  * <mark style="color:red;background-color:purple;">**Tools:**</mark>
    * <mark style="color:red;background-color:purple;">**tools/list ⇒ Discover available tools**</mark>
    * <mark style="color:red;background-color:purple;">**tools/call ⇒ Execute a specific tool**</mark>
    * <mark style="color:purple;background-color:purple;">**Tools are model-controlled, meaning AI models can discover and invoke them automatically.**</mark>
  * <mark style="color:red;background-color:purple;">**Resources:**</mark>
    * <mark style="color:red;background-color:purple;">**Information that the AI application can retrieve and provide to models as context**</mark>
    * <mark style="color:red;background-color:purple;">**Expose data from files, APIs, databases, or any other source that an AI needs to**</mark>**&#x20;**<mark style="color:purple;background-color:purple;">**understand context.**</mark>
    * <mark style="color:purple;background-color:purple;">**Each resource has a unique URI (e.g.,**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`file:///path/to/document.md`**</mark><mark style="color:purple;background-color:purple;">**) and declares its MIME type for appropriate content handling.**</mark>
    * <mark style="color:purple;background-color:purple;">**Types:**</mark>
      * <mark style="color:red;background-color:purple;">**Direct Resources:**</mark>
        * <mark style="color:red;background-color:purple;">**fixed URIs that point to specific data.**</mark>**&#x20;**<mark style="color:purple;background-color:purple;">**Example:**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`calendar://events/2024`**</mark>
      * <mark style="color:red;background-color:purple;">**Resource Templates:**</mark>
        * <mark style="color:red;background-color:purple;">**dynamic URIs with parameters**</mark>
        * <mark style="color:purple;background-color:purple;">**travel://activities/{city}/{category}**</mark>
      * <mark style="color:red;background-color:purple;">**Use list and read method**</mark>&#x20;
  * <mark style="color:red;background-color:purple;">**Prompts:**</mark>
    * <mark style="color:red;background-color:purple;">**prompts/list**</mark>
    * <mark style="color:red;background-color:purple;">**prompts/get**</mark>

