# 🟢 Pydantic - Default value

* <mark style="color:purple;background-color:purple;">**Inside the BaseModel, along with the data type we can specify the default value as well**</mark>

```python
from pydantic import BaseModel, ValidationError

class SignupForm(BaseModel):
    username: str
    email: str
    age: int
    newsletter_opt_in: bool = False   # has a default -> optional

# Two equivalent creation styles
user_a = SignupForm(**{"username": "aditi28", "email": "a@x.com", "age": 28})
```
