Prisma schema defines the data models and their relationships in your application. Prisma is an ORM (Object-Relational Mapper) that interacts with relational databases in a type-safe manner. The schema file, typically located in `schema.prisma`, is where you define your models, their fields, and the relationships between them.

---

### Models

A model in Prisma represents a table in your database. Each model will have fields corresponding to columns in the database table. The syntax for defining a model is as follows:

```javascript
model ModelName {
  field1 Type
  field2 Type
  ...
}
```

For example:

```javascript
model User {
  id       Int    @id @default(autoincrement())
  name     String
  email    String @unique
  age      Int?
  posts    Post[]
}
```

In this example, the `User` model is mapped to a `users` table in the database with columns `id`, `name`, `email`, `age`, and a relation to the `Post` model.

---

### Field Types

Prisma supports a variety of field types, each corresponding to a database column type. The common field types are:

- **String**: Used to represent text or characters. Equivalent to `VARCHAR` in SQL.
- **Int**: Represents an integer value. Equivalent to `INT` in SQL.
- **Float**: Represents a floating-point number. Equivalent to `FLOAT` in SQL.
- **Decimal**: A fixed-point number, useful for precise calculations (e.g., financial data)
- **Boolean**: A field that stores a `true` or `false` value. Equivalent to `BOOLEAN` in SQL.
- **DateTime**: Represents a date and time, commonly used for tracking creation or modification times. Equivalent to `DATETIME` in SQL.
- **Bytes**: Used to store binary data such as images or files. Equivalent to `BLOB` in SQL.
- **Json**: Used to store unstructured data in JSON format. Equivalent to `JSON` in SQL.
    

Example:

```
model Post {
  id        Int      @id @default(autoincrement())
  title     String
  content   String
  published Boolean  @default(false)
  createdAt DateTime @default(now())
}
```

In this example:

- `title` and `content` are strings.
- `published` is a boolean field with a default value of `false`.
- `createdAt` is a DateTime field that defaults to the current timestamp.
    

---

### Field Attributes

Attributes in Prisma provide additional functionality and constraints on fields. Common field attributes include:

- **@id**: Denotes the primary key for the model (table). Each model must have at least one field marked as `@id`.
    
    Example:
```
    model User {
      id Int @id @default(autoincrement())
      name String
    }
```

- **@default**: Sets a default value for a field. This can be used with values like `autoincrement()`, `now()`, `uuid()`, etc.
Example:
```
    model Product {
      id     Int     @id @default(autoincrement())
      price  Decimal @default(10.99)
    }
```

- **@unique**: Ensures the field values are unique across all records in the table. This is typically used for fields like `email`, `username`, or other unique identifiers.
Example:
```
    model User {
      id    Int    @id @default(autoincrement())
      email String @unique
    }
```

- **@relation**: Defines relationships between models. It specifies how one model is connected to another (e.g., one-to-many or many-to-many relationships). It’s often used with foreign key fields.
Example:
```
    model User {
      id    Int     @id @default(autoincrement())
      posts Post[]
    }
    
    model Post {
      id     Int    @id @default(autoincrement())
      userId Int
      user   User   @relation(fields: [userId], references: [id])
    }
```


In this example, a `User` can have many `Post` entries, and each `Post` is linked to a single `User` through the `userId` field.

---

### Data Types

Here’s a list of common data types used in the Prisma schema:

- **String**: Represents a text value (e.g., name, email).
- **Int**: Represents an integer value (e.g., age, quantity).
- **Float**: Represents a decimal number (e.g., ratings, price).
- **Decimal**: Used for precise fixed-point numbers (e.g., monetary amounts).
- **Boolean**: Stores `true` or `false` values (e.g., isActive).
- **DateTime**: Stores date and time information (e.g., timestamps for creation or updates).
- **Bytes**: Used for raw binary data (e.g., file content).
- **Json**: Stores unstructured data in JSON format (e.g., product details).

---

### Example Schema

Here’s an example schema that combines models, field types, and attributes:

```
model User {
  id        Int     @id @default(autoincrement())
  username  String  @unique
  email     String  @unique
  password  String
  posts     Post[]
  profile   Profile?
}

model Post {
  id        Int      @id @default(autoincrement())
  title     String
  content   String
  published Boolean  @default(false)
  createdAt DateTime @default(now())
  updatedAt DateTime @updatedAt
  userId    Int
  user      User     @relation(fields: [userId], references: [id])
}

model Profile {
  id        Int    @id @default(autoincrement())
  bio       String
  userId    Int
  user      User   @relation(fields: [userId], references: [id])
}
```

In this schema:

- The `User` model has a one-to-many relationship with the `Post` model (a user can have many posts).
- The `Post` model has a one-to-one relationship with the `Profile` model (each user can have a single profile).
- Various field attributes are used, such as `@id`, `@default`, `@unique`, `@relation`, and `@updatedAt`.