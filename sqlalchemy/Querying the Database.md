
## 1. Basic Querying — ORM style

In modern SQLAlchemy (2.0+), you use the `select()` construct instead of `session.query()`.

```python
from sqlalchemy import select

stmt = select(User)
result = session.execute(stmt)
users = result.scalars().all()
```

- `select(User)` → builds `SELECT * FROM users`
- `session.execute(stmt)` → sends query to DB
- `result.scalars()` → extracts ORM objects (instead of Row tuples)
- `.all()` → returns all results as a list

✅ Equivalent shortcut (common for beginners):

```python
users = session.query(User).all()  # legacy style, still works
```

---

# 🧩 2. Filtering (WHERE Clause)

### Using `.filter()` (legacy)

```python
session.query(User).filter(User.name == "Alice").all()
```

### Modern style with `select()`:

```python
stmt = select(User).where(User.name == "Alice")
result = session.execute(stmt).scalars().all()
```

**Multiple conditions:**

```python
stmt = select(User).where(
    (User.age > 20) & (User.name.like("A%"))
)
```

✅ Operators you can use:

| Operation    | Example                           |
| ------------ | --------------------------------- |
| Equal        | `User.name == "Alice"`            |
| Not Equal    | `User.name != "Alice"`            |
| Greater/Less | `User.age > 18`, `User.age <= 30` |
| In List      | `User.id.in_([1, 2, 3])`          |
| Like         | `User.email.like("%gmail.com")`   |
| AND          | `(cond1 & cond2)`                 |
| OR           | `(cond1 \| cond2)`                |

---

# 3. **Selecting Specific Columns**

You can select individual columns or expressions.

```python
stmt = select(User.name, User.age)
for row in session.execute(stmt):
    print(row.name, row.age)
```

Or aggregate:

```python
from sqlalchemy import func

stmt = select(func.count(User.id))
total_users = session.execute(stmt).scalar_one()
```

---

# 4. **Ordering & Limiting**

```python
stmt = select(User).order_by(User.name.desc()).limit(5)
users = session.execute(stmt).scalars().all()
```

---

# 5. **Joins**

When you need data from related tables.

### Basic Join

```python
stmt = select(User, Address).join(Address)
```

This performs:

```sql
SELECT * FROM users JOIN addresses ON users.id = addresses.user_id
```

### Filtered Join

```python
stmt = select(User.name, Address.email).join(Address).where(User.name == "Alice")
```

### Outer Join

```python
stmt = select(User).outerjoin(Address)
```

---

# 6. **Relationships and Lazy Loading**

When you query `User` objects, SQLAlchemy can automatically fetch related `Address` rows when accessed (lazy loading):

```python
user = session.get(User, 1)
print(user.addresses)  # triggers SQL query for addresses
```

---

# 7. **Eager Loading (Optimize Performance)**

To avoid N+1 problems (too many small queries), you can fetch related data in one go.

```python
from sqlalchemy.orm import joinedload, selectinload

# joinedload → JOINs the table in the same query
stmt = select(User).options(joinedload(User.addresses))

# selectinload → fires separate SELECT but more efficient for many
stmt = select(User).options(selectinload(User.addresses))
```

---

# 8. **Aggregation & Grouping**

Example: count how many addresses per user.

```python
from sqlalchemy import func

stmt = (
    select(User.name, func.count(Address.id))
    .join(Address)
    .group_by(User.name)
)
for name, count in session.execute(stmt):
    print(name, count)
```

---

# 9. **Updating Data**

Two main ways:

### ORM Way

```python
user = session.get(User, 1)
user.name = "NewName"
session.commit()
```

### Bulk Update (Core Style)

```python
from sqlalchemy import update

stmt = update(User).where(User.name == "Alice").values(name="Alicia")
session.execute(stmt)
session.commit()
```

---

# 10. **Deleting Data**

### ORM Way

```python
user = session.get(User, 1)
session.delete(user)
session.commit()
```

### Core Way

```python
from sqlalchemy import delete

stmt = delete(User).where(User.id == 3)
session.execute(stmt)
session.commit()
```

---

# 11. **Querying Relationships**

If you have:

```python
class User(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True)
    name = Column(String)
    addresses = relationship("Address", back_populates="user")

class Address(Base):
    __tablename__ = "addresses"
    id = Column(Integer, primary_key=True)
    email = Column(String)
    user_id = Column(Integer, ForeignKey("users.id"))
    user = relationship("User", back_populates="addresses")
```

### Fetch a user and all their addresses:

```python
user = session.execute(
    select(User).options(selectinload(User.addresses)).where(User.id == 1)
).scalar_one()

for addr in user.addresses:
    print(addr.email)
```

### Filter users who have a Gmail address:

```python
stmt = (
    select(User)
    .join(User.addresses)
    .where(Address.email.like("%gmail.com"))
)
users = session.execute(stmt).scalars().all()
```

---

# 12. **Advanced Filtering with any() and has()**

**Example:** find users with at least one Gmail address.

```python
stmt = select(User).where(User.addresses.any(Address.email.like("%gmail.com")))
```

**Reverse:** find addresses belonging to a user named “Alice”.

```python
stmt = select(Address).where(Address.user.has(User.name == "Alice"))
```

---

# 13. **Counting & Pagination**

```python
total = session.scalar(select(func.count()).select_from(User))
users = session.execute(select(User).offset(10).limit(10)).scalars().all()
```

---

# 14. **Combining Filters & Logical Operators**

```python
from sqlalchemy import or_, and_

stmt = select(User).where(
    or_(
        User.name.like("%Bob%"),
        and_(User.age > 20, User.city == "NYC")
    )
)
```

---

# 15. **Subqueries & Aliases**

Example: select users who have more than 2 addresses.

```python
subq = (
    select(Address.user_id, func.count(Address.id).label("addr_count"))
    .group_by(Address.user_id)
    .subquery()
)

stmt = (
    select(User)
    .join(subq, subq.c.user_id == User.id)
    .where(subq.c.addr_count > 2)
)
```

---

# 16. **Common Query Patterns**

|Goal|Code|
|---|---|
|Get by primary key|`session.get(User, 1)`|
|Get first matching|`session.scalar(select(User).where(User.name=="Alice"))`|
|Get all|`session.scalars(select(User)).all()`|
|Count|`session.scalar(select(func.count(User.id)))`|
|Filter + join|`select(User).join(Address).where(Address.city=="London")`|
|Exists|`select(User).where(User.addresses.any())`|
