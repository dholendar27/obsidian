---
date created: 2025-12-11 15:07
---

In Express.js, middleware refers to a series of functions that are invoked during the request-response cycle. These functions can modify the request (`req`) and response (`res`) objects, perform operations such as authentication, data validation, logging, and error handling, and either terminate the request-response cycle or pass control to the next middleware function.

#### Types of Middleware in Express.js

1. **Application-level Middleware**

   - This type of middleware is applied globally to the entire Express application or specific routes.
   - It is registered using the `app.use()` method.
   - Application-level middleware can be used for purposes such as logging, body parsing, or applying authentication to all routes.

Example:

```js
   const express = require('express');
   const app = express();

   // Application-level middleware
   app.use((req, res, next) => {
     console.log('Request received');
     next();
   });

   app.get('/', (req, res) => {
     res.send('Hello, world!');
   });

   app.listen(3000, () => {
     console.log('Server running on port 3000');
   });
```

2. **Router-level Middleware**

   - This middleware is bound to a specific router rather than the entire application.
   - It is registered using `router.use()`, and it allows you to apply middleware to a group of routes under a specific path.
   - You can use this to handle authentication, validation, or logging for only certain routes.

   Example:

```js
   const express = require('express');
   const app = express();
   const router = express.Router();

   // Router-level middleware
   router.use((req, res, next) => {
     console.log('Request to /api route');
     next();
   });

   router.get('/info', (req, res) => {
     res.send('API Information');
   });

   app.use('/api', router);  // Use the router for all /api routes

   app.listen(3000, () => {
     console.log('Server running on port 3000');
   });
```

3. **Built-in Middleware**\
   Express provides several built-in middleware functions to handle common tasks such as parsing request bodies, serving static files, and logging requests. Some common built-in middleware functions include:

   - **`express.json()`**: Parses incoming requests with JSON payloads. It is based on `body-parser`.
   - **`express.urlencoded()`**: Parses incoming requests with URL-encoded data, typically from forms.
   - **`express.static()`**: Serves static files (such as HTML, CSS, or JavaScript files) from a directory.

   Example:

```js
   app.use(express.json());  // For parsing application/json
   app.use(express.urlencoded({ extended: true }));  // For parsing application/x-www-form-urlencoded
   app.use(express.static('public'));  // Serve static files from 'public' folder
```

4. **Error-handling Middleware**

   - Error-handling middleware is used to catch and handle errors that occur during the request-response cycle.
   - It is defined with four parameters: `err`, `req`, `res`, and `next`.
   - This middleware must be added after all routes and other middleware, because it is only invoked when an error occurs.

   Example:

```js
   app.use((err, req, res, next) => {
     console.error(err.stack);  // Log the error stack
     res.status(500).send('Something went wrong!');
   });
```

5. **Third-party Middleware**

   - Express supports integration with third-party middleware, which can be added to your application to perform various tasks.
   - Common third-party middleware includes logging, rate limiting, and security-related middleware.
   Example using `morgan` (a popular logging middleware):

```js
   const morgan = require('morgan');
   app.use(morgan('dev'));  // Logs HTTP requests in the 'dev' format
```

6. **Custom Middleware**

   - You can create your own custom middleware functions to meet specific needs in your application.
   - A custom middleware can perform tasks such as logging, modifying request data, or adding headers to the response.

   Example:

```js
   const customMiddleware = (req, res, next) => {
     console.log('Custom middleware executed');
     next();  // Pass control to the next middleware
   };

   app.use(customMiddleware);
```

#### Middleware Workflow

Middleware functions are executed in the order they are defined. Each middleware function has the option to either:

- Call the `next()` function to pass control to the next middleware or route handler.
- End the request-response cycle by sending a response using `res.send()`, `res.json()`, `res.redirect()`, etc.

If a middleware does not call `next()`, the request-response cycle is terminated, and no further middleware or route handlers will be executed.

#### Use Cases for Middleware

1. **Authentication and Authorization**:

   - Middleware is commonly used to verify if the user is authenticated or has the necessary permissions to access specific routes.
2. **Logging**:

   - Middleware can log details about each incoming request, such as the HTTP method, the requested URL, the status code, and the time taken to process the request.
3. **Request Parsing**:

   - Middleware like `express.json()` and `express.urlencoded()` parses the request body and makes the parsed data available in `req.body`.
4. **Rate Limiting**:

   - Middleware can limit the number of requests from a specific IP address or user to prevent abuse or ensure fair usage.
5. **Error Handling**:

   - Error-handling middleware can be used to catch any errors that occur during the request-response cycle and return a consistent error response.
6. **CORS (Cross-Origin Resource Sharing)**:

   - Middleware can be used to add appropriate CORS headers, allowing or restricting cross-origin requests.
7. **Response Modification**:

   - Middleware can add custom headers, manipulate the response data, or compress the response before sending it to the client.
