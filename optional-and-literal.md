# Optional and Literal

* `Optional` (or the modern `| None` syntax) marks a value that might legitimately not exist yet
* `Literal` restricts a value to an exact, specific set of options
* This is not enforced!!

```python
from typing import Optional, Literal

phone: str | None = None

# Literal — a strict multiple-choice constraint
status: Literal["draft", "published", "archived"] = "draft"
```
