# 🟢 Optional and Literal

* <mark style="color:purple;background-color:purple;">**`Optional`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**(or the modern**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**`| None`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**syntax) marks a value that might legitimately not exist yet**</mark>
* <mark style="color:$danger;background-color:purple;">**`Literal`**</mark><mark style="color:$danger;background-color:purple;">**&#x20;**</mark><mark style="color:$danger;background-color:purple;">**restricts a value to an exact, specific set of options**</mark>
* <mark style="color:purple;background-color:purple;">**This is not enforced!!**</mark>

```python
from typing import Optional, Literal

phone: str | None = None

# Literal — a strict multiple-choice constraint
status: Literal["draft", "published", "archived"] = "draft"
```
