# 🟢 Messages

* <mark style="color:purple;background-color:purple;">**Consists of:**</mark>
  * <mark style="color:purple;background-color:purple;">**Role**</mark> ⇒ Human, System, AI, Tool
  * <mark style="color:purple;background-color:purple;">**Content**</mark>
  * <mark style="color:purple;background-color:purple;">**Metadata**</mark> ⇒ IDs, token usage
* There can be different types of contents like image, code block etc
* <mark style="color:purple;background-color:purple;">**Can be passed as text prompt, as message object (SystemMessage, AIMessages, HumanMessage)**</mark>&#x20;
* <mark style="color:purple;background-color:purple;">**We can pass a list of message objects or dictionary of role and message to the model**</mark>
* Multimodal can be passed in message as message in content block with type and text/url/base64
* Message Types:
  * System message ⇒ initial set of instructions that primes the model’s behavior.
  * Human message ⇒ can contain text or multimodal input
  * AI message ⇒ output of a model invocation
  * Tool message
* <mark style="color:purple;background-color:purple;">**Multi modal input can be passed as: type and content**</mark>

```python
human_message = HumanMessage(content=[
    {"type": "text", "text": "Hello, how are you?"},
    {"type": "image_url", "image_url": {"url": "https://example.com/image.jpg"}}
])


```

*

    <figure><img src=".gitbook/assets/image (6).png" alt=""><figcaption></figcaption></figure>









