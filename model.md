# 🟢 Model

* <mark style="color:purple;background-color:purple;">**Can we utilized**</mark>**&#x20;**<mark style="color:red;background-color:purple;">**with agents or standalone**</mark>
* Use invoke to intialize a model
  * invoke
  * Stream
  * Batch
* <mark style="color:purple;background-color:purple;">**We can pass a list of messages(role and content) as history to the model as json OR message object**</mark>
* <mark style="color:red;background-color:purple;">**Models can REQUEST to call tools**</mark>
  * Tools consists of a schema, including the name of the tool, a description and arguments
  * A function&#x20;
* Tools needs to be binded to the model
* <mark style="color:red;background-color:purple;">**model.profile ⇒ information about the model**</mark> ⇒ dictionary which contains max\_token supported, tool call supported etc&#x20;
* <mark style="color:purple;background-color:purple;">**Reasoning:**</mark>
  * <mark style="color:purple;background-color:purple;">**Breaking down complex problems into smaller, more manageable steps**</mark>
  * <mark style="color:purple;background-color:purple;">**Depending on the model, you can sometimes specify the level of effort it should put into reasoning**</mark>
* <mark style="color:red;background-color:purple;">**Prompt caching features to reduce latency and cost on repeat processing of the same tokens**</mark>
  * <mark style="color:red;background-color:purple;">**Implicit:**</mark> <mark style="color:red;background-color:purple;"></mark><mark style="color:red;background-color:purple;">providers automatically pass on cost savings if a request hits a cache</mark>
  * <mark style="color:red;background-color:purple;">**Provider-level explicit:**</mark> <mark style="color:red;background-color:purple;"></mark><mark style="color:red;background-color:purple;">let you manually indicate cache points for greater control or to guarantee cost savings.</mark>
  * <mark style="color:red;background-color:purple;">**LangChain middleware:**</mark> <mark style="color:red;background-color:purple;"></mark><mark style="color:red;background-color:purple;">for agents, middleware lets LangChain optimize caching of stable system prompt and tool content</mark>
* <mark style="color:purple;background-color:purple;">**server-side**</mark> [<mark style="color:purple;background-color:purple;">**tool-calling**</mark>](https://docs.langchain.com/oss/python/langchain/models#tool-calling) <mark style="color:purple;background-color:purple;">**loops: models can interact with web search, code interpreters, and other tools and analyze the results in a single conversational turn**</mark>
* <mark style="color:purple;background-color:purple;">**They need to be binded to the model ⇒**</mark>&#x20;
  * <mark style="color:purple;background-color:purple;">**tool = {"type": "web\_search"}**</mark>&#x20;
  * <mark style="color:purple;background-color:purple;">**model\_with\_tools = model.bind\_tools(\[tool])**</mark>
* <mark style="color:red;background-color:purple;">**rate\_limiter:**</mark> <mark style="color:red;background-color:purple;"></mark><mark style="color:red;background-color:purple;">control the rate at which invocations are done</mark>
* <mark style="color:purple;background-color:purple;">**Dynamic Model Selection:**</mark>
  * Dynamic <mark style="color:purple;background-color:purple;">**models are selected at runtime based on the current state and context.**</mark>&#x20;
  * This enables <mark style="color:purple;background-color:purple;">**sophisticated routing logic and cost optimization**</mark>
  * <mark style="color:red;background-color:purple;">**We can create a function with @wrap\_model\_call ⇒ in this based on conditions like message\_count we return model to be used**</mark>
  * <mark style="color:red;background-color:purple;">**pass this in middleware while creating the agent**</mark>

```python
# As Json
conversation = [
    {"role": "system", "content": "You are a helpful assistant that translates English to French."},
    {"role": "user", "content": "Translate: I love programming."},
    {"role": "assistant", "content": "J'adore la programmation."},
    {"role": "user", "content": "Translate: I love building applications."}
]

response = model.invoke(conversation)
print(response)  # AIMessage("J'adore créer des applications.")

# As messages
from langchain.messages import HumanMessage, AIMessage, SystemMessage

conversation = [
    SystemMessage("You are a helpful assistant that translates English to French."),
    HumanMessage("Translate: I love programming."),
    AIMessage("J'adore la programmation."),
    HumanMessage("Translate: I love building applications.")
]

response = model.invoke(conversation)
print(response)  # AIMessage("J'adore créer des applications.")
```

<mark style="color:purple;background-color:purple;">**Different Parameters:**</mark>

* <mark style="color:purple;background-color:purple;">temperature</mark>
* <mark style="color:purple;background-color:purple;">max\_tokens</mark>
* <mark style="color:purple;background-color:purple;">time\_out</mark>
* <mark style="color:purple;background-color:purple;">max\_retries</mark> ⇒ default 6

