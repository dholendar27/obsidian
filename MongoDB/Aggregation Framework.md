---
date created: 2025-11-14 14:35
date updated: 2025-11-14 14:44
---

## **1. Aggregation Pipeline Basics**

The **aggregation pipeline** in MongoDB is a framework for **data aggregation** modeled on the concept of data processing pipelines. Documents in a collection pass through multiple **stages**, and each stage transforms the documents in some way.

- Think of it as a sequence of steps, each taking input documents and producing output documents.

- Syntax example:

```javascript
db.collection.aggregate([
    { $match: { status: "A" } },
    { $group: { _id: "$cust_id", total: { $sum: "$amount" } } },
    { $sort: { total: -1 } }
])
```

Here, documents are filtered, grouped, and then sorted.

---

## **2. Common Stages**

1. **`$match`** – Filters documents (like `WHERE` in SQL).

```javascript
{ $match: { status: "A" } }
```

2. **`$group`** – Groups documents by a key and allows aggregation operators like `$sum`, `$avg`.

```javascript
{ $group: { _id: "$cust_id", totalAmount: { $sum: "$amount" } } }
```

3. **`$project`** – Reshapes documents, can include/exclude fields, add computed fields.

```javascript
{ $project: { name: 1, totalAmount: 1, discount: { $multiply: ["$totalAmount", 0.1] } } }
```

4. **`$sort`** – Sorts documents by a field.

```javascript
{ $sort: { totalAmount: -1 } } // descending
```

5. **`$limit`** – Limits the number of documents passed downstream.

```javascript
{ $limit: 5 }
```

6. **`$lookup`** – Performs a left outer join with another collection.

```javascript
{
  $lookup: {
    from: "orders",
    localField: "_id",
    foreignField: "cust_id",
    as: "orders_info"
  }
}
```

7. **`$unwind`** – Deconstructs an array field into multiple documents.

```javascript
{ $unwind: "$orders_info" }
```

---

## **3. Aggregation Expressions and Operators**

- Expressions are used in stages like `$project`, `$group`.
- Examples:

| Operator                                    | Description                             |
| ------------------------------------------- | --------------------------------------- |
| `$sum`                                      | Sum of values (can be used in `$group`) |
| `$avg`                                      | Average of values                       |
| `$min`, `$max`                              | Minimum and maximum values              |
| `$push`                                     | Create an array of values               |
| `$add`, `$subtract`, `$multiply`, `$divide` | Arithmetic operations                   |
| `$concat`, `$substr`                        | String operations                       |
| `$cond`, `$ifNull`                          | Conditional expressions                 |

Example combining `$project` and arithmetic:

```javascript
{
  $project: {
    name: 1,
    totalWithTax: { $multiply: ["$totalAmount", 1.1] }
  }
}
```

---

## **4. Map-Reduce Overview**

- **Map-Reduce** is an older MongoDB technique for aggregation.

- Works in **two phases**:
  1. **Map**: Processes each document and emits key-value pairs.
  2. **Reduce**: Combines values with the same key.

- Example:

```javascript
db.sales.mapReduce(
  function() { emit(this.cust_id, this.amount); },  // map
  function(key, values) { return Array.sum(values); }, // reduce
  { out: "total_sales" }
)
```

- **Note:** Aggregation pipeline is usually preferred over Map-Reduce for performance and readability.

---

## **5. Performance and Optimization**

1. **Use `$match` early** – Filtering documents early reduces the amount of data flowing through the pipeline.
2. **Index-friendly stages** – `$match` and `$sort` can leverage indexes.
3. **Avoid `$unwind` on large arrays** – It can explode the number of documents.
4. **Use `$project` to reduce fields** – Passing only required fields reduces memory usage.
5. **Pipeline optimization** – MongoDB can sometimes optimize the order of operations internally, but it’s good practice to put selective operations early.
6. **Explain plan** – Use `.explain("executionStats")` to check pipeline efficiency.

```javascript
	db.collection.aggregate([...]).explain("executionStats")
```

### General Template

```javascript
	db.collection.aggregate([
	    { $match: { /* filter documents */ } },
	    { $group: { _id: "key", accumulators... } },
	    { $project: { /* reshape fields */ } },
	    { $sort: { field: 1 or -1 } },
	    { $limit: N },
	    { $lookup: { /* join another collection */ } },
	    { $unwind: "$arrayField" }
	])
```

---

## **1. `$lookup` - Joining Collections**

The **`$lookup`** stage allows you to **perform left outer joins** between two collections, bringing in data from a related collection based on a common field. This is similar to SQL’s **JOIN** operation.

### Syntax

```javascript
{
  $lookup: {
    from: "<foreign collection>",   // The name of the collection to join with
    localField: "<field in local collection>", // The field in the local collection
    foreignField: "<field in foreign collection>", // The field in the foreign collection
    as: "<alias for the new field>"      // The name of the field where the result will be stored
  }
}
```

### Example

Let’s say we have two collections:

- **`orders`**

```json
[
  { "_id": 1, "cust_id": "A123", "total": 50 },
  { "_id": 2, "cust_id": "B456", "total": 30 },
  { "_id": 3, "cust_id": "A123", "total": 20 }
]
```

- **`customers`**

```json
[
  { "_id": "A123", "name": "Alice" },
  { "_id": "B456", "name": "Bob" }
]
```

### Using `$lookup`

To **join the `orders` collection** with the `customers` collection to get customer information in the order documents, we can use `$lookup` like this:

```javascript
db.orders.aggregate([
  {
    $lookup: {
      from: "customers",        // The name of the collection to join with
      localField: "cust_id",     // The field in the `orders` collection to match
      foreignField: "_id",      // The field in the `customers` collection to match
      as: "customer_info"       // The new field to add to `orders`
    }
  }
])
```

### Output

The result will include an array field called `customer_info` containing the matching customer documents:

```json
[
  {
    "_id": 1,
    "cust_id": "A123",
    "total": 50,
    "customer_info": [ { "_id": "A123", "name": "Alice" } ]
  },
  {
    "_id": 2,
    "cust_id": "B456",
    "total": 30,
    "customer_info": [ { "_id": "B456", "name": "Bob" } ]
  },
  {
    "_id": 3,
    "cust_id": "A123",
    "total": 20,
    "customer_info": [ { "_id": "A123", "name": "Alice" } ]
  }
]
```

- **Note:** If there’s no matching document in the `customers` collection for a given `cust_id`, the `customer_info` field will be an empty array (`[]`).

### **Join Types in `$lookup`**

The `$lookup` operator supports more advanced features like **unwinding** or **filters** on the foreign collection, but by default, it does a left outer join.

For more advanced use cases, MongoDB supports the **`let`** and **`pipeline`** options in `$lookup`, where you can pass a **sub-pipeline** to filter or further process the joined data.

---

## **2. `$unwind` - Deconstructing Arrays**

The **`$unwind`** stage is used to **deconstruct** an array field in the input documents and create multiple documents for each element in the array. It "flattens" arrays.

### Syntax

```javascript
{
  $unwind: "<arrayField>"
}
```

### Example

Let’s say we have the following result from the `$lookup` stage (i.e., the previous example of `orders` with a `customer_info` field):

```json
[
  {
    "_id": 1,
    "cust_id": "A123",
    "total": 50,
    "customer_info": [ { "_id": "A123", "name": "Alice" } ]
  },
  {
    "_id": 2,
    "cust_id": "B456",
    "total": 30,
    "customer_info": [ { "_id": "B456", "name": "Bob" } ]
  },
  {
    "_id": 3,
    "cust_id": "A123",
    "total": 20,
    "customer_info": [ { "_id": "A123", "name": "Alice" } ]
  }
]
```

Let’s use `$unwind` to **deconstruct** the `customer_info` array into individual documents:

```javascript
db.orders.aggregate([
  { $lookup: {
      from: "customers",
      localField: "cust_id",
      foreignField: "_id",
      as: "customer_info"
  }},
  { $unwind: "$customer_info" }
])
```

### Output

After using `$unwind`, the `customer_info` array is flattened into individual documents:

```json
[
  {
    "_id": 1,
    "cust_id": "A123",
    "total": 50,
    "customer_info": { "_id": "A123", "name": "Alice" }
  },
  {
    "_id": 2,
    "cust_id": "B456",
    "total": 30,
    "customer_info": { "_id": "B456", "name": "Bob" }
  },
  {
    "_id": 3,
    "cust_id": "A123",
    "total": 20,
    "customer_info": { "_id": "A123", "name": "Alice" }
  }
]
```

- **Important**: If the array field (`customer_info`) is empty, `$unwind` can be configured with **`preserveNullAndEmptyArrays: true`**, which keeps the document even when the array is empty, instead of removing it.

### Advanced Usage: `$unwind` with Options

You can also use the following options with `$unwind`:

- **`preserveNullAndEmptyArrays`**: If `true`, the stage will **retain documents with empty arrays**
- **`includeArrayIndex`**: Adds a new field to each output document representing the **index of the element** in the array.

Example with options:

```javascript
{
  $unwind: {
    path: "$customer_info",
    preserveNullAndEmptyArrays: true,
    includeArrayIndex: "arrayIndex"
  }
}
```

### Output with `includeArrayIndex`:

```json
[
  {
    "_id": 1,
    "cust_id": "A123",
    "total": 50,
    "customer_info": { "_id": "A123", "name": "Alice" },
    "arrayIndex": 0
  },
  {
    "_id": 2,
    "cust_id": "B456",
    "total": 30,
    "customer_info": { "_id": "B456", "name": "Bob" },
    "arrayIndex": 0
  },
  {
    "_id": 3,
    "cust_id": "A123",
    "total": 20,
    "customer_info": { "_id": "A123", "name": "Alice" },
    "arrayIndex": 0
  }
]
```

---

### **Summary of `$lookup` and `$unwind`**

- **`$lookup`**:
  - Joins two collections based on a **common field**
  - Creates a new field in the output document containing an **array** of matching documents from the foreign collection.
  - Can perform more complex joins using the **`pipeline`** option (since MongoDB 3.6).
- **`$unwind`**:

  - Deconstructs an **array field** into separate documents, with each document containing a single element of the array.
  - Can be used to "flatten" nested data into a more normalized structure.
