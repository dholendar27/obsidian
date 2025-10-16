
## What is `asyncio`?

**`asyncio`** is a **Python standard library** module that provides **asynchronous I/O, event loop, coroutines**, and **concurrency tools**.

It's designed to write **single-threaded concurrent code** using **coroutines** (via `async`/`await` syntax), allowing you to handle **thousands of I/O-bound tasks** (like HTTP requests, database calls, or file I/O) concurrently **without using threads or processes**.

---

## Why use `asyncio`?

- Traditional I/O operations block the program (e.g., reading from a file, network request).
- In applications with **lots of I/O operations**, using threads or multiprocessing is heavy and inefficient.
- `asyncio` is **lightweight**, **efficient**, and works well for:
    - Web servers (e.g., `FastAPI`, `aiohttp`)
    - Web scrapers
    - Chatbots
    - Network services

---

## Key Concepts in `asyncio`

---

### 1. **Coroutines**

- Functions declared with `async def`.
- They are **awaitable**, meaning they can be paused and resumed.

```python
import asyncio

async def say_hello():
    print("Hello")
    await asyncio.sleep(1)  # Simulate I/O
    print("World")
```

---

### 2. **`await` keyword**

- Used to pause execution **until an awaitable is complete**.
- Only works inside `async def` functions.

```python
await asyncio.sleep(1)  # Waits 1 second without blocking the event loop
```

---

### 3. **Event Loop**

- The **core** of `asyncio`.
- Manages and runs all asynchronous tasks and callbacks.

```python
async def main():
    await say_hello()

asyncio.run(main())  # Starts the event loop
```

> `asyncio.run()` automatically creates and closes the event loop.

---

### 4. **Tasks**

- Used to **schedule coroutines concurrently**.

```python
async def task(name):
    print(f"{name} started")
    await asyncio.sleep(2)
    print(f"{name} finished")

async def main():
    t1 = asyncio.create_task(task("Task 1"))
    t2 = asyncio.create_task(task("Task 2"))
    
    await t1
    await t2

asyncio.run(main())
```

Output:

```
Task 1 started
Task 2 started
...2 seconds later...
Task 1 finished
Task 2 finished
```

Both tasks run concurrently!

---

### 5. **Gathering Coroutines**

`asyncio.gather()` is used to run multiple coroutines concurrently and wait for all of them to finish.

```python
async def main():
    await asyncio.gather(
        task("A"),
        task("B"),
        task("C"),
    )
```

---

### 6. **Handling Exceptions**

When using `asyncio.gather()`, you can choose how to handle exceptions.

```python
async def bad_task():
    raise ValueError("Oops!")

async def main():
    try:
        await asyncio.gather(
            task("Good Task"),
            bad_task(),
        )
    except Exception as e:
        print("Caught an error:", e)
```

---

### 7. **Real-world Example: Concurrent Requests**

```python
import asyncio
import aiohttp

async def fetch(url):
    async with aiohttp.ClientSession() as session:
        async with session.get(url) as response:
            return await response.text()

async def main():
    urls = ['https://example.com', 'https://httpbin.org/get']
    results = await asyncio.gather(*(fetch(url) for url in urls))
    for content in results:
        print(content[:100])

asyncio.run(main())
```

---

## Under the Hood

- `asyncio` uses **non-blocking I/O** and the **`selectors`** module.
- Coroutines are scheduled in the **event loop** which runs callbacks, I/O, and tasks.
- Async code doesn’t use threads or processes, but cooperatively yields control using `await`.

---

## When to Use `asyncio`

Use `asyncio` if:
- You're doing **a lot of I/O** (networking, file, DB).
- You need **many concurrent operations** in a single thread.
- Your logic is naturally event-driven (e.g., GUIs, servers).

Avoid `asyncio` for:

- **CPU-bound tasks** (use `concurrent.futures.ProcessPoolExecutor` instead).
- Very simple scripts — complexity might not be worth it.

---

## Testing Asynchronous Code

Use `pytest-asyncio`:

```python
import pytest
import asyncio

@pytest.mark.asyncio
async def test_say_hello():
    await say_hello()
```

---
## Summary

|Feature|Description|
|---|---|
|`async def`|Declares a coroutine|
|`await`|Waits for an async operation|
|`asyncio.run()`|Starts and manages the event loop|
|`create_task()`|Runs a coroutine concurrently|
|`gather()`|Waits for multiple coroutines|
|Event Loop|Core engine managing async tasks|
