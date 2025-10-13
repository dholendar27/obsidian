### **Authentication**

- **What it is**: Authentication is the process of verifying the identity of a user, system, or entity. It answers the question: _Who are you?_
- **How it's done**: Typically through credentials like usernames, passwords, biometric data (fingerprints, face recognition), or tokens (e.g., OAuth or two-factor authentication).
- **Purpose**: It ensures that the person or system requesting access is who they claim to be.
- **Example**: When you log into your email by entering your username and password, the system authenticates you to confirm that you're the correct user.

### **Authorization**
- **What it is**: Authorization is the process of determining what actions or resources a user is allowed to access, once their identity has been authenticated. It answers the question: _What can you do?_
- **How it's done**: Authorization is often handled using roles, permissions, or access control lists (ACLs), where the authenticated user is granted specific rights (read, write, delete, etc.) based on their role or status.
- **Purpose**: It ensures that users can only access resources or perform actions that they have permission to do.
- **Example**: After logging in, you may be able to access your inbox, but not the admin panel, depending on your role. The system checks your permissions and decides what you can or cannot do.
### The Difference in Action:

- **Authentication**: "Who are you?" → A user enters their username and password. The system checks if those match.
- **Authorization**: "What can you do?" → After authenticating, the system checks your access rights (e.g., you can edit documents, but you cannot delete files).

---

**Summary**:

- **Authentication**: Verifies identity (Who you are).
- **Authorization**: Grants access to resources based on identity (What you can do).
