---
date created: 2025-12-02 14:04
date updated: 2025-12-02 14:51
---

## What is WebSocket?

WebSocket is a **communication protocol** that enables full-duplex, real-time communication between a client (usually a web browser) and a server. Unlike the traditional HTTP request-response model, where the client sends a request and the server responds, WebSockets allow for continuous two-way communication once the connection is established. This means both the client and server can send and receive messages at any time, without the overhead of repeated handshakes or HTTP requests.

In simpler terms, WebSocket is like opening a "persistent" connection between the client and server, where both can talk to each other continuously.

## Difference between WebSockets and HTTP

| **Aspect**                 | **HTTP (Request-Response Model)**                                                 | **WebSocket (Full-Duplex Communication)**                              |
| -------------------------- | --------------------------------------------------------------------------------- | ---------------------------------------------------------------------- |
| **Communication Style**    | Client sends a request; server responds once (stateless)                          | Full-duplex: Both client and server can send messages anytime          |
| **Connection**             | Connection is closed after the response                                           | Connection remains open for continuous communication                   |
| **Overhead**               | New request needed for more data, leading to overhead (headers, connection setup) | Minimal overhead once connection is established                        |
| **Real-Time Applications** | Not ideal for real-time updates; needs new requests for each update               | Ideal for real-time applications (e.g., live chats, multiplayer games) |
| **Example**                | Web browser requests data (HTML, CSS, JS) from a server                           | Real-time chat application where messages flow continuously            |

---

## How WebSocket Works:

#### Handshaking Process (HTTP Upgrade Request/Response):

The WebSocket connection begins as an **HTTP request** (since WebSocket uses port 80/443 and initially uses the HTTP protocol to establish the connection), but it then upgrades to the WebSocket protocol for full-duplex communication.

- **Client Side:**

  1. The client (browser) sends an **HTTP request** to the server, but with a special header indicating that it wants to "upgrade" to WebSocket.

     - Request example:

```
GET /chat HTTP/1.1 
Host: example.com 
Upgrade: websocket 
Connection: Upgrade 
Sec-WebSocket-Key: x3JJHMbDL1EzLkh9iD5rPbzH6Bqs7WQ7P0Lh/04jIDg= 
Sec-WebSocket-Version: 13
```

2. The `Sec-WebSocket-Key` header is a base64-encoded string that the server uses to generate a response. This key ensures the connection is secure and prevents hijacking.

- **Server Side:**
  1. The server receives the request and checks if it supports WebSockets and the request is valid.
  2. If the server is ready to upgrade, it sends a **HTTP 101 response** (Switching Protocols), which means the server is willing to accept the WebSocket protocol and switch from HTTP to WebSocket communication.

     - Response example:

       `HTTP/1.1 101 Switching Protocols Upgrade: websocket Connection: Upgrade Sec-WebSocket-Accept: dGhlIHNhbXBsZSBub25jZQ==`

Once the handshaking process is successful, the connection is upgraded to WebSocket, and both the client and the server can send messages over the same open connection.

---

## Opening, Closing, and Error Frames:

WebSockets use a specific framing mechanism to communicate between the client and the server. This ensures that data is sent in manageable packets and can be understood by both sides.

- **Opening Frame (Handshake Request/Response):**\
  The initial handshake between the client and server, as we discussed, is done using the HTTP protocol and involves the upgrade request and response headers.

- **Message Frame:**\
  Once the connection is established, both the client and server send messages in **frames**. Each frame contains a small chunk of data, which could be part of a larger message or the entire message.

- **Closing Frame:**\
  When either the client or the server wants to close the WebSocket connection, they send a closing frame. This frame indicates that the connection should be closed.

  - The closing frame contains an optional **close code** (a status code that can explain why the connection is being closed).
  - Example close code: 1000 (normal closure).

- **Error Frame:**\
  If something goes wrong during the WebSocket communication (like an invalid message or unexpected disconnect), an **error frame** is sent. This frame signals that the connection should be closed due to an error.

---

## 1. WebSocket URL Syntax: `ws://` vs `wss://`

When connecting to a WebSocket server, you use a URL, similar to how you would use HTTP URLs (`http://`) or HTTPS URLs (`https://`). The difference is that WebSocket has its own schemes: `ws://` and `wss://`.

#### `ws://` (WebSocket - Non-secure)

- **Syntax:** `ws://hostname:port/path`
- This is used for **non-secure WebSocket connections**, typically on `port 80` (the same port as HTTP).
- **Example:** `ws://example.com/chat`
- **When to use:** Typically used in development environments, or when you don’t need encryption (though not recommended for production due to security concerns).

#### **`wss://` (WebSocket Secure)**

- **Syntax:** `wss://hostname:port/path`
- This is the **secure WebSocket protocol**, which is WebSocket over TLS/SSL (essentially the WebSocket equivalent of HTTPS).
- **Example:** `wss://example.com/chat`
- **When to use:** Always in production environments, especially when handling sensitive data (like in real-time financial apps, chat apps, etc.). This ensures encrypted communication, protecting data from eavesdropping.

**Key Difference:**

- **`ws://`** is for non-secure connections.
- **`wss://`** is for secure connections (encrypted using SSL/TLS).

If you're building a production application, **always prefer `wss://`** unless you’re specifically working in a controlled, local environment.

---

### **2. Frames in WebSocket**

WebSocket communication is **frame-based**, meaning messages are sent in smaller chunks called **frames**. These frames are the building blocks of the entire WebSocket protocol, and they help in organizing and sending data efficiently.

There are three primary types of frames in WebSocket:

#### **1. Text Frame:**

- **Purpose:** Carries **text data**, typically encoded in **UTF-8** format.
- **Structure:**
  - A **Text frame** has a **fin** bit (indicating if it's the last frame of the message) and a **payload length** that can vary (from 0 to 125 bytes for small payloads, or longer for large ones).
  - The data is always interpreted as a **UTF-8 encoded string**.
- **Example:**
  If a WebSocket server sends a message like "Hello!", it’s packaged into one or more **Text Frames**.

#### **2. Binary Frame:**

- **Purpose:** Carries **binary data** (such as images, videos, or files).
- **Structure:**
  - Binary frames use the same structure as text frames but are designed to handle binary payloads.
  - WebSocket supports two types of binary frames:
    - **`ArrayBuffer`** (for handling binary data directly).
    - **`Blob`** (in browsers, when handling files like images).
- **Example:** Sending a binary file like an image over WebSocket.

#### **3. Control Frame:**

- **Purpose:** Used for managing the WebSocket connection itself (e.g., keeping the connection alive, handling closure).

- **Types of Control Frames:**
  - **Ping Frame**: Sent to check if the connection is still alive (we’ll cover this in the next section).
  - **Pong Frame**: Sent in response to a Ping frame to indicate that the connection is still active.
  - **Close Frame**: Signals that the WebSocket connection is closing.

---

### **3. Message Fragmentation and Reassembly**

WebSocket allows you to **fragment** large messages into smaller pieces, called frames, and then reassemble them on the receiving end. This is useful for sending large data that can't fit in a single frame.

#### **How it works:**

- **Fragmenting messages**: If the message is too large to fit in a single frame (i.e., its payload length exceeds the maximum size that can be transmitted in one frame), WebSocket will **split** it into multiple smaller frames.
- **Reassembling**: The receiver, upon receiving these frames, will **reassemble** them back into the original message.
- The **final frame** of a fragmented message has the `fin` (final) bit set to `1`, which signals the end of the message.

#### **Example:**

- A WebSocket message like `"This is a very long message that will be fragmented into smaller frames"` may be split into several frames, and each frame will contain a part of the message. Once the last frame is received, the server or client will combine the fragments and process the complete message.

#### **Important Notes:**

- Frame fragmentation is automatically handled by the WebSocket protocol. As a developer, you don’t need to manually manage fragmentation when using WebSocket APIs in most cases (unless you're working with very large files, like video/audio).
- Fragmentation is important when dealing with **large binary data** like files.

---

### **4. Ping/Pong Frames for Connection Health Checks**

WebSockets provide a **Ping-Pong** mechanism to ensure that the connection is still active and to prevent timeouts from intermediary devices (like firewalls or proxies) that might close idle connections.

- **Ping Frame**:
  - The client or server sends a **Ping** frame to the other side to check if the connection is still alive.
  - The frame can carry a small payload (although it’s often empty).
- **Pong Frame**:
  - When the recipient of the Ping frame receives it, they respond by sending a **Pong** frame.
  - The Pong frame may carry back the same payload (if included) as the Ping frame to confirm the connection is active.

#### **When Ping/Pong Frames are Useful:**

- Preventing **connection timeouts** due to idle connections.
- Ensuring the server or client is still responsive and not stuck or dead.
- For example, in long-running sessions like multiplayer games, you don’t want the connection to be prematurely closed due to inactivity.

#### **Example of Ping/Pong:**

- The client sends a Ping frame: `Ping: "Are you still there?"`
- The server responds with a Pong frame: `Pong: "Yes, I’m here!"`

---

### **5. Close Frame (Handling Graceful Disconnection)**

The **Close Frame** is used to initiate and gracefully handle the closure of a WebSocket connection. Both the client and server can send a Close frame to end the communication.

#### **Key Points About Close Frames:**

- **Graceful Closure:** The Close frame ensures a **clean shutdown** of the connection without abrupt termination, which helps avoid data loss or corruption.
- **Close Code:** The Close frame can optionally include a **status code** to explain why the connection is being closed. These codes are defined by the WebSocket protocol and can be useful for debugging or handling specific closure scenarios.

  - **1000**: Normal closure (the connection is closed without error).
  - **1001**: Going away (server or client is shutting down).
  - **1002**: Protocol error (an unexpected protocol behavior).
  - **1003**: Unsupported data (the connection is being closed because the data is not understood).

#### **Process of Closing:**

1. **Initiating closure:** One side (either client or server) sends a Close frame.
2. **Acknowledging closure:** The other side sends a Close frame in response. The connection can now be safely closed.
3. **Timeout:** If no response is received within a reasonable time, the connection can be forcefully closed by the sender.

---

## 1. Browser-Side WebSocket API

The **WebSocket API** is a built-in feature in modern web browsers that allows JavaScript to connect to WebSocket servers and interact with them in real-time. It’s simple to use and provides various methods and events to manage communication between the client (browser) and the server.

To interact with WebSockets in the browser, you’ll use the `WebSocket` constructor, which is part of the **Window** object in the browser.

#### **Basic Syntax:**

```javascript
const socket = new WebSocket(url);
```

- **`url`**: This is the WebSocket server's URL, either `ws://` (non-secure) or `wss://` (secure) followed by the server's address and any path (e.g., `wss://example.com/chat`).

Once the `WebSocket` instance is created, you can start working with the connection and handle messages/events.

---

### **2. Creating a WebSocket Connection**

Creating a WebSocket connection involves calling the `WebSocket` constructor with the appropriate server URL. The connection starts immediately after the constructor is called.

#### **Example:**

```javascript
// Creating a WebSocket connection to a server
const socket = new WebSocket('wss://example.com/socket');

// Handling the open event (when the connection is successfully established)
socket.onopen = function(event) {
  console.log("Connection established:", event);
};

// Handling the error event (if something goes wrong)
socket.onerror = function(error) {
  console.error("WebSocket Error:", error);
};

// Handling the close event (when the connection is closed)
socket.onclose = function(event) {
  console.log("Connection closed:", event);
};
```

- **`onopen`**: Triggered when the connection is successfully established.
- **`onerror`**: Triggered if there's an error with the connection.
- **`onclose`**: Triggered when the connection is closed, either by the client, server, or due to network issues.

---

### **3. Sending Messages with `send()`**

Once the WebSocket connection is open, you can send messages to the server using the `send()` method. This method allows you to send either **text** or **binary** data (like an `ArrayBuffer` or `Blob`).

#### **Example of sending a text message:**

```javascript
// Ensure the connection is open before sending a message
socket.onopen = function(event) {
  console.log("Connection established, sending a message...");
  socket.send("Hello, Server!");  // Sending a simple text message
};
```

- You can also send binary data using `send()` by passing a `Blob` or `ArrayBuffer` instead of a string.

#### **Example of sending binary data:**

```javascript
const buffer = new ArrayBuffer(8);  // 8 bytes of binary data
const view = new DataView(buffer);
view.setInt32(0, 12345);  // Set an integer value in the buffer

socket.onopen = function() {
  socket.send(buffer);  // Sending binary data
};
```

#### **Important Notes:**

- You should **only send messages** after the WebSocket connection has been established, i.e., inside the `onopen` event handler.
- The `send()` method throws an error if you try to send a message while the connection is not open (e.g., if the connection is still in the **CONNECTING** state).

---

### **4. Receiving Messages with `onmessage`**

To receive messages from the server, you set up the `onmessage` event handler. This function is called whenever a message is received from the server.

#### **Example:**

```javascript
// Receiving messages from the server
socket.onmessage = function(event) {
  console.log("Message from server:", event.data);  // The message is in event.data
};
```

- The **`event.data`** property contains the data sent by the server. This could be a **string** (if it's text data) or a **`Blob`/`ArrayBuffer`** (if it's binary data).

#### **Handling different types of messages:**

You can check the type of data using `typeof event.data` or `instanceof` to determine if the data is text or binary.

```javascript
socket.onmessage = function(event) {
  if (typeof event.data === 'string') {
    console.log("Text message:", event.data);
  } else if (event.data instanceof ArrayBuffer) {
    console.log("Binary data received.");
  }
};
```

---

### **5. Handling `open`, `close`, and `error` Events**

You can handle several key WebSocket lifecycle events to ensure smooth communication and error handling:

#### **`onopen`** - The connection is established:

- The `onopen` event is fired when the WebSocket connection is successfully opened.
- This is where you can start sending messages.

```javascript
socket.onopen = function(event) {
  console.log("WebSocket connection opened:", event);
  socket.send("Hello server, I'm connected!");
};
```

#### **`onclose`** - The connection is closed:

- The `onclose` event is fired when the connection is closed either by the client, server, or due to an error.
- It provides details about the closure, such as whether it was clean or if there was an error code.

```javascript
socket.onclose = function(event) {
  if (event.wasClean) {
    console.log(`Connection closed cleanly: ${event.code} - ${event.reason}`);
  } else {
    console.error("Connection closed unexpectedly!");
  }
};
```

#### **`onerror`** - An error occurs:

- The `onerror` event is fired if something goes wrong with the connection, such as a network issue or invalid data.

```javascript
socket.onerror = function(event) {
  console.error("WebSocket error:", event);
};
```

---

### **6. Handling Connection Issues and Retries**

WebSocket connections can occasionally drop due to network issues, server failures, or other reasons. To ensure reliable communication, you should handle these failures and potentially attempt to **reconnect**.

#### **Basic Retry Logic:**

You can detect connection issues and attempt to reconnect automatically if the WebSocket connection is closed or encounters an error. Below is a simple retry mechanism:

```javascript
let socket;
let reconnectAttempts = 0;

function createWebSocket() {
  socket = new WebSocket("wss://example.com/socket");

  socket.onopen = function(event) {
    console.log("Connection opened");
    reconnectAttempts = 0;  // Reset retry count on success
  };

  socket.onclose = function(event) {
    console.log("Connection closed:", event.code);
    // Retry connection with exponential backoff
    if (reconnectAttempts < 5) {
      reconnectAttempts++;
      const delay = Math.pow(2, reconnectAttempts) * 1000;  // Exponential backoff
      console.log(`Reconnecting in ${delay}ms...`);
      setTimeout(createWebSocket, delay);  // Retry after delay
    } else {
      console.log("Max retries reached. Please check your connection.");
    }
  };

  socket.onerror = function(error) {
    console.error("WebSocket error:", error);
  };
}

// Initial connection attempt
createWebSocket();
```

#### **Explanation:**

- **Exponential Backoff:** This logic increases the wait time between each reconnect attempt, which can help avoid overwhelming the server with reconnect requests.
- **Retry Limit:** A simple retry limit (`reconnectAttempts`) ensures you don't retry indefinitely if there are persistent issues.

#### **Handling Idle Connections:**

- If your WebSocket connection has been idle for a long time, some servers or network infrastructure (e.g., firewalls) may close it automatically. To prevent this, you can send periodic **ping** frames to keep the connection alive.

---

## WebSocket Security

WebSocket connections are powerful because they enable real-time communication between clients and servers. However, because WebSockets can be persistent and open for long periods, ensuring their security is critical to prevent data breaches, attacks, and other vulnerabilities. Let's go through some of the key **security considerations** when using WebSockets.

---

### 1. Using `wss://` (WebSocket Secure) vs `ws://` (Insecure)

Just like **HTTP** has **HTTPS**, WebSockets have **`ws://`** and **`wss://`**:

- **`ws://`** (WebSocket - Insecure):
  - This is the non-secure version of WebSockets. It does not provide encryption, which means the data is sent in plaintext over the network.
  - Using `ws://` is **not recommended for production** environments, especially for applications where sensitive information is exchanged (e.g., login data, payment information).
- **`wss://`** (WebSocket Secure):

  - This is the secure version of WebSockets, just like HTTPS. It encrypts the data using **SSL/TLS**, providing a secure communication channel between the client and server.
  - **Always use `wss://`** in production environments, especially when transmitting sensitive data, to prevent eavesdropping, tampering, and man-in-the-middle (MITM) attacks.

#### **Why Use `wss://`?**

- Encryption: Protects data from being intercepted by unauthorized parties (e.g., via MITM attacks).
- Authentication: Ensures the server is authenticated (just like HTTPS).
- Data Integrity: Ensures that data sent over the connection cannot be tampered with by third parties.

---

### 2. Handling Cross-Origin Requests (CORS for WebSocket)

Cross-Origin Resource Sharing (CORS) is a mechanism that allows or restricts web applications running at one origin to request resources from a different origin. WebSockets don't use the same CORS model as HTTP requests, but **they can still be affected** by security measures regarding cross-origin requests.

- **WebSocket CORS policy**: WebSocket servers **don’t have a direct CORS policy** like HTTP servers. However, the server can decide whether to accept a WebSocket connection from a different origin.

  - **Origins** are set by the browser, which sends an `Origin` header when initiating a WebSocket connection.
  - The WebSocket server can check the `Origin` header and decide whether to allow the connection.

#### **Example:**

A WebSocket server can inspect the `Origin` header to accept connections only from trusted domains:

```javascript
// WebSocket server (e.g., Node.js with ws library)
const WebSocket = require('ws');
const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', (ws, req) => {
  const origin = req.headers.origin;

  if (origin !== 'https://trusted-website.com') {
    ws.close(1008, 'Origin not allowed');
  } else {
    ws.send('Welcome to the secure WebSocket server!');
  }
});
```

**Important Notes:**

- The **Origin** header is sent automatically by browsers when establishing a WebSocket connection.
- Ensure that only trusted origins are allowed to connect to your WebSocket server to prevent malicious sites from interacting with your server.

---

### **3. CSRF Protection in WebSockets**

While WebSockets are not directly vulnerable to **Cross-Site Request Forgery (CSRF)** attacks in the same way as HTTP requests, **authentication mechanisms** used with WebSockets can still be vulnerable.

#### **Why WebSockets aren’t directly affected by CSRF:**

- WebSocket connections are not triggered automatically by a third-party site (like how a `POST` request could be triggered in CSRF). The connection has to be explicitly initiated by the client using JavaScript.

However, WebSockets can still be **vulnerable to unauthorized access** if the server doesn’t properly authenticate the client before allowing access to sensitive data or actions.

#### **How to prevent unauthorized access:**

- **Session Authentication**: Before opening the WebSocket connection, ensure the client is authenticated. This can be done via cookies, sessions, or token-based authentication (like JWT).
- **Token Validation**: Ensure that the WebSocket connection is secured with an **access token** (like a **JWT**). This ensures that only authenticated users can connect to the WebSocket.

---

### **4. JWT (JSON Web Tokens) for WebSocket Authentication**

**JWT** is commonly used for **stateless authentication** and is highly effective for securing WebSocket connections. You can send the JWT token during the WebSocket handshake to authenticate the client.

#### **How to use JWT with WebSocket:**

1. **Client-Side:**
   - The client should include the JWT token in the WebSocket connection URL or as a **header** during the WebSocket handshake.
   - If using a URL parameter:

```javascript
        const token = 'your-jwt-token-here';
        const socket = new WebSocket(`wss://example.com/socket?token=${token}`);
```

2. **Server-Side:**
   - On the server side, validate the JWT token when the WebSocket connection is being established. If the token is invalid, you can reject the connection.

#### **Example (Node.js server using JWT):**

```javascript
const WebSocket = require('ws');
const jwt = require('jsonwebtoken');
const wss = new WebSocket.Server({ port: 8080 });

wss.on('connection', (ws, req) => {
  // Extract token from query parameters or headers
  const url = new URL(req.url, `http://${req.headers.host}`);
  const token = url.searchParams.get('token');  // Assume token is passed in URL

  // Verify JWT token
  jwt.verify(token, 'your-secret-key', (err, decoded) => {
    if (err) {
      ws.close(1008, 'Unauthorized');  // Close connection if invalid
      return;
    }

    // Token is valid
    ws.send('Connection authenticated!');
  });
});
```

#### **Benefits of JWT in WebSocket Authentication:**

- **Stateless Authentication**: JWTs do not require server-side session storage, making them scalable.
- **Security**: JWTs are signed and can be verified using a secret or public/private key pair, ensuring that the message hasn’t been tampered with.

---

### **5. Securing WebSocket Connections with SSL/TLS**

WebSockets over **`wss://`** use **SSL/TLS encryption**, just like **HTTPS** for web pages. This ensures that all data exchanged between the client and server is encrypted, preventing **eavesdropping**, **tampering**, and **man-in-the-middle (MITM)** attacks.

#### **Why SSL/TLS is crucial:**

- **Confidentiality**: All data is encrypted, so even if someone intercepts the connection, they won’t be able to read the messages.

- **Data Integrity**: SSL/TLS ensures that the data cannot be tampered with during transmission.

- **Authentication**: The server is authenticated using an SSL/TLS certificate, which ensures the client is connecting to the intended server.

#### **Configuring SSL/TLS for WebSocket (Node.js example):**

```javascript
const fs = require('fs');
const https = require('https');
const WebSocket = require('ws');

const server = https.createServer({
  cert: fs.readFileSync('server-cert.pem'),
  key: fs.readFileSync('server-key.pem')
});

const wss = new WebSocket.Server({ server });

wss.on('connection', function connection(ws) {
  ws.send('Hello over secure WebSocket!');
});

server.listen(8080, () => {
  console.log('Secure WebSocket server running on wss://localhost:8080');
});
```

---

### **6. Preventing WebSocket Hijacking and MITM Attacks**

Since WebSockets can be open for long periods, it’s crucial to prevent **WebSocket hijacking** and **man-in-the-middle (MITM)** attacks. These attacks involve attackers intercepting or taking control of the WebSocket connection.

#### Ways to mitigate hijacking and MITM attacks:

1. **Always use `wss://`**: Never use `ws://` for production, as unencrypted WebSockets can be easily intercepted.

2. **Authenticate before connection**: Ensure that the client is authenticated (e.g., using JWTs, OAuth tokens) before establishing the WebSocket connection.

3. **Use Secure Cookies**: If using cookies for session management, ensure they are **Secure** and **HttpOnly** to prevent JavaScript access.

4. **Verify Origin Header**: Ensure the `Origin` header sent by the client matches the allowed origin to prevent **Cross-Site WebSocket Hijacking (CSWSH)**.

5. **Enable HSTS (HTTP Strict Transport Security)**: This forces clients to always connect using HTTPS (and therefore `wss://` for WebSockets) to prevent accidental unencrypted connections.

---

### **7. WebSocket Handshake and Headers for Authentication**

The **WebSocket handshake** is the initial HTTP request that upgrades the connection to WebSocket. During the handshake, you can include authentication headers or tokens (like JWTs) to authenticate the client before the WebSocket connection is established.

#### **Example of Authentication Header in Handshake:**

```javascript
const socket = new WebSocket('wss://example.com/socket');

// Add JWT token to the headers if needed (for libraries like `ws` in Node.js)
socket.onopen = function() {
  console.log('WebSocket connection established');
};
```

On the server side, you can check
the headers (or URL parameters) to validate the authentication token before proceeding with the WebSocket connection.
