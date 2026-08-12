# AI Performance Troubleshoot

* We only have system message to provide to the user
* Rather than bulking the message, added system message, user message and AI message
* Using this AI will learn what is expected
* This is also called few shot prompting
* This is also like giving a short term memory
* This can also be passed as dictionary format

```python
messages = [
    {"role": "system", "content": "You are a poetry expert"},
    {"role": "user", "content": "Write a haiku about spring"},
    {"role": "assistant", "content": "Cherry blossoms bloom..."}
]
response = model.invoke(messages)
```
