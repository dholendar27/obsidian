---
date created: 2025-12-11 17:38
---

### Prisma Setup in Node.js / TypeScript

#### Prerequisites:

- Node.js installed.
- A database (PostgreSQL, MySQL, SQLite, etc.) set up.

---

### Step 1: Install Dependencies

1. **Initialize Node.js Project** (if not already done):

```bash
   npm init -y
```

2. **Install Prisma and Prisma Client**:

   - Install Prisma Client (runtime dependency):

```bash
	npm install @prisma/client
```

   - Install Prisma CLI (development dependency):

```bash
     npm install -D prisma
```

3. **Initialize Prisma**:
   Run the following to generate the `prisma` folder and configuration files:

```bash
   npx prisma init
```

---

### Step 2: Configure Database

1. **Set up the Database URL**:\
   In the `.env` file, set your `DATABASE_URL` based on your database provider:

   - **PostgreSQL**:

```bash
     DATABASE_URL="postgresql://USER:PASSWORD@localhost:5432/DATABASE?schema=public"
```

   - **MySQL**:

```bash
     DATABASE_URL="mysql://USER:PASSWORD@localhost:3306/DATABASE"
```

   - **SQLite**:

```bash
     DATABASE_URL="file:./dev.db"
```

---

### Step 3: Define the Data Model

1. **Edit `prisma/schema.prisma`**:

   - Define your data models inside the `schema.prisma` file.
   Example (User model):

```prisma
   datasource db {
     provider = "postgresql" // Change for other databases
     url      = env("DATABASE_URL")
   }

   generator client {
     provider = "prisma-client-js"
   }

   model User {
     id        Int      @id @default(autoincrement())
     name      String
     email     String   @unique
     createdAt DateTime @default(now())
   }
```

---

### Step 4: Migrate Database

1. **Create and Apply Migrations**:
   Generate the migration and apply it to the database:

```bash
   npx prisma migrate dev --name init
```

2. **This will**:

   - Create migration files in `prisma/migrations/`.
   - Apply the migration to your database.
   - Keep your database schema up-to-date.

---

### Step 5: Generate Prisma Client

1. **Generate Prisma Client**:\
   After running the migration, generate the Prisma Client to interact with the database:

```bash
   npx prisma generate
```

---

### Step 6: Use Prisma in Your Code

#### For **JavaScript**:

1. **Example (index.js)**:

```javascript
   const { PrismaClient } = require('@prisma/client');
   const prisma = new PrismaClient();

   async function main() {
     const newUser = await prisma.user.create({
       data: {
         name: "John Doe",
         email: "johndoe@example.com",
       },
     });
     console.log("New User: ", newUser);
   }

   main()
     .catch(e => {
       throw e
     })
     .finally(async () => {
       await prisma.$disconnect()
     });
```

#### For **TypeScript**:

1. **Example (index.ts)**:

```typescript
   import { PrismaClient } from '@prisma/client';

   const prisma = new PrismaClient();

   async function main() {
     const newUser = await prisma.user.create({
       data: {
         name: "John Doe",
         email: "johndoe@example.com",
       },
     });
     console.log("New User: ", newUser);
   }

   main()
     .catch(e => {
       throw e
     })
     .finally(async () => {
       await prisma.$disconnect()
     });
   ```

2. **Running TypeScript Code**:

   - Compile with TypeScript:

     ```bash
     npx tsc
     ```

   - Or use `ts-node` to run directly:

     ```bash
     npx ts-node index.ts
     ```

---

### Step 7: Common Prisma Commands

- **Migrate the database**:

  ```bash
  npx prisma migrate dev
  ```

- **Generate Prisma Client**:

  ```bash
  npx prisma generate
  ```

- **Introspect an existing database** (to generate the Prisma schema from an existing DB):

  ```bash
  npx prisma introspect
  ```

- **View migrations status**:

  ```bash
  npx prisma migrate status
  ```

---

### Notes:

- **Database Provider**: Prisma supports PostgreSQL, MySQL, SQLite, and SQL Server. Make sure the provider in `schema.prisma` matches your database.

- **Prisma Client**: Use the Prisma Client to interact with the database programmatically. It provides methods like `create`, `findMany`, `update`, and `delete`.

- **Migrations**: When you modify your data models, run `npx prisma migrate dev` to apply the changes to your database.
