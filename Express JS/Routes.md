## 1. What Are Routes in Express.js?

In **Express.js**, _routes_ define how an application responds to client requests (like HTTP GET, POST, PUT, DELETE) made to specific endpoints (URLs).

A route is made up of:

- An **HTTP method** (GET, POST, etc.)
- A **path** (like `/`, `/users`, `/api/products`)
- A **callback function** (called when the route is matched)

---

## 2. Basic Syntax

```js
app.METHOD(PATH, HANDLER)
```

- **app**: the Express application instance.
- **METHOD**: the HTTP method (e.g., get, post, put, delete).
- **PATH**: the route path.
- **HANDLER**: the function executed when the route is matched.

### Example:

```js
const express = require('express');
const app = express();

app.get('/', (req, res) => {
  res.send('Home Page');
});

app.post('/submit', (req, res) => {
  res.send('Form submitted');
});

app.put('/update', (req, res) => {
  res.send('Data updated');
});

app.delete('/delete', (req, res) => {
  res.send('Data deleted');
});

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

---

## 3. Route Parameters

You can capture values in the URL using **route parameters**.

```js
app.get('/users/:userId', (req, res) => {
  res.send(`User ID: ${req.params.userId}`);
});
```

Here, `:userId` is a **route parameter**.  
For a request like `/users/42`, `req.params.userId` will be `'42'`.

You can have multiple parameters:

```js
app.get('/users/:userId/books/:bookId', (req, res) => {
  res.send(req.params);
});
```

Request: `/users/1/books/99`  
Response: `{ userId: '1', bookId: '99' }`

---

## 4. Query Parameters

Query parameters appear in the URL after `?` and are accessed through `req.query`.

```js
app.get('/search', (req, res) => {
  res.send(req.query);
});
```

Request: `/search?term=node&sort=asc`  
Response: `{ term: 'node', sort: 'asc' }`

---

## 5. Handling Different HTTP Methods on the Same Path

You can define different handlers for the same path but with different methods:

```js
app.route('/user')
  .get((req, res) => res.send('Get user'))
  .post((req, res) => res.send('Add user'))
  .put((req, res) => res.send('Update user'))
  .delete((req, res) => res.send('Delete user'));
```

The `app.route()` method helps group routes for the same path.

---

## 6. Route Handlers and Middleware

A route can have multiple handler functions that act like **middleware**.

```js
app.get('/example',
  (req, res, next) => {
    console.log('Middleware 1');
    next();
  },
  (req, res) => {
    res.send('Response from Middleware 2');
  }
);
```

Handlers are executed in order, and each must call `next()` to move on.

---

## 7. Express Router

As your app grows, you’ll want to organize routes in separate files using `express.Router()`.

### Example: `routes/users.js`

```js
const express = require('express');
const router = express.Router();

router.get('/', (req, res) => {
  res.send('List of users');
});

router.get('/:id', (req, res) => {
  res.send(`User ID: ${req.params.id}`);
});

module.exports = router;
```

### Example: `app.js`

```js
const express = require('express');
const app = express();
const userRoutes = require('./routes/users');

app.use('/users', userRoutes);

app.listen(3000, () => {
  console.log('Server running on port 3000');
});
```

Now:

- `/users` → List of users
- `/users/123` → User ID: 123

---

## 8. Using Middleware with Routes

You can attach middleware to specific routes or route groups.

```js
const logger = (req, res, next) => {
  console.log(`${req.method} ${req.url}`);
  next();
};

app.use(logger); // applies to all routes

app.get('/', (req, res) => res.send('Home'));
```

You can also apply middleware to a single route:

```js
app.get('/admin', logger, (req, res) => {
  res.send('Admin page');
});
```

---

## 9. Wildcard and Regular Expression Routes

Express routes can include wildcards (`*`) or regex for flexible matching.

### Wildcard Example:

```js
app.get('/files/*', (req, res) => {
  res.send('Accessing a file');
});
```

### Regex Example:

```js
app.get(/.*fly$/, (req, res) => {
  res.send('Route ends with "fly"');
});
```

Matches `/butterfly`, `/dragonfly`, etc.

---

## 10. Route Order

Express routes are **matched in the order they are defined**.  
Once a route matches, Express stops checking further routes.

Example:

```js
app.get('/user', (req, res) => res.send('First route'));
app.get('/user', (req, res) => res.send('Second route'));
```

Only the first route will run.
