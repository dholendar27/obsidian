---
date created: 2025-10-06 16:38
---

### 1. **TCL (Transaction Control Language)**

TCL commands manage transactions in the database. A transaction is a sequence of operations performed as a single logical unit of work.

**Common TCL commands:**

- **COMMIT**: Saves all changes made in the current transaction permanently to the database.
- **ROLLBACK**: Undoes all changes made in the current transaction.
- **SAVEPOINT**: Sets a point within a transaction to which you can later roll back.
- **SET TRANSACTION**: Sets the properties for the transaction.

**Example:**

```sql
BEGIN TRANSACTION;
UPDATE accounts SET balance = balance - 100 WHERE account_id = 1;
UPDATE accounts SET balance = balance + 100 WHERE account_id = 2;
COMMIT; -- saves changes permanently
```

---

### 2. **DCL (Data Control Language)**

DCL commands control access and permissions on the database objects.

**Common DCL commands:**

- **GRANT**: Gives user access privileges to the database.
- **REVOKE**: Removes user access privileges.

**Example:**

```sql
GRANT SELECT, INSERT ON employees TO user1;
REVOKE INSERT ON employees FROM user1;
```

---

### 3. **DML (Data Manipulation Language)**

DML commands are used to manipulate data stored in tables.

**Common DML commands:**

- **SELECT**: Retrieves data from the database.
- **INSERT**: Adds new records.
- **UPDATE**: Modifies existing records.
- **DELETE**: Removes records.

**Example:**

```sql
SELECT * FROM employees;
INSERT INTO employees (name, age) VALUES ('John', 30);
UPDATE employees SET age = 31 WHERE name = 'John';
DELETE FROM employees WHERE name = 'John';
```

---

### 4. **DDL (Data Definition Language)**

DDL commands define or alter the structure of database objects like tables, indexes, and schemas.

**Common DDL commands:**

- **CREATE**: Creates new database objects.
- **ALTER**: Modifies existing database objects.
- **DROP**: Deletes database objects.
- **TRUNCATE**: Removes all records from a table, but not the table itself.

**Example:**

```sql
CREATE TABLE employees (id INT, name VARCHAR(50), age INT);
ALTER TABLE employees ADD COLUMN salary DECIMAL(10,2);
DROP TABLE employees;
TRUNCATE TABLE employees;
```

---

## How to Remember Them Easily

Here’s a simple way to remember these:

- **DDL (Data Definition Language)** = _Define_ (structure of DB)
- **DML (Data Manipulation Language)** = _Manipulate_ (data itself)
- **DCL (Data Control Language)** = _Control_ (permissions)
- **TCL (Transaction Control Language)** = _Transactions_ (commit/rollback)

**Mnemonic:**

> **D**on't **M**ix **C**old **T**ea\
> DDL - Define\
> DML - Manipulate\
> DCL - Control\
> TCL - Transaction

Or think of it as:

- **DDL** is about **"building"** your database (creating tables, etc.)
- **DML** is about **"working with data"** inside your tables.
- **DCL** is about **"who can do what"** in your database.
- **TCL** is about **"saving or undoing changes"** (transactions).
