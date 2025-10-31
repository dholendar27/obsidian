
## 1. ORM Mapping Overview

ORM = **Object Relational Mapper** — it translates between:

- **Python objects** (User, Product, Order)
- **Database tables** (users, products, orders)

So instead of writing raw SQL like:

```sql
INSERT INTO users (name, email) VALUES ('Alice', 'alice@example.com');
```

We do:

```python
session.add(User(name="Alice", email="alice@example.com"))
session.commit()
```

SQLAlchemy handles the conversion automatically.

---

## 2. The Declarative Base (Classic Style)

Here’s how you define ORM models in SQLAlchemy 1.x/2.x (compatible way):

```python
from sqlalchemy import Column, Integer, String
from sqlalchemy.orm import declarative_base

Base = declarative_base()  # Base class for all models

class User(Base):
    __tablename__ = "users"  # Table name in the database

    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    email = Column(String(120), unique=True)
```

### What’s Happening Here

|Part|Meaning|
|---|---|
|`Base = declarative_base()`|Creates a base class to register all models|
|`__tablename__`|The actual table name in the database|
|`Column()`|Defines database columns|
|`primary_key=True`|Sets a column as the unique identifier|
|`unique=True`|Adds a uniqueness constraint|
|`String(120)`|Specifies column data type and max length|

---

## 3. Declarative Mapping (SQLAlchemy 2.0+)

In SQLAlchemy 2.0, there’s a **newer syntax** using **type hints** — cleaner and modern.

```python
from typing import Optional
from sqlalchemy import String, Integer
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column

class Base(DeclarativeBase):
    pass

class User(Base):
    __tablename__ = "users"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(50))
    email: Mapped[Optional[str]] = mapped_column(String(120), unique=True)
```

Type-safe  
IDEs can infer column types  
Cleaner and easier to maintain

---

## 4. Metadata and Table Creation

Every model you define is registered inside `Base.metadata`.

You can use it to **create all tables** in one command:

```python
from sqlalchemy import create_engine
engine = create_engine("sqlite:///example.db", echo=True)

Base.metadata.create_all(engine)
```

This scans all models that inherit from `Base` and creates corresponding tables in the database.

---

## 5. Accessing Table Metadata

You can introspect your model like this:

```python
print(User.__tablename__)          # "users"
print(User.__table__)              # Table('users', ...)
print(User.__table__.columns)      # list of columns
```

Example output:

```
users
Table('users', MetaData(), Column('id', INTEGER(), table=<users>, primary_key=True), ...)
```

---

## 6. Best Practices

|Practice|Description|
|---|---|
|Always define `__tablename__`|Keeps table names predictable|
|Use meaningful primary keys|Usually an `id` column|
|Keep names lowercase|Follows SQL conventions|
|Separate models into `models.py`|Keeps your code organized|
|Use SQLAlchemy 2.0 type hints for new projects|Cleaner, future-proof|

---
## 1. Defining Columns — Recap

Columns describe what each field in your model looks like in the database.

Example:

```python
from sqlalchemy import Column, Integer, String, Boolean, DateTime, func
from sqlalchemy.orm import declarative_base

Base = declarative_base()

class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True, autoincrement=True)
    name = Column(String(50), nullable=False)
    email = Column(String(100), unique=True, nullable=False)
    is_active = Column(Boolean, default=True)
    created_at = Column(DateTime(timezone=True), server_default=func.now())
```

---

## 2. Common SQLAlchemy Column Types

|Type|Description|Example|
|---|---|---|
|`Integer`|Integer numbers|`Column(Integer)`|
|`String(length)`|Text (varchar)|`Column(String(50))`|
|`Text`|Large text|`Column(Text)`|
|`Boolean`|True/False|`Column(Boolean)`|
|`Date`, `DateTime`|Dates/timestamps|`Column(DateTime)`|
|`Float`, `Numeric`|Decimal numbers|`Column(Float)`|
|`Enum`|Fixed set of values|`Column(Enum('A', 'B', 'C'))`|
|`JSON`|JSON objects (for supported DBs)|`Column(JSON)`|
|`LargeBinary`|Binary data|`Column(LargeBinary)`|

### Example:

```python
from sqlalchemy import Float, Text, Enum, JSON
import enum

class ProductType(enum.Enum):
    FOOD = "food"
    ELECTRONICS = "electronics"
    BOOKS = "books"

class Product(Base):
    __tablename__ = "products"

    id = Column(Integer, primary_key=True)
    name = Column(String(100), nullable=False)
    price = Column(Float, nullable=False)
    category = Column(Enum(ProductType), nullable=False)
    description = Column(Text)
    metadata = Column(JSON)  # optional metadata field
```

---

## 3. Constraints in Columns

### Primary Key

Marks a column as the unique identifier.

```python
id = Column(Integer, primary_key=True)
```

### Unique Constraint

Ensures no two rows have the same value.

```python
email = Column(String(120), unique=True)
```

### Nullable Constraint

Determines whether NULL is allowed.

```python
name = Column(String(50), nullable=False)
```

### Default Values

- **Python-side default:** applied by Python before insert
- **Database-side default:** handled by DB server (`server_default`)

```python
from sqlalchemy import func

is_active = Column(Boolean, default=True)  # Python default
created_at = Column(DateTime, server_default=func.now())  # DB default
```

---

## 4. Table-Level Constraints

Sometimes you need constraints involving multiple columns (like composite uniqueness or checks).

```python
from sqlalchemy import UniqueConstraint, CheckConstraint

class Employee(Base):
    __tablename__ = "employees"

    id = Column(Integer, primary_key=True)
    name = Column(String(50))
    age = Column(Integer)
    department = Column(String(50))

    __table_args__ = (
        UniqueConstraint('name', 'department', name='uix_name_department'),
        CheckConstraint('age >= 18', name='check_age_adult'),
    )
```

💡 **Why use them?**

- `UniqueConstraint` → ensures uniqueness across multiple fields
- `CheckConstraint` → enforces conditions at DB level (like `age >= 18`)

---

## 5. SQLAlchemy 2.0 Syntax with Type Hints

Here’s how this looks in the modern style:

```python
from sqlalchemy.orm import DeclarativeBase, Mapped, mapped_column
from sqlalchemy import String, Integer, Boolean, func, DateTime

class Base(DeclarativeBase):
    pass

class User(Base):
    __tablename__ = "users"

    id: Mapped[int] = mapped_column(primary_key=True)
    name: Mapped[str] = mapped_column(String(50), nullable=False)
    email: Mapped[str] = mapped_column(String(100), unique=True)
    is_active: Mapped[bool] = mapped_column(Boolean, default=True)
    created_at: Mapped = mapped_column(DateTime, server_default=func.now())
```

---

## 6. Default vs Server Default

| Type             | Applied By           | Example                                       | When to Use                                           |
| ---------------- | -------------------- | --------------------------------------------- | ----------------------------------------------------- |
| `default`        | Python before INSERT | `Column(Boolean, default=True)`               | When logic is handled in app                          |
| `server_default` | Database engine      | `Column(DateTime, server_default=func.now())` | When DB should handle it (timestamps, triggers, etc.) |
