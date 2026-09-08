# 🟢 AI Performance Troubleshoot

* <mark style="color:purple;background-color:purple;">**We only have system message to provide to the user**</mark>
* <mark style="color:red;background-color:purple;">**Rather than bulking the message, added system message, user message and AI message**</mark>
* <mark style="color:purple;background-color:purple;">**Using this AI will learn what is expected**</mark>
* <mark style="color:purple;background-color:purple;">**This is also called few shot prompting**</mark>
* <mark style="color:purple;background-color:purple;">**This is also like giving a short term memory**</mark>
* <mark style="color:purple;background-color:purple;">**This can also be passed as dictionary format**</mark>

```python
messages = [
    {"role": "system", "content": "You are a poetry expert"},
    {"role": "user", "content": "Write a haiku about spring"},
    {"role": "assistant", "content": "Cherry blossoms bloom..."}
]
response = model.invoke(messages)
```
