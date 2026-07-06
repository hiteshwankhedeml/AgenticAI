# Type Hints

* They tell python and everyone reading the code about the data type of variable
* Python does not enforce type hints at runtime

```python
# The four basic types you'll use constantly
name: str = "Aditi"
age: int = 28
price: float = 499.99
is_active: bool = True

# Container types
tags: list[str] = ["python", "pydantic", "fastapi"]
word_counts: dict[str, int] = {"error": 12, "warning": 5}
```
