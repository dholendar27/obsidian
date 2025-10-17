## 1. `Depends`: Core of Dependency Injection in FastAPI

The `Depends()` function is how you declare a dependency in FastAPI.

### Basic Example

```python
from fastapi import FastAPI, Depends

app = FastAPI()

# A simple dependency
def common_parameters(q: str = None):
    return {"q": q}

# Inject it into an endpoint
@app.get("/items/")
def read_items(commons: dict = Depends(common_parameters)):
    return commons
```

### What happens:

- The function `common_parameters` runs before the `read_items` function.
    
- Its return value (`{"q": q}`) is injected as the `commons` argument to `read_items`.
    

### Dependencies can be:

- Functions (synchronous or asynchronous)
    
- Classes (used like singletons or factories)
    
- Other dependencies (chained)
    

---

## 2. Managing Reusable Logic (e.g., DB, Security)

Dependencies help reuse code like:

### Database Session (e.g., SQLAlchemy)

```python
from sqlalchemy.orm import Session
from fastapi import Depends

# Assume SessionLocal is a session factory
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()

@app.get("/users/")
def read_users(db: Session = Depends(get_db)):
    return db.query(User).all()
```

- `get_db()` yields a session.
    
- FastAPI automatically handles entering and exiting (cleanup).
    
- This is powerful with async frameworks and ORMs.
    

### Security (e.g., OAuth2, API Keys)

```python
from fastapi import Depends, HTTPException, status

def get_current_user(token: str = Depends(oauth2_scheme)):
    user = verify_token(token)
    if not user:
        raise HTTPException(status_code=401, detail="Invalid token")
    return user

@app.get("/me")
def read_me(current_user: User = Depends(get_current_user)):
    return current_user
```

- `get_current_user` depends on another dependency: `oauth2_scheme`.
    
- You can chain dependencies, making composable and reusable logic.
    

---

## 3. Dependency Scopes (Lifetimes)

FastAPI dependencies can have different lifecycles (scopes). You manage these mostly via `yield`, and FastAPI uses them to handle setup and teardown.

### Scopes:

|Scope|Description|
|---|---|
|Per-request (default)|A new instance is created for each request.|
|Singleton|One instance reused across all requests.|
|Session (custom)|Scoped to a database session or similar.|
|Background tasks / events|Useful for tasks outside of the request cycle.|

### How to Control Scope

By default, dependencies are per-request. You influence scope by how you define them.

#### `yield` dependencies are teardown-aware:

```python
from fastapi import Depends

def get_connection():
    conn = create_connection()
    try:
        yield conn
    finally:
        conn.close()
```

- This is commonly used for database connections, files, sessions, etc.
    

#### Singleton example (manually):

```python
class Settings:
    def __init__(self):
        print("Loading settings...")
        self.database_url = "sqlite:///..."

settings = Settings()

def get_settings():
    return settings  # returned without yield

@app.get("/config")
def read_config(config: Settings = Depends(get_settings)):
    return {"db_url": config.database_url}
```

- `settings` is loaded once; `get_settings` always returns the same object.

---

## Advanced Usage

### Dependencies depending on other dependencies

```python
def get_token_header(x_token: str = Header(...)):
    if x_token != "expected":
        raise HTTPException(400, detail="Invalid token")
    return x_token

def get_user(x_token: str = Depends(get_token_header)):
    return {"user": "admin"}
```

- `get_user` depends on `get_token_header`, creating a chain of dependencies.

### Use `Depends` in:

- Routes (`@app.get(...)`)
- Background tasks
- Middleware
- Event handlers (startup/shutdown)

---

## Summary

|Concept|Explanation|
|---|---|
|`Depends()`|Main way to inject dependencies (functions, classes)|
|Reusable logic|Database, auth, validation, logging|
|Scopes|Per-request (default), Singleton (manual), Session|
|`yield` dependencies|Great for managing setup/teardown|
|Nested/chained dependencies|One dependency can rely on another|
