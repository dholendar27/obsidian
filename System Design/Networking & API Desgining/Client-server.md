In client-server model where the clients send a request to the server. The server then process the request and provide the reponse to the client.

Example:

User request browser to fetch a particular video from the youtube. The browser(Client) send a request to the server. the server process the request and send back the response to the browser.

![[images/Client-server.png]]

The Client-server architecture is a network model in the clinet and model are the two entites.

## Types of Client-server Architecture

The core idea behind **multi-tier architecture** (e.g. two-tier, three-tier, or even n-tier) is **separation of concerns** — dividing the system into distinct layers, each with a specific responsibility.

### **One-Tier architecture:**

In this architecture the infertace, databse layer, application logic running on the same machine. This models is good for simple application with few users.

### Two-Tier **architecture:**

In this layer the system is split into two distinct layers. one running on the client and other part is running on the server. 

### Three-Tier **architecture**

In this layer the system is split into three distinct layers. each with specific role:

    1. Presentation Tier (UI Layer)
    2. Application Tier (Logic Layer)
    3. Data Tier (Database Layer)

This type if architecture is useful to for scaling independently. 

### N-Tier Architecture

The N-tier provides an optiont o add more layers to fit specific application needs. we can add different layers based on the user choice. But adding the more layers can be challenging as the layers increase the complexities such as handling traffice. handling bottlenecks.

> Choosing the right number of tiers is a **balancing act**:
> - A **simple app** might work fine with a two-tier setup.
> - A **large-scale enterprise system** may need a full three-tier or n-tier design for maintainability and performance.
>The goal is to find the **right architecture** for your application’s size, complexity, and future growth needs — without overengineering.

## Pros and cons of Client-Server Architecture

### Pros

- **Centrailized Control:** Where the server can accepts the request from multiple clients.
- **Scalability:** Allowing developers to add more clinets or servers as needed without affecting the entire system.
- **Data Security:** servers can be secured with layers of securtiy controls, protecting sensitive information and user data.

### cons

1. **Single Point of Failure**: If the server goes down, all connected clients lose access to data and services.
2. **Network Dependency**: Since the model relies on network communication, poor connectivity can lead to performance issues.
3. **Resource-Intensive**: Servers require significant resources to manage and serve multiple clients, which may increase infrastructure costs.