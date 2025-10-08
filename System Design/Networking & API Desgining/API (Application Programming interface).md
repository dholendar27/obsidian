The APIs are the mechanisms that are allow the software to communicate with each other to transfer the data using set of definitions and protocols. 
~={blue}For Example=~:
The Weather app in our mobile with fetch the information from the weather bureau's

## API Architecture
API Architecture is explained in terms of client and server ([[Client-server]]). The application which is sending the request is called client. and the application which is sending the response is called the server.

There are many API's that are used world wide

![[images/Types of API.png]]
### SOAP API
SOAP (Simple object access protocol) is used for exchanging the structured information over the network. It used the XML (Extensible Markup language) for the message format. and relies on the HTTP method to send the data
#### Key Characteristics  of SOAP API
1. ~={yellow}Protocol Based=~: SOAP is a protocol with specific rules to follow for messaging
2. ~={yellow} XML Messaging:=~ Uses the XML to format the data in the request and response.
3. ~={yellow}Platform and Language Independent=~: Because it uses XML, SOAP APIs can be used across different operating systems and programming languages
4. ~={yellow}Transport Protocol=~: Primarily uses HTTP/HTTPS but can also use SMTP, TCP, or JMS.
5. ~={yellow} Extensibility=~: Supports WS-* standards for security, transactions, and addressing.
#### Structure of a SOAP Message

A SOAP message is an XML document that consists of the following elements:

``` XML
<Envelope>
  <Header> (optional)
    <!-- Metadata such as authentication info -->
  </Header>
  <Body>
    <!-- The main message or request/response data -->
  </Body>
  <Fault> (optional)
    <!-- Error and status information -->
  </Fault>
</Envelope>
```

- **Envelope**: The root element that defines the XML document as a SOAP message.
- **Header**: Contains optional attributes like security credentials or transaction information.
- **Body**: Contains the actual message payload (data or request/response).
- **Fault**: Contains error and status information if something goes wrong.
---
### REST API
REST (Representational State Transfer protocol)
- **REST** Is an architectural style for designing networked application
- **API** is a set of rules that allows different software applications to communicate with each other
A **REST API** is a way for applications to communicate over the internet using standard HTTP methods in a stateless way.

> [!NOTE]
> - In the context of a **REST API**, **resources** are the key abstractions or entities that the API exposes and operates on. They represent any kind of object, data, or service that can be accessed and manipulated over the web using standard HTTP methods.
> - We need to use the plural nouns
#### Key Characteristics  of REST API

- ==**Statelessness**==  
    Each request from the client to the server must contain all the information needed to understand and process the request. The server does not store any client context between requests.
- **Client-Server Architecture**  
    The client (frontend, mobile app, etc.) and the server (backend, database) are separate. The client sends requests; the server processes and responds.
- **Uniform Interface**  
	REST APIs have a consistent and uniform way to communicate, which simplifies interaction between components.
- **Cacheable**  
	Responses from the server can be marked as cacheable or non-cacheable to improve client-side performance.
- **Layered System**  
    The architecture can have multiple layers (load balancers, servers, caches), but clients don't know and don't need to care
#### HTTP Methods
- GET - Retrieve data
- ==POST== - Create data
- PUT - Idempotent update/replace
- PATCH - Partial update
- DELETE - Remove data 
#### Inputs
In REST API the inputs typically comes in three different ways
##### Path parameters
`GET /events/{id}`
- specify which resource we are working with
- use when required to identify the resource
##### Query parameters
`GET /events?city=LA&date=2025-01-01`
- Optional filters
##### Request body
`POST /events`
```json
{
"title": "All of us are dead",
"platform": "Netflix"
}
```

---
### GraphQL 
The GraphQL is a query language for APIs, and a runtime that lets clients ask for exactly what they need - nothing more, nothing less. It was created by facebook in 2012 and made public in 2025
Unlike REST, where we have fixed endpoints and return the fixed data structure, GraphQL allows client to control what data they get back.
##### Why GraphQL? (The Problem It Solves)

- **Over-fetching**: REST endpoints return more data than needed  
    → GraphQL lets clients request _only the fields they want_
- **Under-fetching**: REST requires multiple requests to get related data  
    → GraphQL fetches all related data in _one query_
- **Too many endpoints**: REST APIs have many endpoints to manage  
    → GraphQL uses _one single endpoint_ for all data
- **API versioning issues**: Changing REST APIs can break clients  
    → GraphQL APIs evolve safely by _adding fields without breaking clients_
- **Mobile app optimization**: REST often sends large payloads  
    → GraphQL reduces data transfer by _sending precise data_
##### Common Issues with GraphQL & Solutions

| Issue                        | Description                                                             | Solution                                                                     |
| ---------------------------- | ----------------------------------------------------------------------- | ---------------------------------------------------------------------------- |
| **Overly Complex Queries**   | Clients can make deeply nested, expensive queries that slow down server | Limit query depth (e.g., `graphql-depth-limit`), complexity scoring          |
| **Caching Difficulties**     | Single `/graphql` POST requests make HTTP caching hard                  | Use Apollo Client cache, persistent queries, CDN with GraphQL caching        |
| **Authorization Challenges** | Clients can query any field, risking data leaks                         | Implement field-level authorization in resolvers, use GraphQL Shield         |
| **N+1 Problem**              | Multiple DB queries per request due to nested resolvers                 | Use DataLoader for batching & caching, eager loading via ORM                 |
| **Schema Complexity**        | Large schemas can be hard to maintain                                   | Use schema stitching/federation, maintain docs with tools like Apollo Studio |

---
### gRPC (google Remote Procedure Call)

- **gRPC** (Google Remote Procedure Call) is a high-performance, open-source framework for remote procedure calls.
- Enables communication between services or applications by calling methods on remote servers as if they were local functions.
- Ideal for connecting microservices or distributed systems efficiently.
#### Key Features

- Uses **Protocol Buffers (protobuf)** for defining service interfaces and serializing data—faster and smaller than JSON/XML.
- Runs on **HTTP/2**, which provides:
    - Multiplexing multiple requests on one connection.
    - Full-duplex streaming (both client and server can send data simultaneously).
    - Header compression for efficient network usage.
- Supports multiple programming languages like Go, Python, Java, C++, Node.js, C#, and more.
- Supports four types of RPC calls:
    1. **Unary RPC:** One request, one response.
    2. **Server Streaming:** One request, many responses.
    3. **Client Streaming:** Many requests, one response.
    4. **Bidirectional Streaming:** Many requests and responses exchanged simultaneously.
- Built-in features include authentication, deadlines/timeouts, retries, and load balancing.
#### How gRPC Works (Conceptual Flow)

1. Define your **service and message types** in a `.proto` file using Protocol Buffers syntax.
2. Use the protobuf compiler to **auto-generate client and server code** in your chosen languages.
3. Implement the server-side logic for the service.
4. Write client-side code that calls remote methods transparently.
5. gRPC takes care of **serializing data, sending it over the network**, and **deserializing** on the other end.
#### Why Use gRPC?

- **Performance:** Efficient binary serialization + HTTP/2 make communication fast and lightweight.
- **Strongly typed APIs:** Auto-generated code reduces bugs by enforcing correct data types.
- **Streaming support:** Great for real-time updates, live data feeds, or any use case needing continuous data flow.
- **Cross-language interoperability:** Easily connect services written in different programming languages.
- **Built-in features:** Security (TLS), deadlines, retries, load balancing simplify complex distributed system requirements.
