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

# 🔗 3. **Many-to-Many Relationship**

### Real-World Example:

A **student** can enroll in **many courses**,  
and a **course** can have **many students**.

We can’t just store this with a single foreign key —  
we need a **third table** (called an **association table** or **junction table**) that connects them.

---

### Database Design:

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

### SQLAlchemy Example:

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

### Usage:

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

✅ **Summary:**

> “Each student can take many courses.”  
> “Each course can have many students.”  
> The link between them is stored in a **third table**.

---

# 🧠 4. **Quick Comparison Table**

|Relationship Type|Foreign Key Location|Example|ORM Representation|
|---|---|---|---|
|**One-to-Many**|Child table|One user → many posts|`relationship("Post", back_populates="user")`|
|**Many-to-One**|Child table|Many posts → one user|`relationship("User", back_populates="posts")`|
|**Many-to-Many**|Association table|Many students ↔ many courses|`relationship(..., secondary=association_table)`|

---

# 🔍 5. **Common Confusions Cleared**

|Confusion|Clarification|
|---|---|
|“Do I need both One-to-Many and Many-to-One?”|They’re two sides of the same coin — define one with `back_populates` for bidirectional traversal.|
|“Why Many-to-Many needs an extra table?”|Because you can’t store multiple foreign keys in a single column — you need a junction table to link both sides.|
|“Can I access both sides?”|Yes — if you use `back_populates` or `backref`.|
|“Does relationship() create a foreign key?”|No, it just _uses_ existing foreign keys to map Python objects.|

---

Would you like me to show a **visual diagram (table + arrows)** for all three relationships (1→∞, ∞→1, and ∞↔∞)?  
It makes the differences instantly clear.