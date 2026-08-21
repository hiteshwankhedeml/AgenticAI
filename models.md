# Models

* LLM
* You initialize a model with `init_chat_model("openai:gpt-5.5", api_key=..., temperature=0.7)`
* Can be called as invoke, stream, batch
* **Tool Calling:**
  * `model.bind_tools([fn])` : tells the model which tools exist and what their schemas look like
  * Model doesn't run the tool itself
  * For server side tools in that case the result comes back alreayd embedded in the response
* **Capabilities:**
  * **Multimodal:**&#x20;
    * check model.profile ⇒ if model supports.&#x20;
    * images/audio/video can be passed in messages
  * **Reasoning:**&#x20;
    * We can specify how much efforts it needs to put up
    * &#x20;thinking shows up as a separate `"reasoning"` block if you're streaming
  * **Prompt caching:**
    * reduces cost/latency on repeated context
    * Can be done automatically or configured via explicit provider cache points
    * AnthropicPromptCachingMiddleware
  * **Rate limiting:** to avoid hitting provider limits
  * **Custom endpoints:** point at any OpenAI-compatible API, not just the official one
  * **Routing between models dynamically:**
    * `@wrap_model_call` middleware hook can pick a cheaper or more capable model depending on conversation state (e.g. use a bigger model once the conversation gets long)

```python
# Basic invoke, tool binding, and streaming:
model = init_chat_model("openai:gpt-5.5", temperature=0)
model.invoke("hello").text

ai = model.bind_tools([get_weather]).invoke("weather in SF?")
ai.tool_calls          # [{'name': 'get_weather', 'args': {...}, 'id': 'call_1'}]

for chunk in model.stream("write a haiku"):
    print(chunk.text, end="")

# Turning up reasoning effort, and reading the reasoning steps out while streaming:
model = ChatAnthropic(model="claude-sonnet-4-6")
response = model.invoke("Why do parrots have colorful feathers?", reasoning_effort="high")

for chunk in model.stream("Why do parrots have colorful feathers?"):
    reasoning_steps = [r for r in chunk.content_blocks if r["type"] == "reasoning"]
    print(reasoning_steps if reasoning_steps else chunk.text)
```
