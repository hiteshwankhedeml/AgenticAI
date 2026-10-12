# 🟢 Build an MCP Client

<mark style="color:purple;background-color:purple;">**Client is initiated (once, at startup)**</mark>

```python
# Python – `async with` handles connect AND cleanup
params = StdioServerParameters(command="python", args=[server_script_path])
async with Client(stdio_client(params)) as client:
    await chat_loop(client)
```

<mark style="color:purple;background-color:purple;">**1. Client asks the server for its tool list (**</mark><mark style="color:purple;background-color:purple;">**`list_tools`**</mark><mark style="color:purple;background-color:purple;">**)**</mark>

```python
tool_list = await client.list_tools()
available_tools = [{"name": t.name, "description": t.description,
                    "input_schema": t.input_schema} for t in tool_list.tools]
```

<mark style="color:purple;background-color:purple;">**2. User query + tool descriptions are sent to Claude**</mark>

```python
messages = [{"role": "user", "content": query}]
response = anthropic.messages.create(model=MODEL, max_tokens=1000,
                                     messages=messages, tools=available_tools)
```

<mark style="color:purple;background-color:purple;">**3. Claude decides which tools (if any) to use and returns**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`tool_use`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**blocks**</mark>

```python
for content in response.content:
    if content.type == 'text':
        final_text.append(content.text)
    elif content.type == 'tool_use':
        tool_name, tool_args = content.name, content.input   # Claude only *asks*
```

<mark style="color:purple;background-color:purple;">**4. Client executes each tool call via the MCP server (**</mark><mark style="color:purple;background-color:purple;">**`call_tool`**</mark><mark style="color:purple;background-color:purple;">**)**</mark>

```python
result = await client.call_tool(tool_name, tool_args)   # returns CallToolResult
```

<mark style="color:purple;background-color:purple;">**5. Tool results are sent back to Claude as**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`tool_result`**</mark>

```python
tool_results.append({"type": "tool_result", "tool_use_id": content.id,
                     "content": "\n".join(b.text for b in result.content
                                          if isinstance(b, TextContent)),
                     "is_error": result.is_error})
messages.append({"role": "assistant", "content": response.content})
messages.append({"role": "user", "content": tool_results})
```

<mark style="color:purple;background-color:purple;">**6. Claude writes a natural-language answer**</mark>

```python
response = anthropic.messages.create(model=MODEL, max_tokens=1000,
                                     messages=messages, tools=available_tools)
final_text += [c.text for c in response.content if c.type == 'text']
```

<mark style="color:purple;background-color:purple;">**7. Client displays it**</mark>

