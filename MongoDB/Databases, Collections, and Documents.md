## 1. Databases

A **database** in MongoDB is a logical container that holds related **collections**.  
Each database keeps its own set of data files, collections, and indexes.

- You can have multiple databases on a single MongoDB server (for example, `customers`, `inventory`, `analytics`).
- Each database is independent from the others.

**Example:**

```
MongoDB Server
│
├── ecommerce (Database)
│   ├── products (Collection)
│   ├── customers (Collection)
│   └── orders (Collection)
│
└── hr (Database)
    ├── employees (Collection)
    └── departments (Collection)
```

In MongoDB, the **default database** is called `test` (used if no database is specified).

---

## 2. Collections

A **collection** is a group of **documents** within a database.  
You can think of it as similar to a **table** in a relational database, but with some key differences:

- A collection **does not enforce a schema**, meaning each document can have different fields or structures.
- Collections are created automatically when you first insert data into them.

**Example:**  
A collection named `users` may contain documents like these:

```json
{ "_id": 1, "name": "Alice", "email": "alice@example.com" }
{ "_id": 2, "name": "Bob", "email": "bob@example.com", "age": 30 }
```

Notice that the second document has an extra field (`age`), which is perfectly valid.

---

## 3. Documents

A **document** is the basic unit of data in MongoDB — similar to a row in a relational database, but stored in **BSON** format (Binary JSON).

Each document is a set of key–value pairs and can contain nested documents or arrays.  
Every document has a unique field called `_id`, which acts like a primary key.

**Example Document:**

```json
{
  "_id": "u101",
  "name": "John Doe",
  "email": "john.doe@example.com",
  "age": 29,
  "address": {
    "city": "New York",
    "zip": "10001"
  },
  "skills": ["MongoDB", "Node.js", "React"]
}
```

### Document Characteristics:

- **Flexible structure:** Fields can differ between documents.
- **Nested data:** Documents can contain other documents or arrays.
- **Self-describing:** Each document stores both its data and field names.
