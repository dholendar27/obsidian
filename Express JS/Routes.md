## Express.js Routes and Key Concepts

## What Are Routes?

In Express.js, a route defines how the server responds to a specific HTTP request made to a particular path.  
A route consists of three parts:

1. HTTP method (GET, POST, PUT, DELETE, etc.)
2. Path or endpoint (e.g., `/users`, `/products/:id`)
3. Handler function (the code that runs when the request is received)

Example:

```js
app.get('/users', (req, res) => {
  res.send('List of users');
});
```

---

## Why Are Routes Used?

Routes allow you to:

- Structure how your API handles different requests
- Separate different functionalities by URL
- Implement CRUD operations (Create, Read, Update, Delete)
- Organize server logic cleanly

---

# Common HTTP Methods

|Method|Purpose|Example|
|---|---|---|
|GET|Retrieve data|GET /users|
|POST|Create new data|POST /users|
|PUT|Update existing data|PUT /users/5|
|PATCH|Partial update|PATCH /users/5|
|DELETE|Remove data|DELETE /users/5|

---

# Important Concepts in Express.js

## 1. Middleware

Middleware is a function that runs before your route handler.  
It can be used for:

- Parsing JSON
- Logging requests
- Authentication
- Data validation
- Error handling

Example:

```js
app.use(express.json());
```

Custom middleware:

```js
app.use((req, res, next) => {
  console.log('A request was made');
  next();
});
```

---

## 2. Request Object (req)

The `req` object contains information about the incoming request.

Key properties:

- `req.params` — URL parameters (e.g., `/users/:id`)
- `req.query` — query string parameters (e.g., `/search?name=john`)
- `req.body` — data sent in POST or PUT requests
    

Example:

```js
app.get('/users/:id', (req, res) => {
  console.log(req.params.id);
});
```

---

## 3. Response Object (res)

The `res` object is used to send data back to the client.

Common methods:

- `res.send()`
- `res.json()`
- `res.status()`
- `res.redirect()`

Example:

```js
res.status(200).json({ message: "Success" });
```

---

## 4. Express Router

The Router allows you to organize routes into separate files.

Example (routes/users.js):

```js
const express = require('express');
const router = express.Router();

router.get('/', (req, res) => res.send('Get all users'));
router.post('/', (req, res) => res.send('Create user'));

module.exports = router;
```

Use it in the main file:

```js
app.use('/users', require('./routes/users'));
```

---

## 5. Controllers

Controllers hold the logic for routes, helping keep the route files clean.

Example (controllers/userController.js):

```js
exports.getUsers = (req, res) => {
  res.send('Get all users');
};
```

Routes using the controller:

```js
router.get('/', userController.getUsers);
```

---

## 6. Route Parameters vs Query Parameters

### Route Parameters

Part of the URL path.

Example:

```
/users/:id
```

Access:

```js
req.params.id
```

### Query Parameters

Sent after a question mark.

Example:

```
/search?keyword=node
```

Access:

```js
req.query.keyword
```
