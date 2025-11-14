In MongoDB, **schema validation** is the process of enforcing rules to ensure that the data inserted or updated into a collection meets specific criteria. MongoDB allows you to define these rules at the collection level using **JSON Schema** or custom validation expressions.

MongoDB’s schema validation feature was introduced in **version 3.6** and can help ensure data consistency and integrity without requiring an external tool or application logic to enforce the rules.

### **Key Concepts of MongoDB Schema Validation**

- **Validation Action**: This defines what happens when a document doesn't conform to the validation rules (e.g., reject or warn).
- **Validation Level**: This defines when the validation rules are enforced (e.g., before or after an insert or update).
- **Validation Rules**: These define the conditions that must be met by documents in the collection. MongoDB supports both **JSON Schema** and **custom expressions** for validation.

### **Types of Schema Validation in MongoDB**

1. **JSON Schema Validation**
2. **Custom Validation Using MongoDB Expressions**

Let’s go through both in more detail.

---

### **1. JSON Schema Validation**

**JSON Schema** is a powerful and flexible tool for defining the structure and validation rules of documents. MongoDB supports a subset of the JSON Schema specification, allowing you to define properties like types, required fields, and specific constraints for your documents.

#### Basic Structure of JSON Schema Validation:

- `type`: Specifies the expected type of a field (e.g., `string`, `number`, `object`).
- `properties`: Defines the fields that the document should have, along with their validation rules.
- `required`: Specifies an array of required fields.
- `minLength`, `maxLength`: Defines the minimum or maximum length for a string.
- `enum`: Restricts a field to a specific set of values.

#### Example:

Let’s say we want to enforce the following validation rules for a **users** collection:

- Each user document must have a `username` (string), which is required and must be at least 3 characters long.
    
- Each user document must have an `email` (string) that should be a valid email format.
    
- Age should be an integer greater than or equal to 18.
    

Here’s how we could define a **JSON Schema** for this validation:

```javascript
db.createCollection("users", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["username", "email", "age"],
      properties: {
        username: {
          bsonType: "string",
          minLength: 3,
          description: "Username is required and must be at least 3 characters"
        },
        email: {
          bsonType: "string",
          pattern: "^.+@.+\..+$",  // Regular expression for valid email format
          description: "Email is required and must be a valid email address"
        },
        age: {
          bsonType: "int",
          minimum: 18,
          description: "Age must be an integer and greater than or equal to 18"
        }
      }
    }
  },
  validationAction: "error"  // Reject documents that don't meet the schema
});
```

In this schema:

- The `username`, `email`, and `age` fields are **required**.
- The `username` must be at least **3 characters** long.
- The `email` must follow a valid **email format** (using a regular expression).
- The `age` must be an **integer** greater than or equal to 18.

---

### **2. Custom Validation Using MongoDB Expressions**

If you need more complex validation that cannot easily be expressed with JSON Schema alone, MongoDB also allows **custom expressions** to define validation rules.

Custom validation can be defined using MongoDB’s query language, which supports operators like `$gt`, `$lt`, `$in`, `$regex`, etc., to perform complex checks on fields.

#### Example of Custom Validation:

Let’s say we have a **products** collection, and we want to enforce the following validation rules:

- The `price` field must always be greater than 0.
    
- The `category` field must be one of a predefined set of values.
    

Here’s how you could define custom validation using **MongoDB Expressions**:

```javascript
db.createCollection("products", {
  validator: {
    $and: [
      { price: { $gt: 0 } },  // price must be greater than 0
      { category: { $in: ["electronics", "clothing", "furniture"] } }  // category must be one of the defined values
    ]
  },
  validationAction: "error"  // Reject documents that don't meet the validation criteria
});
```

In this case:

- The `price` must be greater than `0`.
    
- The `category` must be one of the values: `electronics`, `clothing`, or `furniture`.
    

---

### **Validation Action and Level**

When creating a collection with validation, you can specify the **validation level** and **action**:

1. **Validation Action**:
    
    - **"error"**: The document is rejected if it does not meet the validation rules (this is the default).
        
    - **"warn"**: The document is inserted or updated, but a warning is logged.
        
    
    Example:
    
    ```javascript
    validationAction: "warn"
    ```
    
    This would allow the document to be inserted, but a warning would be logged if it doesn't conform to the schema.
    
1. **Validation Level**:
    - **"strict"**: Applies validation to both **inserts and updates**. If a document doesn't meet the validation rules during either operation, it is rejected.
    - **"moderate"**: Validation is only applied when a document is **inserted**. Updates to existing documents are not validated.
    - **"off"**: Validation is completely **disabled** (not recommended for most production environments).
    
    Example:
```javascript
   validationLevel: "strict"
```

This ensures that validation is applied both when inserting **new documents** and when **updating existing documents**.

---

### **Examples of MongoDB Schema Validation**

#### Example 1: **User Registration Schema**

```javascript
db.createCollection("users", {
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["username", "email", "password"],
      properties: {
        username: {
          bsonType: "string",
          minLength: 3,
          description: "Username must be a string and at least 3 characters"
        },
        email: {
          bsonType: "string",
          pattern: "^.+@.+\..+$", // Basic email validation using regex
          description: "Email must be a valid email address"
        },
        password: {
          bsonType: "string",
          minLength: 8,
          description: "Password must be at least 8 characters"
        }
      }
    }
  },
  validationAction: "error",  // Reject invalid documents
  validationLevel: "strict"   // Apply validation on both inserts and updates
});
```

#### Example 2: **Order Schema with Custom Validation**

```javascript
db.createCollection("orders", {
  validator: {
    $and: [
      { totalAmount: { $gte: 0 } },  // totalAmount must be >= 0
      { status: { $in: ["pending", "shipped", "delivered", "cancelled"] } }  // status must be one of these values
    ]
  },
  validationAction: "error",  // Reject invalid documents
  validationLevel: "moderate"  // Validation only on inserts, not updates
});
```

---

### **Modifying Validation Rules**

If you want to **modify the validation rules** of an existing collection, you can use the `collMod` command.

#### Example of Modifying Validation Rules:

```javascript
db.runCommand({
  collMod: "users",
  validator: {
    $jsonSchema: {
      bsonType: "object",
      required: ["username", "email", "password"],
      properties: {
        username: {
          bsonType: "string",
          minLength: 3
        },
        email: {
          bsonType: "string",
          pattern: "^.+@.+\..+$"
        },
        password: {
          bsonType: "string",
          minLength: 8
        }
      }
    }
  },
  validationAction: "error",
  validationLevel: "strict"
});
```

This command would **update the validation rules** for the **users** collection to ensure they meet the newly defined schema.

---

### **Conclusion**

MongoDB’s schema validation provides flexibility to enforce data integrity at the collection level using JSON Schema or custom expressions. Whether you are defining simple types or creating complex rules for your documents, MongoDB's validation capabilities give you a powerful way to ensure the data structure is consistent, accurate, and follows your application’s rules.

Key points:

- **JSON Schema Validation** is ideal for enforcing specific data types and structures.
- **Custom Validation** allows for more flexible and complex rules.
- You can set **validation actions** (e.g., "error", "warn") and **levels** ("strict", "moderate") based on your needs.
- Schema validation helps maintain **data integrity** and can reduce application logic complexity.