---
date created: 2025-11-14 12:54
---

Indexes are essential for improving query performance in MongoDB. They help you quickly locate documents, and understanding how they work is key to writing efficient queries.

---

### **1. What Are Indexes?**

An **index** is a **data structure** that improves the speed of data retrieval operations on a database collection. MongoDB uses indexes to avoid **full collection scans** and enables fast lookups, sorting, and range queries.

Without an index, MongoDB would need to scan every document in the collection to find matching results, which can be slow for large datasets.

### **How an Index Works**:

- It stores **field values** (keys) and **pointers** to the documents (usually via the `_id`).
- MongoDB sorts these keys (like a dictionary or phonebook), allowing **direct access** to documents without scanning the whole collection.

### **Example of a Simple Index:**

If you create an index on `name`:

```js
db.users.createIndex({ name: 1 })
```

MongoDB will store an index like this:

| name (indexed key) | pointer to document (_id) |
| ------------------ | ------------------------- |
| Alice              | → _id: 1                  |
| Bob                | → _id: 2                  |
| John               | → _id: 3                  |

Now, when you search for `{ name: "John" }`, MongoDB will quickly look up `"John"` in the index and retrieve the document using the pointer to `_id: 3`.

---

### **2. Types of Indexes**

MongoDB provides several types of indexes to suit different query needs. Here’s a breakdown:

#### **1. Single Field Index**

- **Description:** This is the most basic index, created on a single field.

- **Example:**

  ```js
   db.users.createIndex({ name: 1 })
  ```

- **Use case:** Optimizes queries that filter or sort by a single field.

#### **2. Compound Index**

- **Description:** Indexes created on multiple fields. MongoDB creates an index that works on **combinations of fields** in a document.

- **Example:**

```js
  db.users.createIndex({ name: 1, age: -1 })
```

- **Use case:** Optimizes queries that filter or sort by multiple fields (e.g., `{ name: "John", age: { $gt: 30 } }`).

#### **3. Multikey Index**

- **Description:** Created when a field contains an array. MongoDB automatically creates a multikey index to handle array data.
- **Example:**

```js
	db.users.createIndex({ tags: 1 })
```

If `tags` contains an array, MongoDB will create multiple index entries for each element in the array.

- **Use case:** Optimizes queries where the indexed field contains arrays (e.g., `tags: ["mongodb", "database"]`).

#### **4. Text Index**

- **Description:** Supports **full-text search** on string fields. It allows searching for words within text fields.
- **Example:**

  ```js
  db.articles.createIndex({ content: "text" })
  ```

- **Use case:** Optimizes searches like `db.articles.find({ $text: { $search: "database" } })`.

#### **5. Geospatial Index (2d, 2dsphere)**

- **Description:** Supports **geospatial queries**. It is used for location-based searches (e.g., finding documents near a given coordinate).

- **Types:**
  - `2d` index: For flat 2D coordinate data.
  - `2dsphere` index: For spherical coordinates (earth-like data).

- **Example:**

  ```js
  db.locations.createIndex({ location: "2dsphere" })
  ```

- **Use case:** Optimizes spatial queries like finding points within a certain radius.

#### **6. Hashed Index**

- **Description:** An index that hashes the value of the field and uses the hash to find documents.

- **Example:**

  ```js
  db.users.createIndex({ username: "hashed" })
  ```

- **Use case:** Mainly used for **sharding** to distribute data evenly across shards.

#### **7. TTL (Time-To-Live) Index**

- **Description:** Automatically deletes documents after a specified time period.

- **Example:**

  ```js
  db.sessions.createIndex({ createdAt: 1 }, { expireAfterSeconds: 3600 })
  ```

- **Use case:** Used for temporary data like sessions, where the data expires after a set time.

#### **8. Sparse Index**

- **Description:** Only indexes documents that **contain** the indexed field (ignores documents with missing fields).

- **Example:**

  ```js
  db.users.createIndex({ email: 1 }, { sparse: true })
  ```

- **Use case:** Useful when only some documents contain the indexed field.

#### **9. Partial Index**

- **Description:** Allows creating an index for documents that meet a specific condition (filter).

- **Example:**

  ```js
  db.users.createIndex(
    { age: 1 },
    { partialFilterExpression: { age: { $gte: 18 } } }
  )
  ```

- **Use case:** Optimizes queries for documents that meet a condition, reducing the size of the index.

---

### **3. Creating and Managing Indexes**

#### **Creating an Index:**

To create an index on a field:

```js
db.collection.createIndex({ field: 1 })
```

#### **Viewing Existing Indexes:**

To see all indexes for a collection:

```js
db.collection.getIndexes()
```

#### **Dropping an Index:**

To drop a specific index:

```js
db.collection.dropIndex("indexName")
```

To drop all indexes except for the default `_id` index:

```js
db.collection.dropIndexes()
```

#### **Ensuring Indexes are Unique:**

To create a **unique index** (prevents duplicates):

```js
db.users.createIndex({ username: 1 }, { unique: true })
```

---

### **4. Index Performance Considerations**

While indexes dramatically improve read performance, they come with trade-offs.

#### **Pros:**

- **Faster reads** (queries, sorts, range queries).

- **Optimized query execution** by reducing the need for full collection scans (COLLSCAN).

#### **Cons:**

- **Slower writes** (every insert, update, delete must also update the index).

- **Increased storage**: Indexes take up additional space in the database.

- **More RAM usage**: Indexes must be loaded into memory for optimal performance.

### **Best Practices for Indexes:**

- **Index frequently queried fields** (fields used in `find`, `sort`, or range queries).
- **Use compound indexes** for multi-field queries.
- **Avoid over-indexing**: Too many indexes can slow down writes and use excessive disk space.
- **Use `sparse` or `partial` indexes** for fields that are missing in many documents.

---

### **5. Explain Plans & Query Optimization**

MongoDB’s **explain()** method helps analyze and optimize query performance.

#### **What is an Explain Plan?**

An **explain plan** shows how MongoDB executes a query, including:
- **Query stages** (e.g., COLLSCAN, IXSCAN, etc.)
- **Indexes used**
- **Execution time**
  This helps you determine if an index is being used and how efficiently the query is executed.

#### **How to Use explain()**

You can use the `.explain()` method to get the query plan:

```js
db.users.find({ name: "John" }).explain("executionStats")
```

This will return information such as:

- **stage**: The operation being performed (e.g., COLLSCAN, IXSCAN).

- **nscanned**: Number of documents scanned.

- **nreturned**: Number of documents returned by the query.

#### **Example Output of explain()**:

```json
{
  "queryPlanner": {
    "winningPlan": {
      "stage": "IXSCAN",
      "inputStage": {
        "stage": "FETCH",
        "inputStage": {
          "stage": "IXSCAN",
          "indexName": "name_1",
          "keyPattern": { "name": 1 },
          "isMultiKey": false,
          "direction": "forward",
          "indexBounds": { "name": ["[\"John\", \"John\"]"] }
        }
      }
    }
  },
  "executionStats": {
    "nReturned": 1,
    "executionTimeMillis": 2
  }
}
```

#### **Key Terms in Explain Output:**

- **IXSCAN**: Index scan — the query used an index.
- **COLLSCAN**: Collection scan — the query scanned all documents (inefficient)
- **FETCH**: MongoDB fetched the full document after scanning the index.

---

### Best Practices for Index Optimization**

- **Start with the most selective fields**: Index fields with high cardinality (many unique values).
- **Use compound indexes** for multi-field queries to avoid multiple index scans.
- **Keep your index sizes manageable**: Too many indexes can degrade write performance.
- **Check explain plans** regularly to ensure the query is using the best index.
- **Limit indexing on fields with low selectivity** (e.g., boolean or small range fields).
