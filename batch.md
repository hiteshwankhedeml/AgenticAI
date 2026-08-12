# 🟢 Batch

* <mark style="color:purple;background-color:purple;">**If we call AI for every query it will take time as well as costly, as in each query we will have to pass system message and all**</mark>
* <mark style="color:purple;background-color:purple;">**Batch multiple queries in one**</mark>

```python
responses = model.batch([
    "Why do parrots have colorful feathers?",
    "How do airplanes fly?",
    "What is quantum computing?"
])
for response in responses:
    print(response)
```

