# 🔴 State vs Runtime vs Context

<mark style="color:purple;background-color:purple;">**State: information about the current workflow/task that can evolve during execution.**</mark>

<mark style="color:purple;background-color:purple;">**Context:**</mark>&#x20;

* <mark style="color:purple;background-color:purple;">**Information about the execution**</mark>
* <mark style="color:purple;background-color:purple;">**This is different from LLM context**</mark>
* <mark style="color:purple;background-color:purple;">**LLM context = System instructions + Messages + Relevant documents + Tool results + Relevant state**</mark>

| State                                                                                                                                               | Runtime Context                                                                                                                                                                                                               |
| --------------------------------------------------------------------------------------------------------------------------------------------------- | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| <mark style="color:purple;background-color:purple;">**Tracks workflow/task information**</mark>                                                     | <mark style="color:purple;background-color:purple;">**Provides execution-specific information**</mark>                                                                                                                        |
| <mark style="color:purple;background-color:purple;">**Evolves during execution**</mark>                                                             | <mark style="color:purple;background-color:purple;">**Supplied for the execution**</mark>                                                                                                                                     |
| <mark style="color:purple;background-color:purple;">**Can be updated by agent/workflow**</mark>                                                     | <mark style="color:purple;background-color:purple;">**Not treated as evolving workflow state**</mark>                                                                                                                         |
| <mark style="color:purple;background-color:purple;">**Example: messages, search results**</mark>                                                    | <mark style="color:purple;background-color:purple;">**Example: user ID, dependencies**</mark>                                                                                                                                 |
| <mark style="color:purple;">**Defined by**</mark><mark style="color:purple;">**&#x20;**</mark><mark style="color:purple;">**`state_schema`**</mark> | <mark style="color:purple;background-color:purple;">**Defined by**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`context_schema`**</mark> |

* <mark style="color:purple;background-color:purple;">**`ToolRuntime`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**is an interface available to a tool for accessing information about the current agent execution**</mark>
* <mark style="color:purple;background-color:purple;">**Middleware receives execution information through its middleware hook/request object.**</mark>

```python
# STATE SCHEMA
class MyState(AgentState):
    order_id: str | None

# CONTEXT SCHEMA
@dataclass
class MyContext:
    user_id: str
    user_role: str

@tool
def get_order_status(
    order_id: str,
    runtime: ToolRuntime
) -> str:
    """
    Get the status of an order.
    """
    # ---- Access STATE ----
    current_order = runtime.state.get("order_id")
    # ---- Access RUNTIME CONTEXT ----
    user_id = runtime.context.user_id
    user_role = runtime.context.user_role
    return f"Order {order_id} is shipped."

@before_model
def my_middleware(
    state: MyState,
    runtime: Runtime[MyContext]
) -> dict | None:
    print(state)
    print(runtime.context.user_id)

    return None

agent = create_agent(
    model="gpt-5-nano",
    tools=[get_order_status],
    state_schema=MyState,
    context_schema=MyContext,
    middleware=[my_middleware],
)

result = agent.invoke(
    {
        "messages": [
            {
                "role": "user",
                "content": "What's the status of my order?"
            }
        ],
        # STATE
        "order_id": "ORD-123",
    },
    # RUNTIME CONTEXT
    context=MyContext(
        user_id="USER-456",
        user_role="premium"
    ),
)
```
