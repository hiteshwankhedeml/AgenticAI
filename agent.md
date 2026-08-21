# Agent

* Created using model and tools and returns compiled langGraph graph
* We can also give it
  * system\_prompt
  * response\_format
  * state\_schema
  * context\_schema
  * checkpointer
  * middleware
  * name
* Agent state is a dictionary like structure which holds everything that the agents knows including messages
* New messages are always appended to the list
* Invoke agent ⇒ agent.invoke({"messages": \[{"role": "user", "content": "..."}]}, config={"configurable": {"thread\_id": "..."\}})
* thread\_id ⇒ to let conversation continue across call
* checkpointer ⇒ to store the history ⇒ InMemorySaver() to store it locally
* Model strings are prefixed by provider: `openai:`, `anthropic:`, `google_genai:`&#x20;

```python
agent = create_agent(
    model="anthropic:claude-sonnet-4-6",
    tools=[get_weather],
    system_prompt="You are a helpful assistant.",
    checkpointer=InMemorySaver(),        # required for thread_id memory
)
cfg = {"configurable": {"thread_id": "1"}}
result = agent.invoke({"messages": [{"role": "user", "content": "weather in SF?"}]}, config=cfg)
result["messages"][-1].text
```
