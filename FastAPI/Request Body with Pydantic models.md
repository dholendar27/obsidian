If we want to handle **request bodies** in a Python web framework like **FastAPI** using **Pydantic models**, 
### Example: FastAPI with Pydantic Request Body

```python
from fastapi import FastAPI
from pydantic import BaseModel

app = FastAPI()

# Define a Pydantic model to describe the request body
class User(BaseModel):
    name: str
    age: int
    email: str | None = None  # optional field

@app.post("/users/")
async def create_user(user: User):
    return {
        "message": "User created successfully",
        "user": user
    }
```

---

### Explanation

- `User` is a Pydantic model that defines the shape of the JSON data expected in the request body.
- When you send a POST request to `/users/` with JSON like:

```json
{
  "name": "Alice",
  "age": 30,
  "email": "alice@example.com"
}
```

- FastAPI automatically parses the JSON into a `User` instance and passes it to the `create_user` function.
- The response returns the user data back as confirmation.
