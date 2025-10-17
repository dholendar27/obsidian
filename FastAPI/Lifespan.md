---
date created: 2025-10-17 14:23
---

In FastAPI, lifespan events are a mechanism that allows you to manage startup and shutdown behavior in a more structured and efficient way. The `lifespan` event is part of FastAPI's lifecycle and gives you a way to execute code at the beginning of the application's startup and at the end when it's shutting down.

### What is a Lifespan?

The `lifespan` event is triggered during the application's lifecycle. You can use it to handle tasks such as:

- **Startup:** Initialize connections, create resources, or load configurations.
- **Shutdown:** Close connections, clean up resources, or release other system resources.

### How does the `lifespan` work?

The `lifespan` is implemented using Python's asynchronous context manager (`async with`), which allows for better management of startup and shutdown tasks. It ensures that your application can perform necessary tasks before it starts serving requests and after it stops serving.

### The Structure of Lifespan

FastAPI provides a simple API to implement a lifespan event. Here's the basic structure of using the `lifespan` event in FastAPI:

1. **Startup:** This is where you'll put any code to initialize things you need, like setting up databases, starting background tasks, or opening connections.
2. **Shutdown:** This is where you'll put code to clean up and close things down, like closing database connections, stopping background tasks, or shutting down connections to external services.

```python
from fastapi import FastAPI
from typing import AsyncGenerator

# A simple lifespan event handler
async def lifespan(app: FastAPI) -> AsyncGenerator[None, None]:
    # Startup code (this runs when the app starts)
    print("Application is starting...")
    yield  # This is where FastAPI "yields control" to let the app run

    # Shutdown code (this runs when the app shuts down)
    print("Application is shutting down...")

# Create a FastAPI app instance and link the lifespan event handler
app = FastAPI(lifespan=lifespan)

@app.get("/")
def read_root():
    return {"message": "Hello, World!"}
```

### Key Components:

1. **`lifespan` function**: This function is a coroutine that must return an `AsyncGenerator`. It gets passed an instance of the FastAPI app when it is called. The code before the `yield` is the startup code, and the code after `yield` is the shutdown code.
2. **`yield` keyword**: This is the point where FastAPI starts serving requests. The code after `yield` will run when the app shuts down.

### Practical Use Cases for Lifespan Events

1. **Database Connections:**
   You might want to establish a connection to a database or a cache service when the application starts, and close the connection when it shuts down.
   ```python
   from fastapi import FastAPI
   from typing import AsyncGenerator
   import databases

   DATABASE_URL = "sqlite:///./test.db"

   # Async database connection
   database = databases.Database(DATABASE_URL)

   async def lifespan(app: FastAPI) -> AsyncGenerator[None, None]:
       # Startup: Connect to the database
       print("Connecting to the database...")
       await database.connect()
       yield
       # Shutdown: Disconnect from the database
       print("Disconnecting from the database...")
       await database.disconnect()

   app = FastAPI(lifespan=lifespan)

   @app.get("/")
   async def read_root():
       return {"message": "Hello, World!"}
   ```

2. **Logging Setup:**
   You could use `lifespan` to set up logging at startup and clean up resources (like log file handles) at shutdown.

3. **Background Tasks:**
   If your app has background tasks, you might want to ensure these tasks are started up when the application starts and gracefully shut down when the app stops.

   ```python
   from fastapi import FastAPI
   from typing import AsyncGenerator
   import asyncio

   async def background_task():
       while True:
           await asyncio.sleep(5)
           print("Running background task...")

   async def lifespan(app: FastAPI) -> AsyncGenerator[None, None]:
       # Startup: Start a background task
       task = asyncio.create_task(background_task())
       yield
       # Shutdown: Cancel the background task
       task.cancel()
       print("Background task stopped.")

   app = FastAPI(lifespan=lifespan)

   @app.get("/")
   def read_root():
       return {"message": "Hello, World!"}
   ```

4. **External Services/Queues:**\
   If your app interacts with external services, you might want to initialize and shut them down properly. For example, opening and closing connections to message queues or APIs.

### Things to Consider:

- **Async Support:** Lifespan events are asynchronous, which allows for non-blocking operations during startup and shutdown.

- **Exception Handling:** You might want to add exception handling within the `lifespan` function to ensure that if something goes wrong, your shutdown logic can still run, allowing for graceful shutdown.

### Integration with Uvicorn

FastAPI typically runs on an ASGI server like Uvicorn. When the app is started with Uvicorn, it will automatically manage the lifespan events by invoking the `lifespan` function during the app's lifecycle.

You would typically run the FastAPI app like this:

```bash
uvicorn app:app --reload
```

Where `app` is the name of your FastAPI instance.
