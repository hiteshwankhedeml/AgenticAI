# 🟢 Messages

* <mark style="color:red;background-color:purple;">**Consists of:**</mark>
  * <mark style="color:red;background-color:purple;">**Role ⇒ Human, System, AI, Tool**</mark>
  * <mark style="color:red;background-color:purple;">**Content**</mark>
  * <mark style="color:red;background-color:purple;">**Metadata ⇒ IDs, token usage**</mark>

```python
human_msg = HumanMessage(
    content="Hello!",
    name="alice",  # Optional: identify different users
    id="msg_123",  # Optional: unique identifier for tracing
)
```

* <mark style="color:purple;background-color:purple;">**There can be different types of contents like image, code block etc**</mark>
* <mark style="color:purple;background-color:purple;">**Can be passed as text prompt, as message object (SystemMessage, AIMessages, HumanMessage)**</mark>&#x20;
* <mark style="color:purple;background-color:purple;">**We can pass a list of message objects or dictionary of role and message to the model**</mark>
* <mark style="color:red;background-color:purple;">**Multimodal can be passed in message as message in content block with type and text/url/base64**</mark>
* <mark style="color:purple;background-color:purple;">**Message Types:**</mark>
  * <mark style="color:purple;background-color:purple;">**System message ⇒ initial set of instructions that primes the model’s behavior.**</mark>
  * <mark style="color:purple;background-color:purple;">**Human message ⇒ can contain text or multimodal input**</mark>
  * <mark style="color:purple;background-color:purple;">**AI message ⇒ output of a model invocation**</mark>
  * <mark style="color:purple;background-color:purple;">**Tool message**</mark>
* <mark style="color:purple;background-color:purple;">**An**</mark> [<mark style="color:purple;background-color:purple;">**`AIMessage`**</mark>](https://reference.langchain.com/python/langchain-core/messages/ai/AIMessage) <mark style="color:purple;background-color:purple;">**can hold token counts:**</mark>

```python
{'input_tokens': 8,
 'output_tokens': 304,
 'total_tokens': 312,
 'input_token_details': {'audio': 0, 'cache_read': 0},
 'output_token_details': {'audio': 0, 'reasoning': 256}}
```

* <mark style="color:purple;background-color:purple;">**When models make**</mark> [<mark style="color:purple;background-color:purple;">**tool calls**</mark>](https://docs.langchain.com/oss/python/langchain/models#tool-calling)<mark style="color:purple;background-color:purple;">**, they’re included in the**</mark> [<mark style="color:purple;background-color:purple;">**`AIMessage`**</mark>](https://reference.langchain.com/python/langchain-core/messages/ai/AIMessage)

```python
for tool_call in response.tool_calls:
    print(f"Tool: {tool_call['name']}")
    print(f"Args: {tool_call['args']}")
    print(f"ID: {tool_call['id']}")
```

* <mark style="color:red;background-color:purple;">**Multi modal input can be passed as: type and content**</mark>

```python
human_message = HumanMessage(content=[
    {"type": "text", "text": "Hello, how are you?"},
    {"type": "image_url", "image_url": {"url": "https://example.com/image.jpg"}}
])
```

*

    <figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>
*







