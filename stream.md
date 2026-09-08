# 🟢 Stream

* User experience matters&#x20;
* Text appears as its generated
* <mark style="color:red;background-color:purple;">**stream() returns an iterator that yields output chunks as they are produced**</mark>
*   <mark style="color:purple;background-color:purple;">**We can even loop over the iterator to show output**</mark>

    <figure><img src=".gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

```python
from langchain.agents import create_agent


def get_weather(city: str) -> str:
    """Get weather for a given city."""

    return f"It's always sunny in {city}!"

agent = create_agent(
    model="gpt-5-nano",
    tools=[get_weather],
)
for chunk in agent.stream(
    {"messages": [{"role": "user", "content": "What is the weather in SF?"}]},
    stream_mode="messages",
    version="v2",
):
    if chunk["type"] == "messages":
        token, metadata = chunk["data"]
        print(f"node: {metadata['langgraph_node']}")
        print(f"content: {token.content_blocks}")
        print("\n")
```
