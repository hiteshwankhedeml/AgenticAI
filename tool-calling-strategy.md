# Tool Calling Strategy

* For models that don’t support native structured output, LangChain uses tool calling to achieve the same result.&#x20;
* This works with all models that support tool calling (most modern models).
* The `tool_message_content` parameter allows you to customize the message that appears in the conversation history when structured output is generated
* It will be useful as it will also go as an input to the LLM

```python
class ToolStrategy(Generic[SchemaT]):
    schema: type[SchemaT]
    tool_message_content: str | None
    handle_errors: Union[
        bool,
        str,
        type[Exception],
        tuple[type[Exception], ...],
        Callable[[Exception], str],
    ]
```
