---
date created: 2025-10-31 10:31
date updated: 2025-10-31 14:07
---

## 1.1 — Installation and Setup

---

## Step 1: Install SQLAlchemy

Once inside your environment:

```bash
pip install sqlalchemy
```

To verify:

```bash
python -m sqlalchemy --version
```

You’ll see something like:

```
SQLAlchemy 2.0.25
```

---

## Step 4: Connect to a Database

Let’s use **SQLite**, since it doesn’t need a server — perfect for learning.

Inside `main.py`:

```python
from sqlalchemy import create_engine

# SQLite database file (created automatically)
DATABASE_URL = "sqlite:///example.db"

# Create the engine (this manages DB connections)
engine = create_engine(DATABASE_URL, echo=True)

print("Database engine created successfully!")
```

Run it:

```bash
python main.py
```

You’ll see console logs like:

```
Database engine created successfully!
```

…and an `example.db` file in your folder (SQLite database file).

---

## Quick Explanation

| Concept                    | Description                                                                                       |
| -------------------------- | ------------------------------------------------------------------------------------------------- |
| **Engine**                 | The _core_ of SQLAlchemy — it manages connections and talks directly to the database.             |
| **`create_engine()`**      | Creates a ==factory== that gives SQLAlchemy access to your database.                              |
| **`echo=True`**            | Logs all SQL statements — super useful while learning and debugging.                              |
| **`sqlite:///example.db`** | This is your connection URL — different databases have different formats (we’ll learn them soon). |

---

## Example URLs for Other Databases

| Database           | Example Connection URL                               |
| ------------------ | ---------------------------------------------------- |
| PostgreSQL         | `postgresql+psycopg2://user:password@localhost/mydb` |
| MySQL              | `mysql+pymysql://user:password@localhost/mydb`       |
| SQLite (file)      | `sqlite:///example.db`                               |
| SQLite (in-memory) | `sqlite:///:memory:`                                 |

SQLAlchemy **does not actually create the database file** when you call:

```python
engine = create_engine(url)
```

That only creates the **Engine object**, which is a _configuration_ — not an active connection.\
The database file (`example.db`) is created **only when you first execute an actual SQL command** (for example, when a connection is opened or a table is created).

```
+--------------------------------------------+
| 1️ You call create_engine()                |
+--------------------------------------------+
       |
       v
	Engine object created
	(contains DB URL, dialect info, connection pool setup)
	but no actual DB connection yet.

```

---

## Core vs ORM Layer

SQLAlchemy is designed with two powerful layers:

![[ORM vs Core.png]]

## The Core Layer

The **Core** is SQLAlchemy’s _foundation_.\
It’s a **SQL abstraction toolkit**, not just an ORM backend.

It lets you:

- Define tables programmatically
- Write SQL expressions in Python
- Execute them efficiently

Essentially, you get **full SQL control**, but without writing raw strings.

```python
from sqlalchemy import create_engine, MetaData, Table, Column, Integer, String

# Setup
engine = create_engine("sqlite:///core_example.db", echo=True)
metadata = MetaData()

# Define a table
users = Table(
    "users",
    metadata,
    Column("id", Integer, primary_key=True),
    Column("name", String),
    Column("email", String, unique=True),
)

# Create the table in the DB
metadata.create_all(engine)

# Insert data
with engine.connect() as conn:
    conn.execute(users.insert().values(name="Alice", email="alice@example.com"))
    conn.commit()
```

**What’s happening here:**

- You’re manually defining tables and columns.
- No classes or ORM models — just pure schema and SQL.
- You control the _exact SQL operations_ that happen.

**Best for:**

- Scripting, migrations, admin tools, or when you need fine SQL control.

## The ORM Layer

The **ORM** (Object Relational Mapper) sits _on top_ of the Core.\
It lets you work with **Python classes and objects instead of SQL tables and rows.**

Instead of thinking about SQL, you think about **Python objects** that map to database rows.

```python
from sqlalchemy import create_engine, Column, Integer, String
from sqlalchemy.orm import declarative_base, Session

# Setup
engine = create_engine("sqlite:///orm_example.db", echo=True)
Base = declarative_base()

# Define ORM model
class User(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True)
    name = Column(String)
    email = Column(String, unique=True)

# Create tables
Base.metadata.create_all(engine)

# Use ORM session to insert data
with Session(engine) as session:
    user = User(name="Alice", email="alice@example.com")
    session.add(user)
    session.commit()

```

**What’s happening here:**

- You define a `User` class → SQLAlchemy maps it to a table (`users`).
- ORM automatically translates `session.add(user)` into an `INSERT` SQL command.
- You work in Python objects; ORM handles SQL generation and execution.

**Best for:**

- Full backend applications (Flask/FastAPI/Django style)
- Clean domain models
- Business logic tied to database entities

### 1. ORM Layer (Top)

- You write **Python classes** and **objects** (`User`, `Post`, etc.).
- You call methods like `session.query(User).filter(User.id == 1)`.
- The ORM takes care of mapping your Python logic into database instructions.\
  **Goal:** Make database work feel Pythonic — no need to hand-write SQL.

---

### 2. Core Layer (Middle)

- This is the **SQL Expression Language** — the “translator” between ORM and raw SQL.
- It constructs **SQL statements** safely and efficiently (like `SELECT users.id FROM users WHERE ...`).
- You can use it directly if you want fine-grained control.\
  **Goal:** Provide an abstract but SQL-aware language that ORM and advanced users rely on.

---

### 3. Database Driver (Bottom)

- The **driver** (e.g., `sqlite3`, `psycopg2`, `mysqlclient`) handles the **actual communication** with the database.
- SQLAlchemy sends the **generated SQL** to the driver, which executes it on the real database.\
  **Goal:** Handle connections, execute queries, and return results to SQLAlchemy

---

## 1. The Engine, Connection, and Session

```
Python Code  →  ORM  →  Session  →  Engine  →  Connection  →  Database
```

**Engine** → database factory; knows _how to connect_ (you created this earlier).
**Connection** → actual active DB connection; low-level, short-lived.
**Session** → ORM interface that uses connection(s) under the hood to:

  - Track object changes
  - Queue INSERT/UPDATE/DELETEs
  - Handle transactions (commit/rollback)

---

## 2. Creating a Session (Modern Style)

Here’s how we do it with the latest SQLAlchemy 2.x pattern:

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import Session

# Step 1: Create engine
engine = create_engine("sqlite:///example.db", echo=True)

# Step 2: Create a session
with Session(engine) as session:
    # perform operations here
    result = session.execute("SELECT 'Hello SQLAlchemy!'")
    print(result.scalar())
```

`with Session(engine)` automatically handles closing and cleanup.\
Inside the `with` block, the session is **active and bound** to the engine.

---

## 3. The ORM Session in Action

Let’s create a simple example using ORM models:

```python
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import declarative_base, Session

Base = declarative_base()

class User(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True)
    name = Column(String)

engine = create_engine("sqlite:///example.db", echo=True)
Base.metadata.create_all(engine)  # create tables

# Session usage
with Session(engine) as session:
    new_user = User(name="Alice")
    session.add(new_user)
    session.commit()  # COMMIT triggers INSERT
```

**Behind the scenes:**

- The ORM tracks `new_user`.
- On `commit()`, SQLAlchemy sends an `INSERT` to the database.
- Then it clears the pending transaction and closes automatically (thanks to the `with` block).

---

## Common Pitfalls

1. Creating sessions but never closing them (causes memory/connection leaks).\
   Always use `with` or `try/finally` to manage lifecycle.
2. Using one global session everywhere (causes data mix-ups).\
   Use **Session factories** (we’ll see this soon) for isolated requests.
3. Forgetting to `commit()` after adding or updating objects.\
   SQLAlchemy uses **transactional semantics** — nothing is saved until commit.

---
## Session
## 1. The `sessionmaker` Factory

we created sessions manually like:

```python
with Session(engine) as session:
```

That’s fine for scripts, but in **real applications** (like FastAPI or Flask backends), we often need to create sessions repeatedly — for each request or background job.

That’s where **`sessionmaker`** comes in — it’s a **factory** that creates new session instances bound to an engine.

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker

engine = create_engine("sqlite:///example.db", echo=True)

# Create a session factory
SessionLocal = sessionmaker(bind=engine)

# Create a session
session = SessionLocal()
```

Now `SessionLocal()` returns a **new independent session** every time.  
This makes it safe to use in multi-threaded or multi-request environments.

---

## 2. Typical Pattern in Real Projects

You’ll often see something like this in backend apps:

```python
from sqlalchemy import create_engine
from sqlalchemy.orm import sessionmaker, declarative_base

DATABASE_URL = "sqlite:///example.db"

engine = create_engine(DATABASE_URL, echo=True)
SessionLocal = sessionmaker(autocommit=False, autoflush=False, bind=engine)

Base = declarative_base()
```

and then in your app code:

```python
def get_db():
    db = SessionLocal()
    try:
        yield db
    finally:
        db.close()
```

 In **FastAPI**, this `get_db()` is used as a dependency so that each request gets its own database session — safe and isolated.

---

## 3. Session Lifecycle: Commit, Rollback, and Flush

Let’s explore what really happens when you interact with the session.

```python
with Session(engine) as session:
    new_user = User(name="Alice")
    session.add(new_user)

    # No SQL yet — only pending in memory
    session.flush()   # Now SQLAlchemy sends INSERT to DB (but not committed)
    
    session.commit()  # Commit makes it permanent in the database
```

### Breakdown:

- `add()` → adds to pending state.
- `flush()` → sends SQL to DB but still inside transaction.
- `commit()` → commits the transaction and ends it.
- `rollback()` → cancels the transaction, undoing uncommitted changes.

If any error occurs (like a constraint violation), **you must rollback** before making new queries with the same session.

---

## 4. Thread Safety & Scoped Sessions

In multithreaded environments (like web servers), you shouldn’t share one session globally.  
Instead, you use **scoped sessions**:

```python
from sqlalchemy.orm import scoped_session, sessionmaker

SessionLocal = scoped_session(sessionmaker(bind=engine))
```

Each thread/request gets its **own session**, safely isolated.  
You can later call `SessionLocal.remove()` to clean up after the request.