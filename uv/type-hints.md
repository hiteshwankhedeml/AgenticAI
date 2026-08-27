# 🟢 Type Hints

* <mark style="color:purple;background-color:purple;">**They tell Python and everyone reading the code about the data type of variable**</mark>
* <mark style="color:purple;background-color:purple;">**Python does not enforce type hints at runtime**</mark>
* <mark style="color:$danger;background-color:purple;">**variable\_name : data\_type = 'value'**</mark>

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
