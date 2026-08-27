# 🟢 Pydantic - First Model

* @dataclass does not validates — It accepts whatever has been passed
* <mark style="color:purple;background-color:purple;">**BaseModel validates types, and raises a Validation Error if something is mismatch**</mark>

```python
class UserModel(BaseModel):
    name: str
    email: str
    age: int

UserModel(name="Alice", email="alice@example.com", age="not a number")
# ValidationError: Input should be a valid integer,
```
