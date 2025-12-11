---
date created: 2025-12-11 11:57
date updated: 2025-12-11 12:27
---

## What is ExpressJS.

- Express JS is a fast and minimalist web framework for Node.js.
- It simplifies the process of building server-side applications and APIs by providing a robust set of features for handling HTTP requests, routing, middleware, and more.

## How Express.js works

![[HowExpressJSWorks.png]]

---

## Creating an simple ExpressJs server

### 1. Install Express JS

```bash
npm install express
```

### 2. Create an use the Express in the code

#### JavaScript

```javascript
const express = require("express");

const app = express()

app.get("/", (req,res) => {
	res.send('Hello, World!');
})

app.listen(3000, () => { 
  console.log('Server is running on http://localhost:3000'); 
});
```

#### TypeScript

```typescript

import express, {Request, Response} from "express"

const app = express()

app.get("/", (req: Request, res: Response) => {
	res.send('Hello, World!');
})

app.listen(3000, () => { 
  console.log('Server is running on http://localhost:3000'); 
});

```

- `express()` initializes the app.
- `app.get()` sets up a route to respond to GET requests on the home page (`/`).
- `res.send()` sends a plain text response.
- `app.listen()` starts the server and listens on port 3000.

### 3. Run the application

```bash
node app.js 
```

## Advantages and Disadvantages

### Advantages

| **Advantages of Express.js**            | **Description**                                                                              |
| --------------------------------------- | -------------------------------------------------------------------------------------------- |
| **Fast and Lightweight**                | Minimal by design, avoiding unnecessary features and allowing fast customization.            |
| **Flexible Structure**                  | Does not enforce a specific project structure, giving developers freedom in organization.    |
| **Large Ecosystem & Community Support** | Thousands of middleware options and strong community backing to extend functionality easily. |
| **Full JavaScript Stack**               | Built on Node.js, enabling JavaScript usage on both front end and back end.                  |
| **Perfect for APIs**                    | Clean routing system and powerful middleware support make it ideal for RESTful APIs.         |
### Disadvantages

|**Disadvantages of Express.js**|**Description**|
|---|---|
|**Lack of Built-in Features**|Many common features (authentication, validation, etc.) must be added via middleware or third-party libraries.|
|**No Defined Project Structure**|Flexibility can lead to inconsistent code organization, especially for beginners.|
|**Callback Hell (in older code)**|Poorly managed middleware can cause deeply nested callbacks, though async/await reduces this issue.|
|**Less Opinionated**|Fewer conventions and built-in tools compared to frameworks like Django or Rails, leading to more boilerplate for complex apps.|
