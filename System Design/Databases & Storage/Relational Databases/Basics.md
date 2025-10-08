## What are relational databases
The relation databases store and organize data in structured table with predefined relationships, allowing for efficient access and management of related information.
> It means that the data is store in table with rows and columns. where the columns are the attributes and row are the records
##  **Tables, Rows, and Columns**

In a relational database, data is stored in **tables**.
- **Table**: Think of a table as a collection of data organized into rows and columns, like a spreadsheet.
- **Column**: Each **column** contains a specific type of data. Each column has a name and holds data of a particular type (e.g., text, numbers, dates).
- **Row**: A **row** represents a single record in the table. Each row contains data for all the columns defined in that table.
## Relationships  (1:1, 1:N, M:N)
These relationships define how data in one table connects to data in another table. Let’s look at the three main types of relationships:
### 1:1 Relationship (one-to-one)
In a 1:1 relationship, each record in one table is linked exactly to one record in another table. 
#### Example:
We have two tables:
- Students (where each student have a unique records)
- StudentAddress (where each student have exactly one address)
### 1:N Relationship (One-to-Many)
A **1:N relationship** (One-to-Many) occurs when one record in a table is associated with **many** records in another table, but each record in the second table is associated with **only one** record in the first table.
#### Example:
let's consider we have 
- Students (one student)
- Enrollments (many enrollments to each students)
One student can enroll to different class. suppose there is a student with id 1001 and he likes maths and science. 
### **M:N Relationship (Many-to-Many)**
A **M:N relationship** (Many-to-Many) occurs when **many records in one table are associated with many records in another table**. This means each record in both tables can be related to multiple records in the other table.
#### Example of a M:N Relationship:
Let’s consider:
- **Students** (many students)
- **Courses** (many courses)
## Primary Key, Foreign Key, Unique Constraints
#### **Primary Key**
A **Primary Key** is a **column (or a set of columns)** that uniquely identifies each row in a table. It ensures that every record in the table is unique and can be identified without ambiguity.
#### Key Characteristics of Primary Key:
- **Uniqueness**: No two rows can have the same value for the primary key column.
- **Non-null**: A primary key cannot have a `NULL` value because it needs to uniquely identify each record.
- **Single or Composite**: A primary key can be made up of one column (a **simple primary key**) or multiple columns (a **composite primary key**).
#### **Foreign Key**
A **Foreign Key** is a column (or set of columns) in one table that **references the primary key of another table**. It creates a link between two tables, establishing a **relationship** between them.
#### Key Characteristics of Foreign Key:
- **References another table’s Primary Key**: A foreign key points to the primary key of another table (or within the same table in case of a self-reference).
- **Data Integrity**: It ensures that the values in the foreign key column exist in the referenced table, which helps maintain **referential integrity**.
## Database Normalization: 1NF, 2NF, and 3NF

### 1NF (First Normal Form)

#### Definition:
- A table is in **1NF** if **all columns contain atomic (indivisible) values**.
- There are **no repeating groups or arrays**.
- Each row is unique (has a primary key).
#### How to Check 1NF:
- Ensure no column contains multiple values or lists.
- Each cell must hold only one value.
- There should be a unique identifier (primary key) for rows.
#### Example:

|Student_ID|Name|Courses|
|---|---|---|
|1|Alice|Math, Science|
|2|Bob|History|

**Problem:**  
The `Courses` column contains multiple values (Math, Science).

**Fix (1NF):**

|Student_ID|Name|Course|
|---|---|---|
|1|Alice|Math|
|1|Alice|Science|
|2|Bob|History|

---

### 2NF (Second Normal Form)

#### Definition:
- A table is in **2NF** if:
    - It is already in **1NF**.
    - **No partial dependency exists**; i.e., **non-key columns depend on the whole primary key**, not just part of it.
- Applies when the primary key is composite (made of multiple columns).
#### How to Check 2NF:
- Identify if the primary key is composite.
- For each non-key attribute, check if it depends on the entire key or just part of it.
- If any attribute depends only on part of the key, it violates 2NF.
#### Example:

|Student_ID|Course_ID|Student_Name|Course_Name|
|---|---|---|---|
|1|101|Alice|Math|
|2|102|Bob|Science|

- Primary Key = (Student_ID, Course_ID)
- `Student_Name` depends only on `Student_ID` → partial dependency
- `Course_Name` depends only on `Course_ID` → partial dependency

**Fix (2NF):**
Split into separate tables:
**Student Table:**

| Student_ID | Student_Name |
|------------|--------------|
| 1          | Alice        |
| 2          | Bob          |

**Course Table:**

| Course_ID | Course_Name |
|-----------|-------------|
| 101       | Math        |
| 102       | Science     |

**Enrollment Table:**

| Student_ID | Course_ID |
|------------|-----------|
| 1          | 101       |
| 2          | 102       |

---
### 3NF (Third Normal Form)
#### Definition:
- A table is in **3NF** if:
    - It is already in **2NF**.
    - **No transitive dependency exists**; i.e., **non-key attributes must not depend on other non-key attributes**.
#### How to Check 3NF:
- For each non-key attribute, check if it depends on the primary key directly.
- If a non-key attribute depends on another non-key attribute, it violates 3NF (transitive dependency).
#### Example:

|Course_ID|Course_Name|Instructor_Name|Instructor_Phone|
|---|---|---|---|
|101|Math|Mr. Smith|1234567890|

- Primary Key = Course_ID
- `Instructor_Phone` depends on `Instructor_Name` (non-key attribute) → transitive dependency.

**Fix (3NF):**
Split into two tables:
**Course Table:**

| Course_ID | Course_Name | Instructor_Name |
|-----------|-------------|-----------------|
| 101       | Math        | Mr. Smith       |

**Instructor Table:**

| Instructor_Name | Instructor_Phone |
|-----------------|------------------|
| Mr. Smith       | 1234567890       |

---
### Summary of How to Check Normal Forms:

| Normal Form | What to Check                                                          | Fix if Violated                                |
| ----------- | ---------------------------------------------------------------------- | ---------------------------------------------- |
| 1NF         | No multi-valued columns, atomic values only                            | Split multi-valued columns                     |
| 2NF         | No partial dependency (non-key attrs depend on full composite key)     | Split tables to remove partial dependencies    |
| 3NF         | No transitive dependency (non-key attrs depend on other non-key attrs) | Split tables to remove transitive dependencies |
