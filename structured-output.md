# 🟢 Structured Output

* <mark style="color:purple;background-color:purple;">**To return data in a specific, predictable format**</mark>
* <mark style="color:purple;background-color:purple;">**Pass it in response\_format while creating the agent**</mark>
  * <mark style="color:purple;background-color:purple;">**`ToolStrategy[StructuredResponseT]`**</mark><mark style="color:purple;background-color:purple;">**: Uses tool calling for structured output**</mark>
  * <mark style="color:purple;background-color:purple;">**`ProviderStrategy[StructuredResponseT]`**</mark><mark style="color:purple;background-color:purple;">**:**</mark>&#x20;
    * <mark style="color:purple;background-color:purple;">**if the model and provider chosen supports native structured output**</mark>
  * <mark style="color:purple;background-color:purple;">**`type[StructuredResponseT]`**</mark><mark style="color:purple;background-color:purple;">**: Schema type - automatically selects best strategy based on model capabilities**</mark>
  * <mark style="color:purple;background-color:purple;">**`None`**</mark><mark style="color:purple;background-color:purple;">**: Structured output not explicitly requested**</mark>

```python
def create_agent(
    ...
    response_format: Union[
        ToolStrategy[StructuredResponseT],
        ProviderStrategy[StructuredResponseT],
        type[StructuredResponseT],
        None,
    ]
)
```

<mark style="color:purple;background-color:purple;">**Provider Stategy:**</mark>

* <mark style="color:purple;background-color:purple;">**LangChain automatically uses**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`ProviderStrategy`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**when you pass a schema type directly to**</mark> [<mark style="color:purple;background-color:purple;">**`create_agent.response_format`**</mark>](https://reference.langchain.com/python/langchain/agents/factory/create_agent)

```python
from pydantic import BaseModel, Field
from langchain.agents import create_agent


class ContactInfo(BaseModel):
    """Contact information for a person."""
    name: str = Field(description="The name of the person")
    email: str = Field(description="The email address of the person")
    phone: str = Field(description="The phone number of the person")

agent = create_agent(
    model="gpt-5.5",
    response_format=ContactInfo  # Auto-selects ProviderStrategy
)

result = agent.invoke({
    "messages": [{"role": "user", "content": "Extract contact info from: John Doe, john@example.com, (555) 123-4567"}]
})

print(result["structured_response"])
# ContactInfo(name='John Doe', email='john@example.com', phone='(555) 123-4567')
```

<mark style="color:purple;background-color:purple;">**Tool Calling Strategy:**</mark>

* <mark style="color:purple;background-color:purple;">**For models that don’t support native structured output, LangChain uses tool calling to achieve the same result.**</mark>
* <mark style="color:purple;background-color:purple;">**The**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`tool_message_content`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**parameter allows you to customize the message that appears in the conversation history when structured output is generated**</mark>

```python
from pydantic import BaseModel, Field
from typing import Literal
from langchain.agents import create_agent
from langchain.agents.structured_output import ToolStrategy


class MeetingAction(BaseModel):
    """Action items extracted from a meeting transcript."""
    task: str = Field(description="The specific task to be completed")
    assignee: str = Field(description="Person responsible for the task")
    priority: Literal["low", "medium", "high"] = Field(description="Priority level")

agent = create_agent(
    model="gpt-5.5",
    tools=[],
    response_format=ToolStrategy(
        schema=MeetingAction,
        tool_message_content="Action item captured and added to meeting notes!"
    )
)

agent.invoke({
    "messages": [{"role": "user", "content": "From our meeting: Sarah needs to update the project timeline as soon as possible"}]
})

================================ Human Message =================================

From our meeting: Sarah needs to update the project timeline as soon as possible
================================== Ai Message ==================================
Tool Calls:
  MeetingAction (call_1)
 Call ID: call_1
  Args:
    task: Update the project timeline
    assignee: Sarah
    priority: high
================================= Tool Message =================================
Name: MeetingAction

Action item captured and added to meeting notes!
```

<mark style="color:purple;background-color:purple;">**Error Handling:**</mark>

* <mark style="color:purple;background-color:purple;">**Models can make mistakes when generating structured output via tool calling.**</mark>&#x20;
* <mark style="color:purple;background-color:purple;">**LangChain provides intelligent retry mechanisms to handle these errors automatically.**</mark>
* <mark style="color:purple;background-color:purple;">**When a model incorrectly calls multiple structured output tools, the agent provides error feedback in a**</mark> [<mark style="color:purple;background-color:purple;">**`ToolMessage`**</mark>](https://reference.langchain.com/python/langchain-core/messages/tool/ToolMessage) <mark style="color:purple;background-color:purple;">**and prompts the model to retry**</mark>
