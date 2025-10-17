---
date created: 2025-10-16 18:11
---

### 1. **Importing FastAPI**

```python
from fastapi import FastAPI
```

- This line imports the `FastAPI` class from the `fastapi` module.
- `FastAPI` is a modern, high-performance web framework for building APIs with Python 3.7+.
- It is built on top of Starlette (for web handling) and Pydantic (for data validation), and it leverages Python type hints.

---

### 2. **Creating a FastAPI App**

```python
app = FastAPI()
```

- This creates an instance of the `FastAPI` class and stores it in the variable `app`.
- This instance is used to define your API routes and configuration.
- When the application is run using a server like `uvicorn`, this is the object that gets served.

---

### 3. **Root Endpoint**

```python
@app.get("/")
def read_root():
    return {"message": "Hello, FastAPI!"}
```

**Explanation:**

- `@app.get("/")`: This decorator registers the function below it as a handler for GET requests to the root URL (`/`).
- `read_root()`: This is the function that will be executed when the root URL is accessed with a GET request.
- `return {"message": "Hello, FastAPI!"}`: Returns a dictionary, which FastAPI converts to a JSON response.

**Example Request:**

```
GET http://localhost:8000/
```

**Example Response:**

```json
{
  "message": "Hello, FastAPI!"
}
```

---

### 4. **Path Parameter and Query Parameter Endpoint**

```python
@app.get("/items/{item_id}")
def read_item(item_id: int, q: str = None):
    return {"item_id": item_id, "q": q}
```

**Explanation:**

- `@app.get("/items/{item_id}")`: Registers a GET route where `item_id` is a variable path parameter.
- `item_id: int`: This path parameter is expected to be an integer. FastAPI will validate and convert the URL value to an integer automatically.
- `q: str = None`: This is an optional query parameter. If not provided, it defaults to `None`. 
	Example: `/path?key=value`

**Example Request:**

```
GET http://localhost:8000/items/42?q=example
```

**Parsed Values:**

- `item_id`: 42 (from the URL path)
- `q`: "example" (from the query string)

**Example Response:**

```json
{
  "item_id": 42,
  "q": "example"
}
```

---

### Key Features Demonstrated:

| Feature                       | Description                                                                             |
| ----------------------------- | --------------------------------------------------------------------------------------- |
| Type hints                    | Used for automatic data validation and documentation generation.                        |
| Path parameters               | Parameters defined directly in the URL path (e.g., `item_id`).                          |
| Query parameters              | Optional parameters provided after the `?` in the URL.                                  |
| Automatic response conversion | Python dictionaries are automatically converted to JSON.                                |
| Auto-generated docs           | FastAPI creates interactive documentation at `/docs` (Swagger UI) and `/redoc` (ReDoc). |

---

### Running the Code

To run this FastAPI app, use the following command in the terminal:

```bash
uvicorn filename:app --reload
```

- Replace `filename` with the name of your Python file, for example, `main.py`.
- `--reload` enables hot-reloading during development, so changes to the code will automatically restart the server.