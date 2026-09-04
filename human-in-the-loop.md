# Human in the Loop

* Add human oversight to agent tool calls
* If intervention is needed, the middleware issues an [interrupt](https://reference.langchain.com/python/langgraph/types/interrupt) that halts execution.&#x20;
* The graph state is saved using LangGraph’s [persistence layer](https://docs.langchain.com/oss/python/langgraph/persistence), so execution can pause safely and resume later.
* Using result.interrupts() we come to know if agent has finished or has it been interrupted for HITL
* **Decision Types:**
  *

      | Decision Type | Description                                                                                                     | Example Use Case                                  |
      | ------------- | --------------------------------------------------------------------------------------------------------------- | ------------------------------------------------- |
      | ✅ `approve`   | Execute the tool with the original arguments as proposed by the agent.                                          | Send an email draft exactly as written            |
      | ✏️ `edit`     | Modify the tool arguments before execution.                                                                     | Change the recipient before sending an email      |
      | ❌ `reject`    | Skip executing this tool call entirely and return rejection feedback to the agent.                              | Deny file deletion and explain why                |
      | 💬 `respond`  | Return the human’s message directly as a synthetic tool result, skipping execution, for “ask user” style tools. | Answer an `"ask_user"` prompt with a direct reply |
* When multiple tool calls are paused at the same time, each action requires a separate decision.
* Decisions must be provided in the same order as the actions appear in the interrupt request.

```python
from langchain.agents import create_agent
from langchain.agents.middleware import HumanInTheLoopMiddleware 
from langgraph.checkpoint.memory import InMemorySaver 


agent = create_agent(
    model="gpt-5.5",
    tools=[write_file, execute_sql, read_data],
    middleware=[
        HumanInTheLoopMiddleware(
            interrupt_on={
                "write_file": True,  # All decisions (approve, edit, reject, respond) allowed
                "execute_sql": {"allowed_decisions": ["approve", "reject"]},  # No editing allowed
                "read_data": False, # Safe operation, no approval needed
            },
            # Prefix for interrupt messages - combined with tool name and args to form the full message
            # e.g., "Tool execution pending approval: execute_sql with query='DELETE FROM...'"
            # Individual tools can override this by specifying a "description" in their interrupt config
            description_prefix="Tool execution pending approval",
        ),
    ],
    # Human-in-the-loop requires checkpointing to handle interrupts.
    # In production, use a persistent checkpointer like AsyncPostgresSaver or MongoDBSaver.
    checkpointer=InMemorySaver(),
)
```

**Conditional Interrupts:**

* To pause only some calls, add a `when` predicate to a tool’s `InterruptOnConfig`.&#x20;
* The predicate receives a `ToolCallRequest` and returns `True` to interrupt or `False` to auto-approve, so you can gate on the tool’s arguments.

```python
from langchain.agents import create_agent
from langchain.agents.middleware import HumanInTheLoopMiddleware, ToolCallRequest
from langgraph.checkpoint.memory import InMemorySaver


def writes_outside_workspace(request: ToolCallRequest) -> bool:
    """Pause writes to paths outside the workspace directory."""
    path = request.tool_call["args"].get("path", "")
    return not path.startswith("/workspace/")


def is_write_query(request: ToolCallRequest) -> bool:
    """Pause SQL that isn't a read-only SELECT."""
    query = request.tool_call["args"].get("query", "")
    return not query.lstrip().upper().startswith("SELECT")


agent = create_agent(
    model="gpt-5.5",
    tools=[write_file, execute_sql, read_data],
    middleware=[
        HumanInTheLoopMiddleware(
            interrupt_on={
                "write_file": {
                    "allowed_decisions": ["approve", "edit", "reject"],
                    "when": writes_outside_workspace,
                },
                "execute_sql": {
                    "allowed_decisions": ["approve", "reject"],
                    "when": is_write_query,
                },
            },
        ),
    ],
    checkpointer=InMemorySaver(),
)
```

**Responding to Interrupts:**

* Using interrupts which actions are pending
* We will again invoke the agent with the same thread it and pass the decision as Command

```python
from langgraph.types import Command

# Human-in-the-loop leverages LangGraph's persistence layer.
# You must provide a thread ID to associate the execution with a conversation thread,
# so the conversation can be paused and resumed (as is needed for human review).
config = {"configurable": {"thread_id": "some_id"}}
# Run the graph until the interrupt is hit.
result = agent.invoke(
    {
        "messages": [
            {
                "role": "user",
                "content": "Delete old records from the database",
            }
        ]
    },
    config=config,
    version="v2",
)

# result is a GraphOutput with .value and .interrupts
print(result.interrupts)
# > (
# >    Interrupt(
# >       value={
# >          'action_requests': [
# >             {
# >                'name': 'execute_sql',
# >                'arguments': {'query': 'DELETE FROM records WHERE created_at < NOW() - INTERVAL \'30 days\';'},
# >                'description': 'Tool execution pending approval\n\nTool: execute_sql\nArgs: {...}'
# >             }
# >          ],
# >          'review_configs': [
# >             {
# >                'action_name': 'execute_sql',
# >                'allowed_decisions': ['approve', 'reject']
# >             }
# >          ]
# >       }
# >    ),
# > )


# Resume with approval decision
agent.invoke(
    Command(
        resume={"decisions": [{"type": "approve"}]}  # or "reject"
    ),
    config=config, # Same thread ID to resume the paused conversation
    version="v2",
)
```

```python
agent.invoke(
    Command(
        # Decisions are provided as a list, one per action under review.
        # The order of decisions must match the order of actions
        # in the interrupt request.
        resume={
            "decisions": [
                {
                    "type": "respond",
                    # The human's reply, returned directly as the tool result
                    "message": "Blue.",
                }
            ]
        }
    ),
    config=config,  # Same thread ID to resume the paused conversation
    version="v2",
)
```
