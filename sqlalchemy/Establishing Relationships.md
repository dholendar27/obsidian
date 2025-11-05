## 1.  Foreign Key Constraint?

A **foreign key** establishes a **link between two tables** — it ensures that the value in one table corresponds to a value in another.  
This enforces **referential integrity**: you can’t insert a record with an invalid reference or delete a parent record that’s still being referenced (depending on the constraint).

Example in SQL:

```sql
FOREIGN KEY (user_id) REFERENCES users(id)
```

---

### 2. In SQLAlchemy ORM

In **SQLAlchemy ORM**, foreign keys are defined using the `ForeignKey` class and relationships via `relationship()`.

### Example:

```python
from sqlalchemy import Column, Integer, String, ForeignKey
from sqlalchemy.orm import relationship, declarative_base

Base = declarative_base()

class User(Base):
    __tablename__ = 'users'

    id = Column(Integer, primary_key=True)
    name = Column(String)

    # Establish relationship
    addresses = relationship("Address", back_populates="user")


class Address(Base):
    __tablename__ = 'addresses'

    id = Column(Integer, primary_key=True)
    email = Column(String, nullable=False)
    user_id = Column(Integer, ForeignKey('users.id', ondelete="CASCADE"))

    # Establish relationship back to User
    user = relationship("User", back_populates="addresses")
```

---

### What’s Happening:

- `ForeignKey('users.id')` → tells SQLAlchemy this column references `users.id`. Where we are using the `tablename.columnname`.
- `relationship()` → creates an ORM-level connection.
- `back_populates` → allows ==bidirectional traversal== between `User` and `Address`.

---

### **3. Cascade Options for Foreign Keys**

You can control what happens when a parent record is deleted or updated:

|Option|Effect|
|---|---|
|`ondelete="CASCADE"`|Delete all child records automatically.|
|`ondelete="SET NULL"`|Set foreign key to NULL when parent deleted.|
|`ondelete="RESTRICT"`|Prevent parent deletion if children exist.|
|`ondelete="NO ACTION"`|Default behavior (may depend on DB).|

> [!note] Note
> Note: To make `ondelete` work, your DB engine must support it, and you may need to add `passive_deletes=True` in the relationship for ORM-level cascade syncing.
> 

Example:

```python
user_id = Column(Integer, ForeignKey('users.id', ondelete='CASCADE'))
user = relationship("User", back_populates="addresses", passive_deletes=True)
```

---

### 4. Foreign Keys in SQLAlchemy Core (without ORM)

If you’re not using ORM, you can define foreign keys directly on `Table` objects:

```python
from sqlalchemy import Table, Column, Integer, String, MetaData, ForeignKey

metadata = MetaData()

users = Table(
    'users', metadata,
    Column('id', Integer, primary_key=True),
    Column('name', String)
)

addresses = Table(
    'addresses', metadata,
    Column('id', Integer, primary_key=True),
    Column('email', String, nullable=False),
    Column('user_id', Integer, ForeignKey('users.id'))
)
```

---

### 5. Composite Foreign Keys

You can create a foreign key that references multiple columns:

```python
ForeignKeyConstraint(['user_id', 'address_id'], ['users.id', 'addresses.id'])
```

Useful in **many-to-many** or **join tables**.

---

### 6. Checking Relationships

You can navigate ORM relationships seamlessly:

```python
u1 = User(name="Alice")
a1 = Address(email="alice@example.com", user=u1)

session.add(u1)
session.add(a1)
session.commit()

print(u1.addresses)  # [Address(...)]
print(a1.user.name)  # "Alice"
```

---

## 7. Common Pitfalls

|Issue|Solution|
|---|---|
|Missing relationship or foreign key|Always define both sides explicitly.|
|Foreign key not working with SQLite|SQLite ignores some `ondelete` actions — handle manually or use ORM cascades.|
|Circular imports|Use string-based references: `'User'` instead of direct class reference.|

---

## What Are Relationship Types?

A **relationship** describes **how records in one table relate to records in another table**.

In SQLAlchemy (and databases in general), there are **three major types**:

| Type                     | Meaning                                      | Example                                                                           |
| ------------------------ | -------------------------------------------- | --------------------------------------------------------------------------------- |
| **One-to-Many (1 → ∞)**  | One record relates to many in another table  | One `User` has many `Posts`                                                       |
| **Many-to-One (∞ → 1)**  | Many records relate to one in another table  | Many `Posts` belong to one `User`                                                 |
| **Many-to-Many (∞ ↔ ∞)** | Many records relate to many in another table | A `Student` can enroll in many `Courses`, and a `Course` can have many `Students` |

---

### 1. One-to-Many Relationship

#### Real-World Example:

A single **user** can have **many addresses**.

#### Database View:

- **Parent Table**: `users`
- **Child Table**: `addresses`
- The `addresses` table has a **foreign key** pointing to `users.id`.

#### Diagram:

```
users
 ├── id = 1 (Alice)
 │    ├── address_id = 101 ("NYC")
 │    ├── address_id = 102 ("LA")
 └── id = 2 (Bob)
      ├── address_id = 103 ("Paris")
```

#### SQLAlchemy Example:

```python
from sqlalchemy import Column, Integer, String, ForeignKey
from sqlalchemy.orm import relationship, declarative_base

Base = declarative_base()

class User(Base):
    __tablename__ = "users"

    id = Column(Integer, primary_key=True)
    name = Column(String)

    # One-to-Many: One user → many addresses
    addresses = relationship("Address", back_populates="user")


class Address(Base):
    __tablename__ = "addresses"

    id = Column(Integer, primary_key=True)
    email = Column(String)
    user_id = Column(Integer, ForeignKey("users.id"))

    # Many-to-One side of the relationship
    user = relationship("User", back_populates="addresses")
```

#### How it works:

- `Address.user_id` → Foreign Key (`addresses → users`)
- `User.addresses` → List of Address objects
- `Address.user` → A single User object

#### Example usage:

```python
u1 = User(name="Alice")
u1.addresses = [
    Address(email="alice@home.com"),
    Address(email="alice@work.com")
]

session.add(u1)
session.commit()

print(u1.addresses)   # [Address(...), Address(...)]
print(u1.addresses[0].user.name)  # Alice
```

##### Summary:

> “One user → many addresses”  
> “Each address → belongs to one user”

---

### 2. Many-to-One Relationship

This is **just the inverse side** of One-to-Many.

- Many `addresses` belong to **one** `user`.
- In ORM, it’s typically represented by the same code as above — just from the _child table’s perspective_.

In short:

```python
Address.user   # Many-to-One
User.addresses # One-to-Many
```

You don’t define these separately — they’re two sides of the same relationship, linked using `back_populates`.

---

###  3. Many-to-Many Relationship

#### Real-World Example:

A **student** can enroll in **many courses**,  
and a **course** can have **many students**.

We can’t just store this with a single foreign key —  
we need a **third table** (called an **association table** or **junction table**) that connects them.

---

#### Database Design:

```
students
 ├── id = 1 (Alice)
 └── id = 2 (Bob)

courses
 ├── id = 10 (Math)
 └── id = 11 (Physics)

association_table (enrollments)
 ├── student_id = 1, course_id = 10
 ├── student_id = 1, course_id = 11
 ├── student_id = 2, course_id = 10
```

---

#### SQLAlchemy Example:

```python
from sqlalchemy import Table, Column, Integer, ForeignKey
from sqlalchemy.orm import relationship, declarative_base

Base = declarative_base()

# Association (bridge) table — no model class needed
student_course = Table(
    'student_course', Base.metadata,
    Column('student_id', Integer, ForeignKey('students.id')),
    Column('course_id', Integer, ForeignKey('courses.id'))
)

class Student(Base):
    __tablename__ = 'students'

    id = Column(Integer, primary_key=True)
    name = Column(String)

    # Many-to-Many relationship
    courses = relationship(
        "Course",
        secondary=student_course,
        back_populates="students"
    )


class Course(Base):
    __tablename__ = 'courses'

    id = Column(Integer, primary_key=True)
    title = Column(String)

    students = relationship(
        "Student",
        secondary=student_course,
        back_populates="courses"
    )
```

---

#### Usage:

```python
s1 = Student(name="Alice")
s2 = Student(name="Bob")
c1 = Course(title="Math")
c2 = Course(title="Physics")

s1.courses = [c1, c2]
s2.courses = [c1]

session.add_all([s1, s2])
session.commit()

print(s1.courses)  # [Math, Physics]
print(c1.students) # [Alice, Bob]
```
 
 **Summary:**

> “Each student can take many courses.”  
> “Each course can have many students.”  
> The link between them is stored in a **third table**.

|Confusion|Clarification|
|---|---|
|“Do I need both One-to-Many and Many-to-One?”|They’re two sides of the same coin — define one with `back_populates` for bidirectional traversal.|
|“Why Many-to-Many needs an extra table?”|Because you can’t store multiple foreign keys in a single column — you need a junction table to link both sides.|
|“Can I access both sides?”|Yes — if you use `back_populates` or `backref`.|
|“Does relationship() create a foreign key?”|No, it just _uses_ existing foreign keys to map Python objects.|

---
### What is `backref`?

In SQLAlchemy, the `backref` parameter is a **shortcut** to automatically create a **bidirectional relationship** between two models — meaning you can navigate in both directions **without explicitly declaring `relationship()` on both sides**.

So instead of defining:

```python
relationship(..., back_populates=...)
```

on _both_ models, you can define it **once** using `backref`, and SQLAlchemy automatically sets up the reverse side for you.

---

#### Example: Without `backref` (Using `back_populates`)

Let’s start with the _explicit way_ first.

```python
from sqlalchemy import Column, Integer, String, ForeignKey
from sqlalchemy.orm import relationship, declarative_base

Base = declarative_base()

class User(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True)
    name = Column(String)

    posts = relationship("Post", back_populates="author")

class Post(Base):
    __tablename__ = "posts"
    id = Column(Integer, primary_key=True)
    title = Column(String)
    user_id = Column(Integer, ForeignKey("users.id"))

    author = relationship("User", back_populates="posts")
```

Here:

- `User.posts` links to many `Post` objects.
- `Post.author` links back to the `User`.
- Both sides explicitly reference each other using `back_populates`.

#### Example: With `backref`

Now, let’s simplify it using `backref`.

```python
from sqlalchemy.orm import relationship, backref

class User(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True)
    name = Column(String)

    posts = relationship("Post", backref=backref("author"))
```

That’s it — you no longer need to define `relationship()` on the `Post` model!

Here’s what happens automatically:

- On the `User` side → `posts` gives you all posts written by the user.
- On the `Post` side → an **implicit** `author` attribute is created for you.

```python
# Usage example
user = User(name="Alice")
post = Post(title="Hello World")

user.posts.append(post)

print(post.author.name)  # Alice
print(user.posts[0].title)  # Hello World
```

**Bidirectional traversal achieved automatically via backref.**

---
#### How It Works Internally

When you use:

```python
relationship("Post", backref="author")
```

SQLAlchemy:

- Defines a relationship `User.posts` → `Post` (one-to-many)
- Automatically defines the reverse relationship `Post.author` → `User`

This allows both sides to stay in sync:

```python
post.author = user
# user.posts now automatically contains 'post'
```

---

#### `backref()` vs. `backref="name"`

You can use the simple string form:

```python
relationship("Post", backref="author")
```

Or the function form for more control:

```python
from sqlalchemy.orm import backref

relationship(
    "Post",
    backref=backref("author", cascade="all, delete-orphan")
)
```

This lets you customize the reverse relationship (e.g., cascading behavior).

---

#### When to Use `backref` vs. `back_populates`

|Feature|`backref`|`back_populates`|
|---|---|---|
|Setup|One-line shortcut|Explicit on both sides|
|Control|Less control|More flexible|
|Readability|Cleaner for simple relations|Better for complex models|
|Recommended for|Simple one-to-many or many-to-one|Large schemas and advanced mappings|

---
## Cascades: two different layers

1. **ORM-level cascade** — set on `relationship(..., cascade="...")`. Controls what SQLAlchemy **Session/ORM** does to related objects when you add/merge/delete/expunge/expire objects.
2. **DB-level cascade** — set on the foreign key with `ForeignKey(..., ondelete="CASCADE")`. Controls what the **database** does when a parent row is deleted/updated.

They’re related but different. If you want the DB to delete rows (even if someone runs SQL directly), use `ondelete`. If you want SQLAlchemy to delete or attach objects automatically in your Python code, use `cascade`.

---

# Available ORM cascade options (names must be exact)

- `save-update` — when a parent is `Session.add()`ed, associated objects are also added to the session. (Default includes this.)
- `merge` — when `Session.merge()` is called, related objects are merged too.
- `expunge` — when parent is removed from the session (`Session.expunge()`), related objects are also expunged.
- `delete` — when parent is deleted via `Session.delete()`, related objects are marked for deletion as well.
- `delete-orphan` — if a child is de-associated from its parent collection (removed from list/set), the child is marked for deletion. Often combined with `all`.
- `refresh-expire` — related objects are expired when the parent is refreshed/expired.
- `all` — shorthand for `save-update, merge, refresh-expire, expunge, delete`.
- `none` — disables all cascade behavior (rare).

**Default cascade on `relationship()` is**: `save-update, merge`.  
(So by default, adding a parent to a session will add new children, but deleting a parent will NOT delete children automatically.)

---

### Typical patterns & when to use them

#### 1) Default (no explicit cascade)

```py
addresses = relationship("Address")
# default cascade: "save-update, merge"
```

- Adding the `User` with newly created `Address` objects will persist children.
- `session.delete(user)` will **not** delete addresses automatically (DB may complain if FK is non-nullable and no FK cascade exists).

#### 2) Delete children when parent deleted (ORM-level)

```py
addresses = relationship("Address", cascade="delete")
```

- `session.delete(user)` will mark the addresses for deletion too and emit DELETEs on flush.

#### 3) Delete children when parent deleted _and_ delete children removed from collection

```py
addresses = relationship("Address", cascade="all, delete-orphan")
```

- `all` = `save-update, merge, refresh-expire, expunge, delete`.
- `delete-orphan` means if you do `user.addresses.remove(addr)`, `addr` is deleted on flush.
- Very common for parent-owned entities (e.g., `Order` → `OrderLine`).

**Caution:** `delete-orphan` is usually wrong for many-to-many relationships, because children may be shared.

#### 4) Let the DB do deletes (ON DELETE CASCADE) and avoid ORM issuing separate DELETEs

```py
# ForeignKey on child:
user_id = Column(Integer, ForeignKey("users.id", ondelete="CASCADE"))

# relationship:
addresses = relationship("Address", passive_deletes=True)
```

- `ondelete="CASCADE"` makes DB delete child rows when parent row removed via SQL.
- `passive_deletes=True` tells SQLAlchemy: _trust the DB to remove the children; don’t issue SELECTs/deletes for them._ This avoids extra round-trips and prevents ORM from trying to delete children again.
- Important when other clients may delete rows directly in DB (outside your app).

**Note (SQLite):** SQLite ignores FK cascade unless `PRAGMA foreign_keys = ON` is set on the connection.

---

### Behavior examples (practical)

#### Example models

```py
class User(Base):
    __tablename__ = "users"
    id = Column(Integer, primary_key=True)
    addresses = relationship("Address", cascade="all, delete-orphan")

class Address(Base):
    __tablename__ = "addresses"
    id = Column(Integer, primary_key=True)
    user_id = Column(Integer, ForeignKey("users.id", ondelete="CASCADE"))
```

- Creating:

```py
u = User()
a = Address()
u.addresses.append(a)
session.add(u)
session.commit()
# address saved because of save-update (part of "all")
```

- Removing an address from the user:

```py
u.addresses.remove(a)
session.flush()  # because of delete-orphan, a is deleted from DB
```

- Deleting the user:

```py
session.delete(u)
session.commit()  # because of delete in "all", addresses will be removed by ORM
```

If you used DB cascade + passive_deletes instead:

```py
addresses = relationship("Address", passive_deletes=True)
user_id = Column(Integer, ForeignKey("users.id", ondelete="CASCADE"))
session.delete(u)
session.commit()  # DB deletes addresses; ORM does not emit extra DELETEs for Address
```

---

# Important gotchas & tips

- **Default does NOT delete children.** If you remove a parent without configuring cascades and there is a NOT NULL foreign key, you’ll likely get an `IntegrityError` from the DB.
- **`delete` vs `delete-orphan`:** `delete` deletes child rows when parent is deleted. `delete-orphan` deletes the child when it’s removed from the parent’s collection. Use both if child should only exist with parent.
- **Mixing DB `ON DELETE` and ORM delete:** If you rely on DB-level cascade (`ondelete="CASCADE"`) and still have ORM `delete` cascade enabled, you may see duplicate work or surprising behavior. Use `passive_deletes=True` to let DB handle it and to keep ORM quiet.
- **Many-to-many caution:** `delete-orphan` is almost always wrong on many-to-many association objects unless the association _is_ an entity that should be deleted when de-associated.
- **Performance:** ORM-level cascades can cause additional SELECT or DELETE statements. When lots of children exist and DB cascade is available, it’s often faster to use `ondelete` + `passive_deletes=True`.
- **Async sessions**: `all` includes `refresh-expire` which can cause unexpected I/O in async contexts — read docs warnings if you use async sessions.