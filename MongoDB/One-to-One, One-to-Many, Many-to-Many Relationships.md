---
date created: 2025-11-14 10:09
---

In any database system, relationships describe how entities (or documents in MongoDB) are related to one another. MongoDB, being a **NoSQL document database**, doesn't use tables and foreign keys in the same way as relational databases. Instead, relationships are modeled using **embedding** (denormalization) or **referencing** (normalization). The types of relationships you model depend on your application’s requirements.

Let’s explore the three most common types of relationships in MongoDB: **One-to-One**, **One-to-Many**, and **Many-to-Many**.

---

### 1. **One-to-One Relationship**

In a **one-to-one** relationship, each document in one collection is associated with one and only one document in another collection. There’s a **direct mapping** between the documents in both collections.

#### When to use:

- When you need to split large or complex documents into smaller parts (for example, user profile details vs. their security settings).
- When you want to store separate data but keep it tightly related, where the entities can’t exist independently of each other.

#### Example:

Let’s say you have a **Users** collection and each user has one **Profile** (for example, a profile picture, bio, etc.).

- **Users Collection**:

  ```json
  {
    "_id": ObjectId("12345"),
    "username": "alice",
    "email": "alice@example.com",
    "profile_id": ObjectId("67890")  // Reference to Profile
  }
  ```

- **Profiles Collection**:

  ```json
  {
    "_id": ObjectId("67890"),
    "user_id": ObjectId("12345"),  // Reference to Users
    "bio": "Alice loves coding.",
    "profile_picture": "path/to/picture.jpg"
  }
  ```

In this case, the **Users** collection stores a reference to the **Profiles** collection, and vice versa, using ObjectIds. The relationship between a user and their profile is **one-to-one**.

#### How to model it in MongoDB:

- **Embedding**: If the profile information is very small (and unlikely to change independently), you can embed the profile data inside the user document.

  ```json
  {
    "_id": ObjectId("12345"),
    "username": "alice",
    "email": "alice@example.com",
    "profile": {
      "bio": "Alice loves coding.",
      "profile_picture": "path/to/picture.jpg"
    }
  }
  ```

- **Referencing**: For more complex data or if the profile might change frequently, storing the profile in a separate collection and referencing it from the users is better.

---

### 2. **One-to-Many Relationship**

In a **one-to-many** relationship, one document in the "one" collection can be associated with multiple documents in the "many" collection, but each document in the "many" collection can reference only one document in the "one" collection.

#### When to use:

- When a single entity has multiple related sub-entities. For example, a **customer** can have multiple **orders**.

- When you want to store related data that is logically grouped together but can grow independently (such as products in a catalog, which may have reviews).

#### Example:

Let’s say you have an **Authors** collection and an **Books** collection. An **author** can write multiple **books**, but each **book** is written by only one **author**.

- **Authors Collection**:

  ```json
  {
    "_id": ObjectId("111"),
    "name": "George Orwell",
    "email": "george.orwell@example.com"
  }
  ```

- **Books Collection**:

  ```json
  {
    "_id": ObjectId("222"),
    "title": "1984",
    "author_id": ObjectId("111"),  // Reference to Authors
    "publish_date": "1949-06-08"
  },
  {
    "_id": ObjectId("223"),
    "title": "Animal Farm",
    "author_id": ObjectId("111"),
    "publish_date": "1945-08-17"
  }
  ```

In this example, each **author** can have multiple **books**, but each **book** is written by only one **author**. The relationship is **one-to-many** (one author, many books).

#### How to model it in MongoDB:

- **Embedding**: If the "many" documents are small and won't be updated frequently, you can embed the related documents within the "one" document.

  ```json
  {
    "_id": ObjectId("111"),
    "name": "George Orwell",
    "email": "george.orwell@example.com",
    "books": [
      { "title": "1984", "publish_date": "1949-06-08" },
      { "title": "Animal Farm", "publish_date": "1945-08-17" }
    ]
  }
  ```

- **Referencing**: If the "many" documents are large or frequently updated (e.g., books might have reviews that change over time), referencing is better.

  - In this case, the **Books** collection stores a reference to the **Authors** collection (via `author_id`).

---

### 3. **Many-to-Many Relationship**

In a **many-to-many** relationship, multiple documents in one collection can be associated with multiple documents in another collection. This is typically modeled with a **junction table** (or a linking collection in MongoDB) that holds references to both sides of the relationship.

#### When to use:

- When you have entities that can have multiple relationships with other entities, and these relationships are not easily represented with a one-to-one or one-to-many model.

- For example, a **student** can enroll in many **courses**, and a **course** can have many **students**.

#### Example:

Let’s say you have **Students** and **Courses**. A **student** can enroll in multiple **courses**, and a **course** can have multiple **students**.

- **Students Collection**:

  ```json
  {
    "_id": ObjectId("101"),
    "name": "Alice",
    "email": "alice@example.com"
  },
  {
    "_id": ObjectId("102"),
    "name": "Bob",
    "email": "bob@example.com"
  }
  ```

- **Courses Collection**:

  ```json
  {
    "_id": ObjectId("201"),
    "course_name": "Math 101"
  },
  {
    "_id": ObjectId("202"),
    "course_name": "Physics 101"
  }
  ```

- **Enrollments (Junction) Collection**:

  ```json
  {
    "student_id": ObjectId("101"),
    "course_id": ObjectId("201")
  },
  {
    "student_id": ObjectId("101"),
    "course_id": ObjectId("202")
  },
  {
    "student_id": ObjectId("102"),
    "course_id": ObjectId("201")
  }
  ```

In this case, the **Enrollments** collection is the junction that links students and courses. Each record represents an enrollment of a student in a course. This forms a **many-to-many** relationship because:

- One student can be enrolled in many courses.

- One course can have many students.

#### How to model it in MongoDB:

- **Embedding**: For simple cases, you might embed arrays of references to related documents within each document (e.g., embedding course IDs in the student document). However, this becomes inefficient when there are many relationships or if the data grows.

  ```json
  {
    "_id": ObjectId("101"),
    "name": "Alice",
    "email": "alice@example.com",
    "enrolled_courses": [
      ObjectId("201"),  // Math 101
      ObjectId("202")   // Physics 101
    ]
  }
  ```

- **Referencing**: The best practice for a many-to-many relationship is to use a **junction collection** (like the **Enrollments** collection above), which stores references to both the `student_id` and `course_id`.

---

### Summary

1. **One-to-One Relationship**:

   - One document in collection A is related to one document in collection B.
   - Use embedding or referencing depending on how often data changes.
   
1. **One-to-Many Relationship**:

   - One document in collection A is related to multiple documents in collection B.
   - Use referencing for scalability and flexibility.

1. **Many-to-Many Relationship**:

   - Multiple documents in collection A can be related to multiple documents in collection B.
   - Best modeled with a junction collection to store references to both entities.

In MongoDB, the choice between **embedding** and **referencing** for each relationship depends on factors like query patterns, data size, and how often the data is updated. Generally, **embedding** is more suitable for smaller, read-heavy relationships, while **referencing** is better for more complex, large, and frequently updated relationships.
