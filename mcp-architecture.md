# 🟢 MCP Architecture

* <mark style="color:purple;background-color:purple;">**MCP SDKs abstract away many concerns**</mark>
* <mark style="color:purple;background-color:purple;">**If we create a langchain application which used MCP**</mark>
* <mark style="color:purple;background-color:purple;">**YOUR APP (Host)**</mark>  \ <mark style="color:purple;background-color:purple;">**├── Client 1 → Server A (e.g., database)**</mark>  \ <mark style="color:purple;background-color:purple;">**├── Client 2 → Server B (e.g., GitHub)**</mark>  \ <mark style="color:purple;background-color:purple;">**└── Client 3 → Server C (e.g., Slack)**</mark>
*



<mark style="color:purple;background-color:purple;">**Component:**</mark>

* <mark style="color:purple;background-color:purple;">**MCP Host:**</mark> Environment from where we have build the connection, if we are calling from python then python is our host, if we are using claude then it is our host
* <mark style="color:purple;background-color:purple;">**MCP Client: The connector code inside your app**</mark>
* <mark style="color:purple;background-color:purple;">**MCP Server:**</mark>
*

    <figure><img src=".gitbook/assets/image (4).png" alt=""><figcaption></figcaption></figure>
*

    <figure><img src=".gitbook/assets/image (3).png" alt=""><figcaption></figcaption></figure>
