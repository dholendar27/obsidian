### **[[Introduction to MongoDB]]**

- What is MongoDB?
- Features & Advantages
- MongoDB vs. Relational Databases (SQL vs NoSQL)
- MongoDB Architecture Overview
- Use Cases & Best Fit Scenarios

---

### **MongoDB Installation & Setup**

- Installing MongoDB (Windows / macOS / Linux)
- Starting & Stopping MongoDB Server
- MongoDB Shell (`mongosh`)
- MongoDB Atlas (Cloud Deployment)
- Basic Configuration and Security Setup

---

### **3. Core Concepts**

- [[Databases, Collections, and Documents]]
- [[BSON (Binary JSON) Data Format]]
- [[Schema Design & Data Modeling]]
- Primary Keys (`_id`)
- CRUD Operations Overview

---

### **[[CRUD Operations]] (Create, Read, Update, Delete)**

- **Create**: `insertOne()`, `insertMany()`
- **Read**: `find()`, Query Filters, Projections
- **Update**: `updateOne()`, `updateMany()`, Update Operators (`$set`, `$inc`, `$push`, etc.)
- **Delete**: `deleteOne()`, `deleteMany()`
- Query Operators (`$and`, `$or`, `$in`, `$exists`, `$regex`)

---

### **5. Data Modeling & Schema Design**

- [[Embedding vs. Referencing Documents]]
- [[Normalization vs. Denormalization]]
- [[One-to-One, One-to-Many, Many-to-Many Relationships]]
- [[Schema Validation Rules]]
- [[Polymorphic and Time-Series Data Modeling]]

---

### **6. [[Indexing]]**

- What are Indexes?
- Types of Indexes (Single Field, Compound, Multikey, Text, Geospatial, etc.)
- Creating and Managing Indexes
- Index Performance Considerations
- Explain Plans & Query Optimization

---

### **7. Aggregation Framework**

- Aggregation Pipeline Basics
- Common Stages (`$match`, `$group`, `$project`, `$sort`, `$limit`, `$lookup`, `$unwind`)
- Aggregation Expressions and Operators
- Map-Reduce Overview
- Performance and Optimization

---

### **8. Transactions & Atomic Operations**

- What is a Transaction?
    
- Multi-Document Transactions (Replica Sets & Sharded Clusters)
    
- Write Concerns and Read Concerns
    
- Session Management
    

---

### **9. Data Replication**

- Replica Set Overview
    
- Members, Elections, and Failover
    
- Read/Write Operations in Replica Sets
    
- Backup & Recovery
    

---

### **10. Sharding (Horizontal Scaling)**

- Why Sharding?
    
- Shard Keys
    
- Config Servers, Query Routers, and Shards
    
- Balancing & Chunk Splitting
    
- Common Sharding Patterns
    

---

### **11. Security & Authentication**

- Authentication Mechanisms (SCRAM, LDAP, x.509)
    
- Role-Based Access Control (RBAC)
    
- Encryption (At Rest & In Transit)
    
- Auditing and Logging
    
- Network Security (TLS/SSL, IP Binding, Firewalls)
    

---

### **12. Performance & Optimization**

- Profiling Queries
    
- Index Usage & Hints
    
- Hardware & Configuration Tuning
    
- Caching & Connection Pooling
    
- Common Anti-Patterns
    

---

### **13. MongoDB Tools & Utilities**

- `mongosh` (Mongo Shell)
    
- `mongodump`, `mongorestore`
    
- `mongoimport`, `mongoexport`
    
- `mongostat`, `mongotop`
    
- Compass (GUI Tool)
    

---

### **14. Working with MongoDB Drivers**

- Overview of Official Drivers
    
- Connecting via:
    
    - Node.js
        
    - Python (PyMongo)
        
    - Java
        
    - Go
        
    - C#
        
- CRUD Operations via Drivers
    
- Connection Pooling & Configuration
    

---

### **15. MongoDB Atlas (Cloud Platform)**

- Overview of MongoDB Atlas
    
- Creating and Managing Clusters
    
- Atlas CLI and API
    
- Monitoring and Alerts
    
- Serverless and Multi-Cloud Options
    

---

### **16. Advanced Topics**

- Change Streams (Real-time Data)
    
- Triggers and Functions (Serverless Logic)
    
- Time Series Collections
    
- Search Integration (`$search` with Atlas Search)
    
- Data Federation & Analytics
    

---

### **17. Backup, Restore & Migration**

- Snapshot Backups
    
- Point-in-Time Recovery
    
- Cluster Migration Strategies
    
- Data Import/Export
    

---

### **18. Testing & CI/CD**

- Unit Testing with Mocked MongoDB
    
- Using `MongoMemoryServer` in Node.js
    
- Integrating MongoDB in CI/CD Pipelines
    

---

### **19. Monitoring & Administration**

- MongoDB Cloud Manager / Atlas Monitoring
    
- Logs, Metrics, and Health Checks
    
- Replica Lag, Disk Usage, Connections
    
- Performance Dashboard
    

---

### **20. Best Practices**

- Data Modeling Guidelines
    
- Query Optimization Tips
    
- Sharding and Indexing Strategy
    
- Security Hardening
    
- Common Pitfalls & Troubleshooting
    

---

Would you like me to make this **into a clickable Markdown outline (for documentation)** or a **developer-friendly PDF/cheat sheet version** (with code examples)?
