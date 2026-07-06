# Pydantic - Serialization

`model_dump()` :&#x20;

* Returns a plain Python dictionary&#x20;
* Use it when inserting into a database, or passing to another function expecting a dict.

`model_dump_json()` :&#x20;

* Returns a JSON string directly&#x20;
* Use it for HTTP API responses or writing to a `.json`

```python
user = SignupForm(username="aditi28", email="aditi@example.com", age=28, newsletter_opt_in=True)

user.model_dump()
# {'username': 'aditi28', 'email': 'aditi@example.com', 'age': 28, 'newsletter_opt_in': True}

user.model_dump_json()
# '{"username":"aditi28","email":"aditi@example.com","age":28,"newsletter_opt_in":true}'

user.model_dump_json(indent=2)   # pretty-printed for humans/logs
```
