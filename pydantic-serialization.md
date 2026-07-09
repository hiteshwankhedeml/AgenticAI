# 🟢 Pydantic - Serialization

<mark style="color:purple;background-color:purple;">**`model_dump()`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**:**</mark>&#x20;

* <mark style="color:purple;background-color:purple;">**Returns a plain Python dictionary**</mark>&#x20;
* Use it when inserting into a database, or passing to another function expecting a dict.

<mark style="color:purple;background-color:purple;">**`model_dump_json()`**</mark><mark style="color:purple;background-color:purple;">**&#x20;**</mark><mark style="color:purple;background-color:purple;">**:**</mark>&#x20;

* <mark style="color:purple;background-color:purple;">**Returns a JSON string directly**</mark>&#x20;
* Use it for HTTP API responses or writing to a `.json`

```python
user = SignupForm(username="aditi28", email="aditi@example.com", age=28, newsletter_opt_in=True)

user.model_dump()
# {'username': 'aditi28', 'email': 'aditi@example.com', 'age': 28, 'newsletter_opt_in': True}

user.model_dump_json()
# '{"username":"aditi28","email":"aditi@example.com","age":28,"newsletter_opt_in":true}'

user.model_dump_json(indent=2)   # pretty-printed for humans/logs
```
