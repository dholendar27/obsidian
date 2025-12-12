---
date created: 2025-12-12 11:25
---

### 1. Initial Migrations

When you first set up Prisma, you typically generate an initial migration to match the current state of your database schema.

- **Steps:**
  - Define your schema in `schema.prisma`.
  - Run `prisma migrate dev` (or `prisma migrate deploy` in production)
  This will generate a migration file in the `prisma/migrations` folder. You can then apply this migration to your database.

- **Example:**

  ```bash
  prisma migrate dev --name init
  ```

  This command creates an `init` migration and applies it to the development database.

---

### 2. **Applying Migrations on Dev/Stage/Prod**

Managing migrations across different environments is a crucial part of Prisma migrations. Here's how you can apply migrations in different environments:

- **Dev:** The `prisma migrate dev` command is ideal during development. It applies the migrations and can also generate a fresh database schema if necessary.

  - Command: `prisma migrate dev --name migration-name`
- **Stage/Prod:** In staging or production environments, you would typically use `prisma migrate deploy` to apply the migrations in a safe and predictable way. This ensures that all migrations are applied in the correct order.

  - Command: `prisma migrate deploy`

  Make sure to run this on a database with production data and **always** backup your database before deploying migrations in production.

---

### 3. **Drift Detection**

Drift happens when the actual database schema diverges from the Prisma schema (i.e., you manually modify the database outside of Prisma). Prisma provides a tool to detect this drift and bring the schema back in sync.

- To check for drift:

  - Command: `prisma migrate resolve --drift`
- Prisma will compare the database with the schema file and tell you if any drift exists. It might prompt you to apply necessary changes to reconcile the schema with the current state.

---

### 4. **Handling Breaking Schema Changes Safely**

When making changes that might break the schema (e.g., deleting fields, renaming them, or changing relationships), it's essential to handle migrations carefully.

- **Steps:**

  1. **Backup your database** before making any changes.
  2. If possible, create migrations in stages rather than large, disruptive changes.
  3. If you’re deleting fields or changing relations, use "soft" migrations (deprecating fields and then dropping them later).
  4. Be aware of production data: You might need to migrate data before schema changes, especially when removing fields or renaming them.

---

### 5. **Renaming Fields**

When you rename fields in your schema, Prisma will generate a migration for the rename, but if the database field has data that needs to be preserved, you may need to manually handle the renaming (especially for critical fields).

- **Steps to rename a field:**
  - Change the field name in `schema.prisma`.
  - Run `prisma migrate dev` to generate the migration.
  Prisma will detect this change and generate the necessary SQL. But ensure the data doesn't get lost by using custom SQL scripts if needed.

  **Example**:

  ```prisma
  model User {
    id    Int    @id @default(autoincrement())
    email String
    fullName String @map("name")  // Renaming field
  }
  ```

---

### 6. **Splitting Tables**

Splitting tables occurs when you need to break down a large table into smaller, more manageable ones (e.g., due to performance or normalization reasons). This usually involves both schema changes and possible data migrations.

- **Steps:**

  1. Create a new table in your schema.
  2. Write a migration that moves data from the old table to the new one.
  3. Drop the old table once you're sure it's no longer needed.
  4. Ensure proper foreign key relations between the new tables.

  You might need to use custom SQL queries or write migration scripts to migrate the data between the tables.

---

### 7. **Changing Relations**

Changing relations in Prisma (e.g., switching between one-to-many, many-to-many, or one-to-one) can also result in significant schema changes. You need to be cautious when altering these relationships.

- **Steps:**

  1. Modify the relationship in the `schema.prisma`.
  2. If changing a relation type, Prisma may create a new table to represent a many-to-many relationship.
  3. Update any application logic that may rely on the old relation type.
  4. 
  **Example**: Changing a one-to-many relation to a many-to-many relation.

```prisma
  // Before
  model User {
    id     Int    @id @default(autoincrement())
    posts  Post[] // One-to-many
  }

  model Post {
    id     Int    @id @default(autoincrement())
    author User   @relation(fields: [authorId], references: [id])
    authorId Int
  }

  // After (changing to many-to-many)
  model User {
    id     Int    @id @default(autoincrement())
    posts  Post[] @relation("UserPosts")
  }

  model Post {
    id     Int    @id @default(autoincrement())
    authors User[] @relation("UserPosts")
  }
```

- For breaking changes like this, you may need to migrate your data manually or with a custom migration.