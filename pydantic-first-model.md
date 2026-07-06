# Pydantic - First Model

* @dataclass does not validates — It accepts whatever has been passed
* BaseModel validates types, and raises a Validation Error if something is mismatch

```python
class UserModel(BaseModel):
    name: str
    email: str
    age: int

UserModel(name="Alice", email="alice@example.com", age="not a number")
# ValidationError: Input should be a valid integer,
```
