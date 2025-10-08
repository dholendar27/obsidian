---
date created: 2025-10-06 15:57
date updated: 2025-10-06 16:02
---

### What is a **Relational Database**?

A **relational database** is a type of database that stores data in **tables** (also called relations). Each table is like a spreadsheet with rows and columns.

- **Tables** represent entities (like `Customers`, `Orders`, `Products`).
- Each **row** in a table is a single record (e.g., one customer).
- Each **column** represents a property or attribute of the entity (e.g., customer name, order date).

**Why is it called “relational”?**\
Because tables can be related to each other through keys (like a Customer ID that links Orders to Customers). This helps organize data efficiently and avoid duplication.

### What is SQL?

**SQL** stands for **Structured Query Language**.

- It is the **language** used to interact with relational databases.
- You use SQL to **create**, **read**, **update**, and **delete** data inside the tables.
- It allows you to query data, filter it, join tables, and manage database structures.

### What is a Database?

A **database** is an organized collection of data, stored electronically so it can be easily accessed, managed, and updated. Think of it like a digital filing cabinet where data is stored in an organized way.

#### Basic Operations: Create, Delete

##### 1. Create a Database

```sql
CREATE DATABASE my_database;
```

This command creates a new database called `my_database`.

---

##### 2. Delete a Database

```sql
DROP DATABASE my_database;
```

This command deletes the entire database called `my_database` (all tables and data inside it will be lost).

---

### What is a Table?

Inside a database, data is stored in **tables**.

- A **table** is like a spreadsheet with rows and columns.
- Each **row** (or record) represents a single item or entry.
- Each **column** (or field) represents a type of data or attribute for those items.

Example: A table called **Users** might have columns like `UserID`, `Name`, `Email`, and `Age`. Each row will have data for one user.

---

### Basic Operations: Create, Delete, Add Data

##### 3. Create a Table

Example: Create a table named `Users` with columns for user ID, name, email, and age.

```sql
CREATE TABLE Users (
  UserID INT PRIMARY KEY,
  Name VARCHAR(100),
  Email VARCHAR(100),
  Age INT
);
```

- `INT` is for integer numbers.
- `VARCHAR(100)` is for text up to 100 characters.
- `PRIMARY KEY` is a unique identifier for each row.

---

##### 4. Delete a Table

```sql
DROP TABLE Users;
```

Deletes the entire `Users` table and all its data.

---

##### 5. Insert Data (Add Data into Table)

Example: Add a user into the `Users` table.

```sql
INSERT INTO Users (UserID, Name, Email, Age) VALUES (1, 'Alice', 'alice@example.com', 25);
```

This adds a new user with ID 1, name Alice, email, and age 25.

---

### 6. Delete Data from Table

Example: Delete user with `UserID = 1`.

```sql
DELETE FROM Users WHERE UserID = 1;
```

This deletes the row where the user ID is 1.

> [!note] Note
> When you use a DELETE query without a WHERE clause, it will delete all the rows in the table.
