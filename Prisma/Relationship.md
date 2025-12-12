## 1. One-to-One Relationship

A one-to-one relationship is a relationship where each record in one table is associated with exactly one record in another table. In relational databases, this type of relationship is usually implemented with a foreign key constraint where the foreign key is unique in the child table.
#### Example:

Consider a `User` and `Profile` relationship. Each `User` has one `Profile`, and each `Profile` belongs to one `User`.

Schema Example in Prisma:
```javascript

model User {
	id Int @id @default(autoincrement())
	name String
	profile Profile?
}

  

model Profile {
	id Int @id @default(autoincrement())
	bio String
	userId Int @unique
	user User @relation(fields: [userId], references: [id])
}

```

* `User` model: Each user has a `Profile` (one-to-one).
* `Profile` model: The `Profile` has a unique `userId` to enforce the one-to-one relationship.
### 2. One-to-Many Relationship

A one-to-many relationship means one record in the first table is associated with many records in the second table. However, each record in the second table is related to exactly one record in the first table.
#### Example:
Consider a `User` and `Post` relationship where each `User` can have many `Posts`, but each `Post` belongs to only one `User`.

Schema Example in Prisma:

```javascript

model User {
	id Int @id @default(autoincrement())
	name String
	posts Post[]
}

  

model Post {
	id Int @id @default(autoincrement())
	title String
	content String
	userId Int
	user User @relation(fields: [userId], references: [id])
}

```

* `User` model: A user can have many posts.
* `Post` model: Each post has a `userId` as a foreign key to reference the `User`.

### 3. Many-to-Many Relationship (Implicit & Explicit Join Tables)

A many-to-many relationship occurs when many records in one table are associated with many records in another table. This type of relationship typically requires a join table to manage the many-to-many connections.
There are two approaches to handling many-to-many relationships in databases: explicit and implicit join tables.
#### a. Explicit Join Table
An explicit join table is a separate table created specifically to store the many-to-many relationships. This table usually contains the foreign keys referencing the two tables involved in the relationship.

Example in Prisma (Explicit Join Table):
```javascript

model User {
	id Int @id @default(autoincrement())
	name String
	posts Post[] @relation("UserPosts")
}

model Post {
	id Int @id @default(autoincrement())
	title String
	users User[] @relation("UserPosts")
}

model UserPost {
	userId Int
	postId Int
	user User @relation(fields: [userId], references: [id])
	post Post @relation(fields: [postId], references: [id])
	@@id([userId, postId]) // Composite primary key
}

```

* `UserPost` is the explicit join table between `User` and `Post`.
#### b. Implicit Join Table

Prisma can automatically handle many-to-many relationships without needing a manually defined join table. Prisma will automatically generate a join table behind the scenes when defining the relationship.

Example in Prisma (Implicit Join Table):

```javascript

model User {
	id Int @id @default(autoincrement())
	name String
	posts Post[] @relation()
}

model Post {
	id Int @id @default(autoincrement())
	title String
	users User[] @relation()
}

```

* Here, Prisma implicitly creates a join table for the `User` and `Post` relationship.

### 4. Relation Attributes (Fields, References)

In relational databases, when defining relationships, there are certain key attributes to use:
* `fields`: Specifies the fields in the child model (e.g., `userId` in the `Post` model).
* `references`: Specifies the fields in the parent model (e.g., `id` in the `User` model).

Example in Prisma:

```javascript

model Post {
	id Int @id @default(autoincrement())
	title String
	userId Int
	user User @relation(fields: [userId], references: [id])
}

```

In this example:

* The `userId` in the `Post` model is the foreign key (field).
* The `id` in the `User` model is the referenced primary key (reference).

### 5. Referential Actions (onDelete, onUpdate)
Referential actions determine what happens when a referenced record in the parent table is updated or deleted. Common referential actions include:

* `onDelete`: Defines what happens when a record in the parent table is deleted.
* `Cascade`: Automatically deletes dependent records in the child table.
* `SetNull`: Sets the foreign key in the child table to `NULL` when the parent record is deleted.
* `Restrict`: Prevents deletion of the parent record if there are dependent child records.
* `NoAction`: Does nothing and relies on database-level behavior.
* `onUpdate`: Defines what happens when a record in the parent table is updated.

Example in Prisma:
```javascript

model Post {
	id Int @id @default(autoincrement())
	title String
	userId Int
	user User @relation(fields: [userId], references: [id], onDelete: Cascade, onUpdate: Cascade)

}

```

In this case, if a `User` is deleted, the `onDelete: Cascade` action will automatically delete all `Post` records associated with that user.
### 7. Prisma Enums

Prisma enums are a way to represent a set of predefined values in your schema. They are useful for things like status codes, categories, or types.
Example of Prisma Enum:
```javascript

enum Role {
	USER
	ADMIN
	MODERATOR
}

model User {
	id Int @id @default(autoincrement())
	name String
	role Role
}

```

In this example, the `User` model has a `role` field that can be one of three values: `USER`, `ADMIN`, or `MODERATOR`.
### 8. Computed Fields (Using Middleware/API Not Schema)

Computed fields are values that aren't stored directly in the database but are calculated dynamically based on other fields. In Prisma, computed fields aren't defined in the schema, but they can be implemented in application logic using middleware or API calls.

#### Example:
You could compute a `fullName` field dynamically based on `firstName` and `lastName`.

```javascript

const user = await prisma.user.findUnique({
	where: { id: 1 },
	select: {
		firstName: true,
		lastName: true,
	},
});

user.fullName = `${user.firstName} ${user.lastName}`;

```

Here, the `fullName` is a computed field that combines `firstName` and `lastName`, but it's not stored in the database.