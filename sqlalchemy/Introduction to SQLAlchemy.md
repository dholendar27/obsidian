---
date created: 2025-10-31 10:31
date updated: 2025-10-31 12:01
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

The **Core** is SQLAlchemy’s _foundation_.  
It’s a **SQL abstraction toolkit**, not just an ORM backend.

It lets you:

- Define tables programmatically
- Write SQL expressions in Python
- Execute them efficiently

Essentially, you get **full SQL control**, but without writing raw strings.