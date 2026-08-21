# 🟢 Model

* Use invoke to intialize a model
  * invoke
  * Stream
  * Batch
* We can pass a list of messages(role and content) as history to the model as json OR message object
* Models can REQUEST to call tools
  * Tools consists of a schema, including the name of the tool, a description and arguments
  * A function&#x20;
* Tools needs to be binded to the model
* **model.profile** ⇒ dictionary which contains max\_token supported, tool call supported etc informated about the model
* **Reasoning:**
  * Breaking down complex problems into smaller, more manageable steps
  * Depending on the model, you can sometimes specify the level of effort it should put into reasoning
* Prompt caching features to reduce latency and cost on repeat processing of the same tokens
* server-side [tool-calling](https://docs.langchain.com/oss/python/langchain/models#tool-calling) loops: models can interact with web search, code interpreters, and other tools and analyze the results in a single conversational turn
* Using the rate\_limiter we can control the rate at which invocations are done
* **Dynamic Model Selection:**
  * Dynamic models are selected at runtime based on the current state and context.&#x20;
  * This enables sophisticated routing logic and cost optimization
  * We can create a function with @wrap\_model\_call ⇒ in this based on conditions like message\_count we return model to be used
  * In middleware we will pass this in middleware while creating the agent

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

