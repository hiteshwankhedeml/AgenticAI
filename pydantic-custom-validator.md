# Pydantic - Custom Validator

* **field\_validator:**&#x20;
  * per-field custom logic
  * `@field_validator`, which operates on exactly one field.
  * mode='after' — runs after Pydantic's normal type coercion&#x20;
  * mode = 'before' — 'runs on the raw input before any type coercion is attempted
  * `raise ValueError(...)` to reject the whole model with a clear message
* &#x20;**model\_validator:**&#x20;
  * cross-field rules
  * `model_validator` receives the entire model, after every individual field has already passed its own checks

```python
from pydantic import BaseModel, field_validator

class JobApplication(BaseModel):
    full_name: str
    email: str
    years_experience: int

    @field_validator("full_name", mode="after")
    @classmethod
    def normalize_name(cls, value: str) -> str:
        cleaned = value.strip()
        if not cleaned:
            raise ValueError("full_name cannot be empty")
        return cleaned.title()
```

```python
from pydantic import BaseModel, model_validator

class SignupForm(BaseModel):
    username: str
    password: str
    confirm_password: str

    @model_validator(mode="after")
    def passwords_must_match(self):
        if self.password != self.confirm_password:
            raise ValueError("password and confirm_password do not match")
        return self
```
