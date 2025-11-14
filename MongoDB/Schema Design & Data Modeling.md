## 1. Introduction

In traditional relational databases, schema design is rigid: you define tables, columns, and data types before storing any data. MongoDB, as a **NoSQL document database**, is **schema-less** in the sense that it does not enforce a fixed schema.

However, **schema design and data modeling are still important** for:

- Query performance
- Data consistency
- Application scalability

MongoDB schema design focuses on **how you structure your documents and collections** to meet your application requirements.

---

## 2. Schema Design Principles in MongoDB

### Embed vs. Reference

MongoDB provides two primary ways to model relationships:
#### a. Embedding (Denormalization)

- Store related data **inside the same document**.
- Ideal for **“contains” relationships** where related data is frequently accessed together.
- Reduces the need for joins and improves read performance.

**Example:** User with embedded addresses

```json
{
  "_id": "u101",
  "name": "John Doe",
  "email": "john@example.com",
  "addresses": [
    { "street": "123 Main St", "city": "New York", "zip": "10001" },
    { "street": "456 Park Ave", "city": "Boston", "zip": "02101" }
  ]
}
```

#### b. Referencing (Normalization)

- Store related data in **separate collections** and link them using references (ObjectId).
- Ideal for **many-to-many relationships** or data that changes independently.
- Queries may require additional lookups (`$lookup` in aggregation).

**Example:** Users and Orders in separate collections

```json
// users collection
{ "_id": "u101", "name": "John Doe" }

// orders collection
{ "_id": "o1001", "user_id": "u101", "product": "Laptop", "price": 1200 }
```

---

### Consider Query Patterns First

- Design the schema based on **how the application queries the data**, not just how it looks.
- Optimize for **frequent queries** to reduce joins and improve performance.

---

### Avoid Large Documents

- MongoDB has a **16 MB document size limit**.
- Don’t embed unbounded arrays that could grow indefinitely; consider referencing instead.

---

### Use Indexes Wisely

- Index fields that are frequently queried to improve performance.
- MongoDB supports single-field, compound, text, geospatial, and hashed indexes.

---

### Balance Read and Write Patterns
    

- Embedding favors **reads**, while referencing favors **writes and updates**.
- Analyze which operations are more frequent in your application.#

---

## 3. Common Data Modeling Patterns

#### **One-to-One**
- Can use embedding if the related data is always accessed together.

#### **One-to-Many**
- Use embedding for **small and bounded arrays**.
- Use referencing if the array is large or unbounded.

#### **Many-to-Many**
- Usually handled via **referencing** with join-like operations (`$lookup`) in aggregation.

#### **Tree Structures / Hierarchies**
- Can use **parent references**, **child references**, or **nested sets** depending on query patterns.
---

## 4. Example: E-Commerce Application

**Users Collection with Embedded Orders (small order history)**

```json
{
  "_id": "u101",
  "name": "Alice",
  "orders": [
    { "product": "Laptop", "price": 1200, "date": "2025-01-01" },
    { "product": "Mouse", "price": 25, "date": "2025-01-10" }
  ]
}
```

**Orders Collection with References (large order history)**

```json
// users collection
{ "_id": "u101", "name": "Alice" }

// orders collection
{ "_id": "o1001", "user_id": "u101", "product": "Laptop", "price": 1200 }
{ "_id": "o1002", "user_id": "u101", "product": "Mouse", "price": 25 }
```

- Embedding reduces query complexity for small datasets.
- Referencing avoids exceeding document size and supports large datasets.
