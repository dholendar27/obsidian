---
date created: 2025-10-06 16:45
---

**CRUD operations** — which stand for **Create, Read, Update, Delete** — the fundamental operations for managing data in databases. These correspond to SQL commands:

- **CREATE** → `INSERT`
- **READ** → `SELECT`
- **UPDATE** → `UPDATE`
- **DELETE** → `DELETE`

---

## 1. SELECT (Read)

The `SELECT` statement is used to **read or retrieve data** from one or more tables.

### Syntax:

```sql
SELECT column1, column2, ...
FROM table_name
WHERE condition
ORDER BY column1 ASC|DESC
LIMIT number;
```

### Example:

```sql
SELECT id, name, email
FROM users
WHERE age > 18
ORDER BY name ASC
LIMIT 10;
```

**Explanation:**

- Retrieves `id`, `name`, and `email` from the `users` table.
- Filters users where age is greater than 18.
- Orders results by `name` in ascending order.
- Limits the output to 10 rows.

---

## 2. INSERT (Create)

The `INSERT` statement adds **new rows** of data into a table.

### Syntax:

```sql
INSERT INTO table_name (column1, column2, ...)
VALUES (value1, value2, ...);
```

### Example:

```sql
INSERT INTO users (name, email, age)
VALUES ('Alice', 'alice@example.com', 25);
```

**Explanation:**

- Inserts a new user record into the `users` table with name Alice, email, and age.

---

## 3. UPDATE (Update)

The `UPDATE` statement **modifies existing data** in a table.

### Syntax:

```sql
UPDATE table_name
SET column1 = value1, column2 = value2, ...
WHERE condition;
```

### Example:

```sql
UPDATE users
SET email = 'alice.new@example.com'
WHERE id = 1;
```

**Explanation:**

- Updates the email address for the user whose `id` is 1.

**Important:** Always use a `WHERE` clause unless you want to update every row!

---

## 4. DELETE (Delete)

The `DELETE` statement **removes rows** from a table.

### Syntax:

```sql
DELETE FROM table_name
WHERE condition;
```

### Example:

```sql
DELETE FROM users
WHERE id = 1;
```

**Explanation:**

- Deletes the user with `id` equal to 1.
**Warning:** Without a `WHERE` clause, all rows will be deleted!
