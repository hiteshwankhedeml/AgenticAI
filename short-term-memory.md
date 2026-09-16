# 🟢 Short-term memory

* <mark style="color:red;background-color:purple;">**AgentState**</mark>
* <mark style="color:purple;background-color:purple;">**Lets your application remember**</mark>**&#x20;**<mark style="color:red;background-color:purple;">**previous interactions within a single thread or conversation**</mark>
* <mark style="color:purple;background-color:purple;">**Chat models accept context using**</mark> [<mark style="color:purple;background-color:purple;">**messages**</mark>](https://docs.langchain.com/oss/python/langchain/messages)<mark style="color:purple;background-color:purple;">**, which include instructions (a system message) and inputs (human messages)**</mark>
* <mark style="color:red;background-color:purple;">**The default**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**`AgentState`**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**contains only the**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**`messages`**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**field (**</mark><mark style="color:red;background-color:purple;">**`list[BaseMessage]`**</mark><mark style="color:red;background-color:purple;">**).**</mark>
* <mark style="color:red;background-color:purple;">**You can create custom state schemas by subclassing**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**`AgentState`**</mark><mark style="color:red;background-color:purple;">**.**</mark>
* <mark style="color:red;background-color:purple;">**Custom agent states automatically inherit the built-in**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**`messages`**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**field alongside any new custom keys you add.**</mark>
* <mark style="color:red;background-color:purple;">**need to specify a**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**`checkpointer`**</mark><mark style="color:red;background-color:purple;">**&#x20;**</mark><mark style="color:red;background-color:purple;">**when creating an agent.**</mark>
* <mark style="color:red;background-color:purple;">**We can also save it to postgres DB**</mark>

```python
from langgraph.checkpoint.memory import InMemorySaver  

agent = create_agent(
    model="openai:gpt-5.5",
    tools=[get_user_info],
    checkpointer=InMemorySaver(),
)

thread_config = {"configurable": {"thread_id": "1"}}
response = agent.invoke(
    {"messages": [{"role": "user", "content": "Hi! My name is Bob."}]},
    thread_config,
)["messages"][-1].content
```

* <mark style="color:purple;background-color:purple;">**In production, use a checkpointer backed by a database**</mark>

```python
pip install -U langgraph-checkpoint-postgres "psycopg[binary]"

from langgraph.checkpoint.postgres import PostgresSaver 

DB_URI = "postgresql://postgres:postgres@localhost:5432/postgres?sslmode=disable"
with PostgresSaver.from_conn_string(DB_URI) as checkpointer:
    checkpointer.setup() # auto create tables in PostgreSQL
    agent = create_agent(
        "gpt-5.5",
        tools=[get_user_info],
        checkpointer=checkpointer,
    )
```

<mark style="color:purple;background-color:purple;">**Customizing memory:**</mark>

* <mark style="color:purple;background-color:purple;">**Agents use**</mark> [<mark style="color:purple;background-color:purple;">**`AgentState`**</mark>](https://reference.langchain.com/python/langchain/agents/middleware/types/AgentState) <mark style="color:purple;background-color:purple;">**to manage short term memory, specifically the conversation history via a**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`messages`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**key**</mark>
* <mark style="color:purple;background-color:purple;">**We can add additional fields**</mark>
* <mark style="color:red;background-color:purple;">**We can pass custom state schema using the state\_schema in create\_agent**</mark>
* <mark style="color:red;background-color:purple;">**During invocation, we can pass the values of the custom fields**</mark>

```python
class CustomAgentState(AgentState):
    user_id: str
    preferences: dict

agent = create_agent(
    "gpt-5.5",
    tools=[get_user_info],
    state_schema=CustomAgentState,
    checkpointer=InMemorySaver(),
)

result = agent.invoke(
    {
        "messages": [{"role": "user", "content": "Hello"}],
        "user_id": "user_123",
        "preferences": {"theme": "dark"}
    },
    {"configurable": {"thread_id": "1"}})
```

<mark style="color:purple;background-color:purple;">**Trim Message:**</mark>

* <mark style="color:purple;background-color:purple;">**count the tokens in the message history and truncate whenever it approaches that limit**</mark>
* <mark style="color:red;background-color:purple;">**We can create a function with @before\_model ⇒ in this add logic for trimming the old messages**</mark>
* <mark style="color:red;background-color:purple;">**Pass this middleware in create\_agent**</mark>
* <mark style="color:red;background-color:purple;">**Similarly we can also remove messages also**</mark>

<mark style="color:purple;background-color:purple;">**Access Memory:**</mark>

* <mark style="color:purple;background-color:purple;">**Access short term memory (state) in a tool using the**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`runtime`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**parameter (typed as**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`ToolRuntime`**</mark><mark style="color:purple;background-color:purple;">**).**</mark>
* <mark style="color:red;background-color:purple;">**runtime.state\["user\_id"] ⇒ Read memory inside a tool**</mark>
* <mark style="color:purple;background-color:purple;">**To modify the agent’s short-term memory (state) during execution, you can return state updates directly from the tools**</mark>

```python
return Command(update={
        "user_name": name,
        # update the message history
        "messages": [
            ToolMessage(
                "Successfully looked up user information",
                tool_call_id=runtime.tool_call_id
            )
        ]
    }
```

* <mark style="color:red;background-color:purple;">**Access short term memory (state) in middleware to create dynamic prompts based on conversation history or custom state fields**</mark>
* <mark style="color:red;background-color:purple;">**We will create a function with @dynamic\_prompt and pass it in middleware**</mark>

```python
@dynamic_prompt
def dynamic_system_prompt(request: ModelRequest) -> str:
    user_name = request.runtime.context["user_name"]
    system_prompt = f"You are a helpful assistant. Address the user as {user_name}."
    return system_prompt

agent = create_agent(
    model="gpt-5-nano",
    tools=[get_weather],
    middleware=[dynamic_system_prompt],
    context_schema=CustomContext,
)

result = agent.invoke(
    {"messages": [{"role": "user", "content": "What is the weather in SF?"}]},
    context=CustomContext(user_name="John Smith"),
)
```

<mark style="color:purple;background-color:purple;">**Before Model:**</mark>&#x20;

* <mark style="color:red;background-color:purple;">**Access short term memory (state) in**</mark> [<mark style="color:red;background-color:purple;">**`@before_model`**</mark>](https://reference.langchain.com/python/langchain/agents/middleware/types/before_model) <mark style="color:red;background-color:purple;">**middleware to process messages before model calls**</mark>

<mark style="color:purple;background-color:purple;">**After Model:**</mark>

* <mark style="color:red;background-color:purple;">**Access short term memory (state) in**</mark> [<mark style="color:red;background-color:purple;">**`@after_model`**</mark>](https://reference.langchain.com/python/langchain/agents/middleware/types/after_model) <mark style="color:red;background-color:purple;">**middleware to process messages after model calls**</mark>
