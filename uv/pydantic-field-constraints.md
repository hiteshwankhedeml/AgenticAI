# 🟢 Pydantic - Field Constraints

* <mark style="color:purple;background-color:purple;">**We can add field contraints by specify the range limits, regex and special data types like emailstr, httpurl etc**</mark>

Two styles:

* The direct style — `age: int = Field(gt=0, le=120)`
* `Annotated` style — `age: Annotated[int, Field(gt=0, le=120)]`
* Annotated separates "the type" from "the metadata about the type," which becomes valuable once you stack multiple pieces of metadata (constraints, descriptions, examples) on one field.
* Prefer `Annotated` in production code; both behave identically.
* We can also use regex — &#x20;
* ```
   portfolio_url: str = Field(pattern=r"^https?://.*")
  ```
* We can also use built in special types like EmailStr, HttpUrl
* ```
  from pydantic import BaseModel, EmailStr, HttpUrl, SecretStr
  ```
*

```python
from typing import Annotated
from pydantic import BaseModel, Field

# Direct style
class JobApplicationV1(BaseModel):
    full_name: str = Field(min_length=2, max_length=100)
    years_experience: int = Field(ge=0, le=50)
    portfolio_url: str = Field(pattern=r"^https?://.*")

# Annotated style — same behavior, more composable
class JobApplicationV2(BaseModel):
    full_name: Annotated[str, Field(min_length=2, max_length=100)]
    years_experience: Annotated[int, Field(ge=0, le=50)]
    email: Annotated[
        str,
        Field(description="Applicant's contact email", examples=["rohan@example.com"]),
    ]
```
