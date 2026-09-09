# 🟢 Custom Middleware

* <mark style="color:purple;background-color:purple;">**In Prebuilt, we just had to use it, where and how it will get called was pre decided**</mark>
* <mark style="color:purple;background-color:purple;">**Here, we will have to**</mark>**&#x20;**<mark style="color:red;background-color:purple;">**specify where the middleware needs to get called**</mark>
* <mark style="color:purple;background-color:purple;">**By implementing hooks that run at specific points in the agent execution flow.**</mark>
* <mark style="color:purple;background-color:purple;">**Hooks ⇒ Extension points in custom middleware that let intercept, inspect or modify agent execution at specific stages**</mark>
* <mark style="color:purple;background-color:purple;">**Two styles of hooks:**</mark>
  * <mark style="color:red;background-color:purple;">**Node style hooks**</mark><mark style="color:purple;background-color:purple;">**:**</mark>
    * <mark style="color:purple;background-color:purple;">**Run sequentially at specific execution points**</mark>
    * <mark style="color:purple;background-color:purple;">**Use for logging, validation, and state updates**</mark>
    * <mark style="color:red;background-color:purple;">**before\_agent, before\_model, after\_model, after\_agent**</mark>
    * <mark style="color:red;background-color:purple;">**Here we get the state and runtime**</mark>
  * <mark style="color:red;background-color:purple;">**Wrap style hooks:**</mark>
    * <mark style="color:purple;background-color:purple;">**Run around each model or tool call**</mark>
    * <mark style="color:purple;background-color:purple;">**wrap\_model\_call, wrap\_tool\_call**</mark>
    * <mark style="color:red;background-color:purple;">**Here we get the exact request**</mark>
  * <mark style="color:red;background-color:purple;">**We basically have to create a function with decorator like @before\_model or @after\_model and then we need to pass the same in middleware**</mark>
  * <mark style="color:red;background-color:purple;">**For example in wrap\_model\_call based on the messages we can change the model to be used**</mark>
* <mark style="color:purple;background-color:purple;">**We can define middleware using decorator as well as class**</mark>
*

    <figure><img src=".gitbook/assets/image (14).png" alt=""><figcaption></figcaption></figure>
* <mark style="color:purple;background-color:purple;">**Class based:**</mark>

```python
from langchain.agents.middleware import AgentMiddleware, AgentState, hook_config
from langchain.messages import AIMessage
from langgraph.runtime import Runtime
from typing import Any

class MessageLimitMiddleware(AgentMiddleware):
    def __init__(self, max_messages: int = 50):
        super().__init__()
        self.max_messages = max_messages

    @hook_config(can_jump_to=["end"])
    def before_model(self, state: AgentState, runtime: Runtime) -> dict[str, Any] | None:
        if len(state["messages"]) >= self.max_messages:
            return {
                "messages": [AIMessage("Conversation limit reached.")],
                "jump_to": "end"
            }
        return None

    def after_model(self, state: AgentState, runtime: Runtime) -> dict[str, Any] | None:
        print(f"Model returned: {state['messages'][-1].content}")
        return None
```

<mark style="color:red;background-color:purple;">**Custom State Schema:**</mark>

* <mark style="color:red;background-color:purple;">**If your middleware needs to track state across hooks, middleware can extend the agent’s state with custom properties.**</mark>

```python
from langchain.agents import create_agent
from langchain.messages import HumanMessage
from langchain.agents.middleware import AgentState, before_model, after_model
from typing_extensions import NotRequired
from typing import Any
from langgraph.runtime import Runtime


class CustomState(AgentState):
    model_call_count: NotRequired[int]
    user_id: NotRequired[str]


@before_model(state_schema=CustomState, can_jump_to=["end"])
def check_call_limit(state: CustomState, runtime: Runtime) -> dict[str, Any] | None:
    count = state.get("model_call_count", 0)
    if count > 10:
        return {"jump_to": "end"}
    return None


@after_model(state_schema=CustomState)
def increment_counter(state: CustomState, runtime: Runtime) -> dict[str, Any] | None:
    return {"model_call_count": state.get("model_call_count", 0) + 1}


agent = create_agent(
    model="gpt-5.5",
    middleware=[check_call_limit, increment_counter],
    tools=[],
)

# Invoke with custom state
result = agent.invoke({
    "messages": [HumanMessage("Hello")],
    "model_call_count": 0,
    "user_id": "user-123",
})
```

