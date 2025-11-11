Express.js is an minimal and flexible node.js web application framework that provides the list of features for building web and mobile application easily.
It helps us to developer the server side application easily by offering easy-to-use API for routing, middleware and HTTP utilities.

- Built on Node.js for fast and scalable server-side development.
- Simplifies routing and middleware handling for web applications.
- Supports building REST APIs, real-time applications, and single-page applications.
- Provides a lightweight structure for flexible and efficient server-side development.

## Express.js code

``` javascript
// Import Express
const express = require('express');
const app = express();

// Define a route
app.get('/', (req, res) => {
    res.send('Welcome to the Express.js Tutorial');
});

// Start the server
app.listen(3000, () => {
    console.log('Server is running on http://localhost:3000');
});
```

#### 1. Import Express

```js
const express = require('express');
```

- This line **imports the Express library**.
- Express is a Node.js framework that simplifies creating web servers and APIs.
- The `require('express')` function loads the Express module so you can use it in your app.

---

#### 2. Create an Express app

```js
const app = express();
```

- Here, you **create an instance of an Express application**.
- The `app` object represents your entire web server.
- You’ll use `app` to define routes, middleware, and start the server.

---

#### 3. Define a route

```js
app.get('/', (req, res) => {
    res.send('Welcome to the Express.js Tutorial');
});
```

- This defines a **GET route** for the root URL (`/`).
- When a user visits `http://localhost:3000/`, this function runs.
- `req` stands for **request** (information sent by the client, like browser or API request).
- `res` stands for **response** (what the server sends back to the client).
- `res.send('...')` sends a plain text message back to the browser.

So, if you open your browser and go to `http://localhost:3000/`, you’ll see:

> Welcome to the Express.js Tutorial

---
#### 4. Start the server

```js
app.listen(3000, () => {
    console.log('Server is running on http://localhost:3000');
});
```

- This tells your app to **start listening for incoming HTTP requests** on port `3000`.
- When the server starts successfully, it prints a message to the console.

---
### How it all works together

1. You run the script with `node filename.js`.
2. Express starts a server on port `3000`.
3. When someone visits `/`, Express handles the request and sends a response.
4. You see a message printed in your terminal confirming the server is running.

## `req`, `res`, and `next`

In **Express.js**, route handlers and middleware functions commonly use three parameters:

```js
(req, res, next)
```

These represent the **request**, **response**, and **next middleware function** in the application’s request-response cycle.

---

### 1. `req` — The Request Object

`req` stands for **request**.  
It contains all the information about the HTTP request made by the client.

### Common properties:

- `req.params` – Route parameters (e.g., `/users/:id`)
- `req.query` – Query string parameters (e.g., `?search=apple`)
- `req.body` – Data sent in the body (for POST/PUT requests)
- `req.headers` – HTTP request headers
- `req.method` – HTTP method (GET, POST, PUT, DELETE, etc.)
- `req.url` – The full request URL

### Example:

```js
app.get('/user/:id', (req, res) => {
  console.log(req.params.id);     // Access route parameter
  console.log(req.query.name);    // Access query parameter
  res.send(`User ID: ${req.params.id}`);
});
```

---

### 2. `res` — The Response Object

`res` stands for **response**.  
It is used to send data or status codes back to the client.

### Common methods:

- `res.send()` – Send a plain text or HTML response
- `res.json()` – Send a JSON response
- `res.status()` – Set the HTTP status code
- `res.redirect()` – Redirect the client to another URL
- `res.render()` – Render a view template

### Example:

```js
app.post('/login', (req, res) => {
  if (req.body.username === 'admin') {
    res.status(200).json({ message: 'Welcome admin' });
  } else {
    res.status(401).send('Unauthorized');
  }
});
```

---

## 3. `next` — The Next Function

`next` is a function that passes control to the **next middleware** in the stack.  
If `next()` is not called, the request will be left hanging.

### Example (Middleware):

```js
app.use((req, res, next) => {
  console.log('Middleware executed');
  next(); // Pass control to the next middleware or route handler
});

app.get('/', (req, res) => {
  res.send('Home Page');
});
```

### Passing Errors to Next:

If an error occurs, you can pass it to Express’s error handler using `next(error)`.

```js
app.use((req, res, next) => {
  const error = new Error('Something went wrong');
  next(error);
});
```