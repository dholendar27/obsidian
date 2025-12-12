---
date created: 2025-12-12 10:37
---

### 1. **CRUD Operations**

CRUD operations are the basic operations used for managing data in a database:
- **`findUnique`**: Retrieves a unique record by a specific identifier, usually the primary key.

```js
  const user = await prisma.user.findUnique({
    where: { id: 1 }
  });
```

- **`findMany`**: Retrieves multiple records based on some filter criteria.

```js
  const users = await prisma.user.findMany({
    where: { age: { gte: 18 } }
  });
```

- **`create`**: Creates a new record in the database.

```js
  const newUser = await prisma.user.create({
    data: {
      name: "John Doe",
      email: "john@example.com"
    }
  });
```

- **`update`**: Updates an existing record.

```js
  const updatedUser = await prisma.user.update({
    where: { id: 1 },
    data: { email: "newemail@example.com" }
  });
```

- **`delete`**: Deletes a record by its unique identifier.

```js
  const deletedUser = await prisma.user.delete({
    where: { id: 1 }
  });
```

### 2. **Filtering**

You can filter data with various conditions using `where`, `AND`, `OR`, and `NOT`.

- **`where`**: The basic filtering condition.

```js
  const users = await prisma.user.findMany({
    where: {
      age: { gt: 20 }
    }
  });
```

- **`AND`**: Apply multiple conditions, all of which must be true.

```js
  const users = await prisma.user.findMany({
    where: {
      AND: [
        { age: { gte: 18 } },
        { email: { contains: "example" } }
      ]
    }
  });
```

- **`OR`**: Apply at least one of the conditions.

```js
  const users = await prisma.user.findMany({
    where: {
      OR: [
        { age: { lte: 25 } },
        { name: { contains: "John" } }
      ]
    }
  });
```

- **`NOT`**: Negate a condition.

```js
  const users = await prisma.user.findMany({
    where: {
      NOT: {
        age: { lt: 18 }
      }
    }
  });
```

### 3. **Sorting (orderBy)**

You can specify sorting of the results using `orderBy`.

- **Sorting by fields**:

```js
  const users = await prisma.user.findMany({
    orderBy: {
      age: 'desc' // 'asc' for ascending
    }
  });
```

- **Multiple fields**:

```js
  const users = await prisma.user.findMany({
    orderBy: [
      { age: 'desc' },
      { name: 'asc' }
    ]
  });
```

### 4. **Pagination**

Pagination is used to limit the number of records returned.

- **`take`**: Limit the number of records.

```js
  const users = await prisma.user.findMany({
    take: 10 // Fetch 10 users
  });
```

- **`skip`**: Skip a number of records (used for "page" navigation).

```js
  const users = await prisma.user.findMany({
    skip: 20, // Skip the first 20 users
    take: 10  // Fetch the next 10 users
  });
```

- **Cursor-based Pagination**: Use a cursor (usually an ID) to fetch records after a certain point.

```js
  const users = await prisma.user.findMany({
    cursor: { id: 20 },
    take: 10
  });
```

### 5. **Nested Writes (Create/Update Relations Together)**

You can perform operations on related records (e.g., creating or updating related records in one go).

- **Create nested records**:

```js
  const post = await prisma.post.create({
    data: {
      title: "New Post",
      author: {
        connect: { id: 1 } // Connect to an existing user
      },
      comments: {
        create: [{ text: "Great post!" }]
      }
    }
  });
```

- **Update nested records**:

```js
  const updatedPost = await prisma.post.update({
    where: { id: 1 },
    data: {
      comments: {
        update: {
          where: { id: 1 },
          data: { text: "Updated comment" }
        }
      }
    }
  });
```

### 6. **Batch Operations**

You can perform batch operations on multiple records at once.

- **Create many records**:

```js
  const users = await prisma.user.createMany({
    data: [
      { name: "Alice", email: "alice@example.com" },
      { name: "Bob", email: "bob@example.com" }
    ]
  });
```

- **Update many records**:

```js
  const updatedUsers = await prisma.user.updateMany({
    where: { age: { lt: 30 } },
    data: { isActive: true }
  });
```

- **Delete many records**:

```js
  const deletedUsers = await prisma.user.deleteMany({
    where: { age: { lt: 18 } }
  });
```

### 7. **Transactions ($transaction)**

You can use transactions to run multiple queries together and ensure they all succeed or fail as a group.

- **Example**:

```js
  const result = await prisma.$transaction([
    prisma.user.update({
      where: { id: 1 },
      data: { email: "newemail@example.com" }
    }),
    prisma.post.create({
      data: { title: "New Post", authorId: 1 }
    })
  ]);
```

### 8. **Raw Queries**

Sometimes you may need to run raw SQL queries for operations that are not supported directly by the ORM.

- **Raw SQL Query (for `SELECT` queries)**:

```js
  const users = await prisma.$queryRaw`SELECT * FROM User WHERE age > 30`;
```

- **Raw SQL Query (for `INSERT`, `UPDATE`, `DELETE`)**:

```js
  await prisma.$executeRaw`UPDATE User SET name = 'John' WHERE id = 1`;
```