---
date created: 2025-11-13 16:57
---
CRUD is an acronym for the four basic types of operations you can perform on data: Create, Read, Update, Delete.

---

### 1. Create
Purpose: To add new documents to a collection.

**Methods:**
- `insertOne(document)`: Inserts a single document.
- `insertMany([document1, document2, ...])`: Inserts multiple documents.

**Syntax and Examples:**

```javascript
// Insert one document into the 'users' collection.
db.users.insertOne({
    name: "Alice",
    age: 25,
    status: "active"
})
```
*Explanation: The `db.users.insertOne()` function is called with a single object as its argument. This object is the document that will be added to the `users` collection.*

```javascript
// Insert multiple documents into the 'users' collection.
db.users.insertMany([
    { name: "Bob", age: 30, status: "inactive" },
    { name: "Charlie", age: 35, status: "active" }
])
```
*Explanation: The `db.users.insertMany()` function is called with an array of objects. Each object in the array becomes a separate document in the collection.*

---

### 2. Read
Purpose: To retrieve documents from a collection.

**Method:**
- `find(query, projection)`: Retrieves documents that match the query. An empty query `{}` returns all documents. The projection is optional and specifies which fields to return.

**Syntax and Examples:**

```javascript
// Find all documents in the 'users' collection.
db.users.find()
```
*Explanation: Calling `find()` with no arguments, or with an empty object `{}`, acts as a "SELECT *" command, returning every document in the collection.*

```javascript
// Find users where the 'status' field equals "active".
db.users.find({ status: "active" })
```
*Explanation: The first argument `{ status: "active" }` is the query filter. It tells MongoDB to only return documents where the `status` field has the value "active".*

```javascript
// Find active users, but only return their 'name' and 'age' fields (and the default '_id').
db.users.find(
    { status: "active" }, // Query Filter
    { name: 1, age: 1 }   // Projection (1 means include)
)
```
*Explanation: The second argument `{ name: 1, age: 1 }` is the projection. It specifies which fields to include in the results. The `_id` field is included by default unless explicitly set to `0`.*

---

### 3. Update
Purpose: To modify existing documents in a collection.

**Methods:**
- `updateOne(filter, update, options)`: Updates the first document that matches the filter.
- `updateMany(filter, update, options)`: Updates all documents that match the filter.

**Common Update Operators:**
- `$set`: Sets the value of a field.
- `$inc`: Increments the value of a numeric field.
- `$push`: Adds an item to an array.

**Syntax and Examples:**

```javascript
// Update the first user named "Alice", setting her status to "inactive".
db.users.updateOne(
    { name: "Alice" },          // Filter: Which document to update
    { $set: { status: "inactive" } } // Update Action: What to change
)
```
*Explanation: `updateOne` finds the first document matching `{ name: "Alice" }` and applies the update operation `$set`, which changes the value of the `status` field.*

```javascript
// Add 1 to the 'age' field for all active users.
db.users.updateMany(
    { status: "active" }, // Filter: All documents with status "active"
    { $inc: { age: 1 } }  // Update Action: Increment the 'age' field by 1
)
```
*Explanation: `updateMany` finds all documents matching the filter. The `$inc` operator is used to increase the numerical value of the `age` field.*

---

### 4. Delete
Purpose: To remove documents from a collection.

**Methods:**
- `deleteOne(filter)`: Deletes the first document that matches the filter.
- `deleteMany(filter)`: Deletes all documents that match the filter.

**Syntax and Examples:**

```javascript
// Delete the first user named "Charlie".
db.users.deleteOne({ name: "Charlie" })
```
*Explanation: `deleteOne` finds the first document that matches the filter `{ name: "Charlie" }` and removes it from the collection.*

```javascript
// Delete all users who are inactive.
db.users.deleteMany({ status: "inactive" })
```
*Explanation: `deleteMany` finds every document matching the filter `{ status: "inactive" }` and removes them all.*

---

### Query Operators

Query operators are used within the *filter* parameter of `find`, `update`, and `delete` methods to create more specific and powerful queries.

**Common Comparison Operators:**
- `$eq`: Equal to. (This is the default, so `{age: 25}` is the same as `{age: {$eq: 25}}`)
- `$gt`: Greater than.
- `$gte`: Greater than or equal to.
- `$lt`: Less than.
- `$lte`: Less than or equal to.
- `$ne`: Not equal to.
- `$in`: Matches any value in a specified array.
- `$nin`: Matches none of the values in a specified array.

**Examples:**
```javascript
// Find users older than 25.
db.users.find({ age: { $gt: 25 } })

// Find users whose age is 25, 30, or 35.
db.users.find({ age: { $in: [25, 30, 35] } })
```

**Logical Operators:**
- `$and`: Joins query clauses with a logical AND. Returns documents that match *all* conditions.
- `$or`: Joins query clauses with a logical OR. Returns documents that match *at least one* condition.

**Examples:**
```javascript
// Find users who are active AND are 30 years old.
db.users.find({
    $and: [
        { status: "active" },
        { age: 30 }
    ]
})

// Find users who are named "Alice" OR are older than 40.
db.users.find({
    $or: [
        { name: "Alice" },
        { age: { $gt: 40 } }
    ]
})
```

**Element Operator:**
- `$exists`: Matches documents that have the specified field.

**Example:**
```javascript
// Find users who have a 'phone' field defined.
db.users.find({ phone: { $exists: true } })
```

### Summary
- **Create**: Use `insertOne` or `insertMany` to add data.
- **Read**: Use `find` with a filter to retrieve data. Use a projection to select specific fields.
- **Update**: Use `updateOne` or `updateMany` with update operators like `$set` to modify data.
- **Delete**: Use `deleteOne` or `deleteMany` to remove data.
- **Query Operators**: Use operators like `$gt`, `$in`, `$and`, and `$or` inside your filters to build complex queries for finding, updating, or deleting specific documents.