## 1 — Title

**Backend Fundamentals + Express + TypeScript (As Needed)**

* Build a backend from scratch
* Understand how systems actually work

---

## 2 — Agenda

* TypeScript (quick intro)
* What backend does
* HTTP fundamentals
* Express implementation
* Middleware
* Auth (intro)
* Database (intro)

---

## 3 — TypeScript: Why?

**In C:**

```c
int age = 20;
```

**In JavaScript:**

```js
let age;
age = "just a number";
```

Problem:

* No type safety
* Bugs at runtime

---

## 4 — TypeScript Fix

```ts
let age: number = 20;
age = "just a number"; // error
```

**Key Idea**

* TypeScript = JavaScript + safety

---

## 5 — Minimal TypeScript

(used in → src/modules/user/user.service.ts)

```ts
interface User {
  id: number;
  name: string;
  email: string;
}
```

* Defines structure
* Used everywhere in backend

---

## 6 — Backend: What Happens?

```
Client → Request → Backend → Response → Client
```

* User clicks button
* Request goes to server
* Server responds

---

## 7 — Backend Responsibilities

* Routing → src/modules/**/user.route.ts
* Logic → src/modules/**/user.service.ts
* Data → src/db/
* Authentication → src/middleware/auth.ts

---

## 8 — HTTP Basics

**Request contains:**

* Method
* URL
* Headers
* Body

---

## 9 — HTTP Methods

* GET → fetch
* POST → create
* PUT → replace entire resource
* PATCH → partial update
* DELETE → delete

---

## 10 — Status Codes

* 200 → OK
* 201 → Created
* 400 → Bad request
* 401 → Unauthorized
* 404 → Not found
* 500 → Server error

---

## 11 — Params, Query, Body

* Params → `/users/:id`
* Query → `/users?name=nirav`
* Body → JSON data

---

## 12 — Express: Why?

* Node.js HTTP is low-level
* Express simplifies everything

---

## 13 — Basic Server

(actual file → src/app.ts)

```ts
import express from "express";

const app = express();
app.use(express.json());

app.get("/", (req, res) => {
  res.send("API running");
});

app.listen(3000);
```

---

## 14 — First Routes

(actual structure → src/modules/user/user.route.ts)

```ts
app.get("/users", (req, res) => {
  res.json([]);
});
```

```ts
app.post("/users", (req, res) => {
  res.json(req.body);
});
```

---

## 15 — Add TypeScript

(actual usage → service layer)

```ts
interface User {
  id: number;
  name: string;
  email: string;
}

const users: User[] = [];
```

---

## 16 — Create User

(actual mapping → src/modules/user/user.service.ts)

```ts
app.post("/users", (req, res) => {
  const user: User = {
    id: Date.now(),
    name: req.body.name,
    email: req.body.email
  };

  users.push(user);
  res.status(201).json(user);
});
```

---

## 17 — Params Example

```ts
app.get("/users/:id", (req, res) => {
  const user = users.find(u => u.id === Number(req.params.id));
  res.json(user);
});
```

---

## 18 — Payload Validation (Important)

### Why Validate?

* Users can send anything
* Backend must enforce structure
* Prevents bugs and security issues

---

## 19 — Basic Validation Example

(actual place → controller layer)

```ts
app.post("/users", (req, res) => {
  const { name, email } = req.body;

  if (!name || !email) {
    return res.status(400).json({ message: "Invalid payload" });
  }

  const user: User = {
    id: Date.now(),
    name,
    email
  };

  users.push(user);
  res.status(201).json(user);
});
```

---

## 20 — Middleware

* Runs before route
* Controls request flow

---

## 21 — Logger Middleware

(actual file → src/middleware/logger.ts)

```ts
app.use((req, res, next) => {
  console.log(req.method, req.url);
  next();
});
```

---

## 22 — Auth Middleware (Dummy)

(actual file → src/middleware/auth.ts)

```ts
app.use((req, res, next) => {
  if (!req.headers.authorization) {
    return res.status(401).send("Unauthorized");
  }
  next();
});
```

---

## 23 — Authentication vs Authorization

* Authentication → who are you
* Authorization → what can you do

---

## 24 — Auth Flow

```
Login → Token → Send with request → Verify
```

---

## 25 — Database Problem

* Data lost when server restarts

---

## 26 — Database Solution

(actual layer → src/db/)

* Persistent storage
* Stores data permanently

---

## 27 — Types of Databases

* SQL → structured (PostgreSQL)
* NoSQL → flexible (MongoDB)

---

## 28 — Table Example

(actual schema → src/db/schema.ts)

```
users:
id | name | email
```

---

## 29 — Big Picture

```
Client → Request → Middleware → Route → Response
```

(mapping)

* Middleware → src/middleware/
* Route → src/modules/**/user.route.ts
* Service → src/modules/**/user.service.ts
* DB → src/db/

---

## 30 — Final Thought

* Backend systems must be:

  * predictable
  * safe
  * consistent

* Never trust input

* Always validate data

* Always control flow

---

## 31 — Follow-Up Resources

### TypeScript

* [https://www.typescriptlang.org/docs/handbook/intro.html](https://www.typescriptlang.org/docs/handbook/intro.html)

### HTTP

* [https://developer.mozilla.org/en-US/docs/Web/HTTP](https://developer.mozilla.org/en-US/docs/Web/HTTP)

### Express

* [https://expressjs.com/en/starter/installing.html](https://expressjs.com/en/starter/installing.html)

### REST APIs

* [https://restfulapi.net](https://restfulapi.net)

---

## 32 — Next Session

* Replace array with database
* Use PostgreSQL + Drizzle
* Build real backend
