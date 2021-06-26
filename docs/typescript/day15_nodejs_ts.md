## Day15 - Node.js and Express and TypeScript

#### I. [Execute TS code with Node.js](#p1)

#### II. [Project Setup for Node.js & Express.js](#p2)

#### III. [Add Types to the server side project](#p3)

#### IV. [Add Middleware & Types](#p4)

#### V. [Controllers and Parse response body](#p5)

#### VI. [More CRUD Operations](#p6)

#### VII. [nest.js - built with TS ](#p7)

<div id="p1" />

### I. Execute TS code with Node.js

> Important: Node.js does NOT execute and undertand TypeScript. We need to compile TS into JS, then execute on Node.js.

Another npm tool helps to run TS files in node: "[ts-node](https://github.com/TypeStrong/ts-node)"

<div id="p2" />

### II. Project Setup for Node.js & Express.js

#### 2.1 npm init & tsc init

Create a regular npm project and set it up as a TS project.

```bash
npm init
tsc --init
```

#### 2.2 Compiler Options Setup

```json
"compilerOptions" {
	"module": "commonjs",
	"moduleResolution": "node",
}
```

#### 2.3 install Express.js

install two dependencies:

- "[express](https://expressjs.com/en/starter/installing.html)"
- ["body-parser"](https://www.npmjs.com/package/body-parser)

Command:

```bash
npm install --save express body-parser
```

helper tool on node.js server side: "[nodemon](https://www.npmjs.com/package/nodemon)"

> Note: nodemon is a tool that helps develop node.js based applications by automatically restarting the node application when file changes in the directory are detected.

<div id="p3" />

### III. Add Types to the server side project

Helper package:

- "[@types/node](https://www.npmjs.com/package/@types/node)"
  ```bash
  npm install --save @types/node
  ```
- "[@types/express](https://www.npmjs.com/package/@types/express)"

**Note:** node.js use "commonJS" as default syntax, we can use ES module syntax to get TypeScript support.

```js
const express = require("express"); // WRONG in TS
import express from "express"; // Correct in TS
```

<div id="p4" />

### IV. Add Middleware & Types

**Docs:**

- [Using middleware](https://expressjs.com/en/guide/using-middleware.html)
- [Error-handling middleware ](https://expressjs.com/en/guide/using-middleware.html#middleware.error-handling)

Original Syntax:

```javascript
app.use(function (err, req, res, next) {
  res.status(500).send("Something broke!");
});
```

**Add Types:**
These types are coming from the support of installing **"@types/express"** package.

```js
import express, { Request, Response, NextFunction } from "express";

app.use((err: Error, req: Request, res: Response, next: NextFunction) => {
  res.status(500).json({ message: err.message });
});
```

<div id="p5" />

### V. Controllers and Parse response body

5.1 Request Handler Types:

```js
import { RequestHandler } from  'express';
export  const  createTodo: RequestHandler = (req, res, next) => {...};
```

5.2 Build the data model : "Todo class"

```js
export  class  Todo {
	constructor(public  id: string, public  text: string) {}
}
```

5.3 parse response data using **type casting**

```js
export  const  createTodo: RequestHandler = (req, res, next) => {
	const  text = (req.body  as {text: string}).text;
	const  newTodo = new  Todo(Math.random().toString(), text);
	// ...
};
```

5.4 bind the request handler to the routes in express

```js
import { Router } from "express";
import { createTodo } from "../controllers/todos";

const router = Router();
router.post("/", createTodo); // bind routes with request handler
```

5.5 use "[body-parser](https://github.com/expressjs/body-parser)" middleware
syntax:

```js
import { json } from "body-parser";
app.use(json()); // use as middleware
```

Usage:
It will apply to all incoming requests, doing the following steps:

- parse the request body
- extract any json data in there
- then **populate the body key on the "req" object with that parsed json data**

In browser, use ["**postman**" tool](https://www.postman.com/) to send requests.

<div id="p6" />

### VI. More CRUD Operations

CRUD: data _"create, read, update, delete"_ operations.

```js
router.post("/", createTodo);
router.get("/", getTodos);
router.patch("/:id", updateTodo);
router.delete("/:id", deleteTodo);
```

**Docs:** [express routing-method](http://expressjs.com/en/5x/api.html#routing-methods)

### VII. other framework with strong TS support

- Server side: [nest.js](https://nestjs.com/) - built with TS
- Client side: [angular.js built entirely in TS support](https://www.typescriptlang.org/docs/handbook/angular.html)
