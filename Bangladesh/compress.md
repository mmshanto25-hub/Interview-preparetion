# MERN Stack Interview Preparation — Bangladesh

> A practical, interview-ready collection of common Full Stack MERN interview questions and answers.

## Table of Contents

1. [JavaScript + React](#1-javascript--react)
2. [Backend — Node.js + Express](#2-backend--nodejs--express)
3. [Database](#3-database)
4. [Full Stack / System Design](#4-full-stack--system-design)
5. [Git + Deployment + Common](#5-git--deployment--common)
6. [Bonus Rapid-Fire Questions](#bonus-rapid-fire-questions)
7. [Interview Answer Formula](#interview-answer-formula)

---

# 1. JavaScript + React

## 1. `var`, `let`, `const` এর difference কী?

### Answer

- `var` → function scoped, redeclare করা যায়।
- `let` → block scoped, redeclare করা যায় না, কিন্তু reassign করা যায়।
- `const` → block scoped, redeclare/reassign কোনোটাই করা যায় না।

```js
var a = 10;
var a = 20; // allowed

let b = 10;
b = 20; // allowed

const c = 10;
// c = 20; // error
```

### Interview line

> Modern JavaScript-এ সাধারণত `const` ব্যবহার করি, আর value পরিবর্তন হলে `let` ব্যবহার করি। `var` avoid করি।

---

## 2. Closure কী?

Closure হলো যখন একটি function তার বাইরের scope-এর variable-কে মনে রাখতে পারে, এমনকি outer function execution শেষ হওয়ার পরেও।

```js
function counter() {
  let count = 0;

  return function () {
    count++;
    return count;
  };
}

const increment = counter();

console.log(increment()); // 1
console.log(increment()); // 2
```

এখানে inner function `count` variable-টাকে মনে রাখছে।

### Interview line

> Closure allows an inner function to access variables from its outer lexical scope even after the outer function has finished execution.

---

## 3. Promise vs async/await

**Promise** asynchronous operation-এর future result represent করে।

```js
fetch("/api/users")
  .then(res => res.json())
  .then(data => console.log(data))
  .catch(err => console.log(err));
```

`async/await` Promise-এর ওপর তৈরি এবং asynchronous code-কে synchronous-এর মতো readable করে।

```js
async function getUsers() {
  try {
    const res = await fetch("/api/users");
    const data = await res.json();
    console.log(data);
  } catch (error) {
    console.log(error);
  }
}
```

### Interview line

> async/await is basically a cleaner way to work with Promises.

---

## 4. React `useState` কীভাবে কাজ করে?

`useState` component-এর state manage করার জন্য ব্যবহার করা হয়।

```jsx
const [count, setCount] = useState(0);

setCount(count + 1);
```

এখানে:

- `count` → current state
- `setCount` → state update function
- `0` → initial value

State update হলে React component আবার render করে।

---

## 5. `useEffect` কী?

`useEffect` component-এর side effects handle করার জন্য ব্যবহার করা হয়।

যেমন:

- API call
- Event listener
- Timer
- DOM interaction

```jsx
useEffect(() => {
  fetchUsers();
}, []);
```

`[]` থাকলে সাধারণত component প্রথমবার render হওয়ার পরে effect চলে।

---

## 6. Dependency array কী?

```jsx
useEffect(() => {
  console.log("User changed");
}, [user]);
```

এখানে `user` dependency।

- `[]` → initial mount-এর পরে
- `[user]` → `user` পরিবর্তন হলে
- dependency না দিলে → প্রতিটি render-এর পরে

---

## 7. Props drilling কী?

যখন parent থেকে deeply nested child component-এ props পাঠানোর জন্য মাঝের অনেক component-এর মধ্য দিয়ে props pass করতে হয়, সেটাকে **props drilling** বলে।

```text
App
 ↓
Parent
 ↓
Dashboard
 ↓
Profile
 ↓
User
```

সব component-কে props pass করতে হচ্ছে।

### Solutions

- Context API
- Zustand
- Redux / Redux Toolkit
- অন্যান্য state-management solutions

---

## 8. Virtual DOM কী?

Virtual DOM হলো actual DOM-এর একটি lightweight JavaScript representation।

```text
State Change
     ↓
Virtual DOM
     ↓
Comparison / Reconciliation
     ↓
Required DOM updates
```

React state change হলে পুরো DOM অযথা পরিবর্তন না করে প্রয়োজনীয় অংশ update করার চেষ্টা করে।

> Important: React is fast only because of Virtual DOM — এভাবে বলা ঠিক না। React-এর performance-এর পেছনে reconciliation, efficient rendering, batching, scheduling ইত্যাদিও গুরুত্বপূর্ণ।

---

## 9. React list-এ `key` কেন লাগে?

React-কে list-এর কোন item পরিবর্তন হয়েছে সেটা identify করতে সাহায্য করে।

```jsx
users.map(user => (
  <div key={user.id}>
    {user.name}
  </div>
));
```

Unique ID ব্যবহার করা best।

```jsx
key={user.id}
```

সাধারণত index কে key হিসেবে ব্যবহার না করাই ভালো যদি list dynamically change হয়।

---

## 10. Next.js Server Component vs Client Component

### Server Component

- Server-এ render হয়
- Backend/database access-এর কাছাকাছি কাজ করতে পারে
- Browser-এ unnecessary JavaScript কম পাঠাতে পারে
- Interactive state/event handler ব্যবহার করতে পারে না

### Client Component

```jsx
"use client";
```

ব্যবহার করলে component client-side interactive behavior-এর জন্য তৈরি হয়।

এতে ব্যবহার করা যায়:

- `useState`
- `useEffect`
- `onClick`
- Browser APIs

### Interview line

> Next.js App Router-এ components are Server Components by default. We use Client Components when we need client-side interactivity or browser-only APIs.

---

# 2. Backend — Node.js + Express

## 11. REST API কী?

REST API হলো HTTP-based architectural style যেখানে resources-এর সাথে standard HTTP methods ব্যবহার করে কাজ করা হয়।

```text
GET    /api/products
GET    /api/products/123
POST   /api/products
PUT    /api/products/123
DELETE /api/products/123
```

---

## 12. GET, POST, PUT, DELETE কখন ব্যবহার করো?

| Method | Purpose |
|---|---|
| GET | Data নেওয়া |
| POST | নতুন data তৈরি |
| PUT | Existing resource update/replace |
| PATCH | Partial update |
| DELETE | Data delete |

Example:

```text
GET /users
POST /users
PUT /users/10
DELETE /users/10
```

---

## 13. Express Middleware কী?

Middleware হলো এমন function যেটা request এবং response-এর মাঝখানে কাজ করে।

```js
app.use((req, res, next) => {
  console.log(req.method);
  next();
});
```

### Common uses

- Authentication
- Logging
- Validation
- Error handling
- CORS

---

## 14. Auth middleware কীভাবে বানাবে?

```js
const authMiddleware = (req, res, next) => {
  const token = req.headers.authorization?.split(" ")[1];

  if (!token) {
    return res.status(401).json({
      message: "Unauthorized"
    });
  }

  // verify token here

  next();
};
```

Route:

```js
app.get("/profile", authMiddleware, getProfile);
```

---

## 15. JWT Authentication কীভাবে কাজ করে?

Typical flow:

```text
User Login
   ↓
Email + Password
   ↓
Backend
   ↓
Password verify
   ↓
JWT generate
   ↓
Client stores/sends authentication
   ↓
Protected API request
   ↓
Backend verifies JWT
   ↓
Access granted
```

JWT সাধারণত তিনটি অংশ নিয়ে তৈরি:

```text
Header.Payload.Signature
```

---

## 16. JWT Cookie vs LocalStorage

দুটোর trade-off আছে।

### HttpOnly Cookie

Cookie-তে `HttpOnly`, `Secure`, এবং appropriate `SameSite` settings ব্যবহার করা যায়। এতে JavaScript-এর মাধ্যমে token access সীমিত করা যায়।

### LocalStorage

সহজ:

```js
localStorage.setItem("token", token);
```

কিন্তু JavaScript-এর মাধ্যমে access করা যায়। তাই XSS হলে token exposure-এর risk থাকে।

### Interview answer

> For sensitive authentication tokens, I generally prefer secure HttpOnly cookies with proper SameSite and CSRF protections rather than storing tokens in localStorage.

---

## 17. bcrypt কেন ব্যবহার করি?

Password সরাসরি database-এ রাখা উচিত না।

```text
Password
   ↓
bcrypt hash
   ↓
Database
```

Login-এর সময়:

```text
Entered Password
       ↓
bcrypt.compare()
       ↓
Stored Hash
```

bcrypt password hashing-এর জন্য ব্যবহার করা হয়।

---

## 18. Express error handling কীভাবে করো?

Central error middleware ব্যবহার করতে পারি।

```js
app.use((err, req, res, next) => {
  console.error(err);

  res.status(err.statusCode || 500).json({
    message: err.message || "Internal Server Error"
  });
});
```

Route/controller থেকে error `next(error)` দিয়ে পাঠানো যায়।

---

## 19. CORS কী?

CORS = **Cross-Origin Resource Sharing**

ধরো:

```text
Frontend:
https://myapp.com

Backend:
https://api.myapp.com
```

Browser-এর cross-origin security policy-এর কারণে backend-কে allowed origins configure করতে হতে পারে।

Express:

```js
import cors from "cors";

app.use(cors({
  origin: "https://myapp.com",
  credentials: true
}));
```

---

## 20. Rate Limiting কী?

একজন user/IP যেন খুব বেশি request পাঠিয়ে server abuse করতে না পারে।

Example:

```text
100 requests / 15 minutes
```

বিশেষ করে useful:

- Login
- OTP
- Password reset
- Public APIs

---

# 3. Database

## 21. MongoDB vs MySQL/PostgreSQL

### MongoDB

- NoSQL
- Document-based
- JSON-like BSON documents
- Flexible schema
- JavaScript ecosystem-এর সাথে ভালোভাবে কাজ করে

Example:

```json
{
  "name": "Shanto",
  "skills": ["React", "Node", "MongoDB"]
}
```

### PostgreSQL / MySQL

- Relational database
- Tables + rows
- Structured schema
- SQL
- Relationships এবং joins-এর জন্য powerful

### Interview line

> MongoDB is document-oriented and flexible, while PostgreSQL/MySQL are relational databases with structured schemas and SQL.

---

## 22. Mongoose Schema vs Model

### Schema

Document-এর structure define করে।

```js
const userSchema = new mongoose.Schema({
  name: String,
  email: String,
  password: String
});
```

### Model

Schema-এর ওপর ভিত্তি করে database-এর সাথে কাজ করার interface।

```js
const User = mongoose.model("User", userSchema);
```

তারপর:

```js
User.find();
User.create();
User.findById();
```

---

## 23. SQL JOIN কী?

Multiple tables-এর related data একসাথে পাওয়ার জন্য JOIN ব্যবহার করি।

Example:

```text
Users
-----
id
name

Orders
------
id
user_id
amount
```

SQL:

```sql
SELECT users.name, orders.amount
FROM users
JOIN orders
ON users.id = orders.user_id;
```

এতে user এবং তার orders-এর data পাওয়া যায়।

---

## 24. One-to-Many relationship কী?

একজন user-এর অনেক orders থাকতে পারে।

```text
User
 |
 ├── Order 1
 ├── Order 2
 └── Order 3
```

Relational database-এ সাধারণত:

```text
users.id
    ↑
orders.user_id
```

MongoDB-তে reference ব্যবহার করতে পারো:

```js
{
  user: ObjectId("...")
}
```

---

## 25. Database Index কী?

Index database-কে দ্রুত data খুঁজে পেতে সাহায্য করে।

Example:

```js
userSchema.index({ email: 1 });
```

Email দিয়ে অনেক search হলে index useful।

### Trade-off

Index read/search speed বাড়াতে পারে, কিন্তু extra storage লাগে এবং writes/updates-এর overhead বাড়তে পারে।

---

# 4. Full Stack / System Design

## 26. E-commerce website বানাতে API কী কী বানাবে?

আমি resources অনুযায়ী API design করব।

### Auth

```text
POST /api/auth/register
POST /api/auth/login
POST /api/auth/logout
GET  /api/auth/me
```

### Products

```text
GET    /api/products
GET    /api/products/:id
POST   /api/products
PUT    /api/products/:id
DELETE /api/products/:id
```

### Cart

```text
GET    /api/cart
POST   /api/cart
PUT    /api/cart/:id
DELETE /api/cart/:id
```

### Orders

```text
POST /api/orders
GET  /api/orders
GET  /api/orders/:id
```

### Payment

```text
POST /api/payments/create
POST /api/payments/webhook
```

---

## 27. Frontend → Backend → Database full flow

এটা interview-এর **খুব important question**।

ধরো registration form:

```text
User fills form
      ↓
React state
      ↓
Submit
      ↓
POST /api/auth/register
      ↓
Express Route
      ↓
Validation Middleware
      ↓
Controller
      ↓
bcrypt password hash
      ↓
MongoDB
      ↓
Response
      ↓
Frontend
      ↓
UI update
```

Example:

```js
await fetch("/api/auth/register", {
  method: "POST",
  headers: {
    "Content-Type": "application/json"
  },
  body: JSON.stringify(formData)
});
```

---

## 28. Image/File Upload কীভাবে করবে?

Typical MERN approach:

```text
Frontend
   ↓
FormData
   ↓
Express
   ↓
Multer
   ↓
Cloudinary / S3
   ↓
Image URL
   ↓
MongoDB
```

Database-এ সাধারণত পুরো image binary না রেখে URL/metadata রাখা convenient।

Example:

```js
{
  name: "Product",
  image: "https://cloudinary.com/..."
}
```

---

## 29. SSLCommerz / Stripe payment কীভাবে integrate করবে?

Basic flow:

```text
Customer
   ↓
Checkout
   ↓
Backend creates payment/session
   ↓
Payment Gateway
   ↓
Customer pays
   ↓
Gateway callback/webhook
   ↓
Backend verifies payment
   ↓
Order status = paid
```

### Important interview point

> I would not trust only the frontend success response. The backend should verify the payment through the gateway/webhook before marking an order as paid.

---

## 30. Website slow হলে কীভাবে optimize করবে?

আমি প্রথমে bottleneck identify করব, তারপর optimize করব।

### Frontend

- Code splitting
- Lazy loading
- Image optimization
- Reduce unnecessary re-renders
- Caching
- Minification/compression

### Backend

- Efficient queries
- Pagination
- Caching
- Rate limiting
- Avoid unnecessary API calls

### Database

- Proper indexes
- Query optimization
- Avoid unnecessary data fetching
- Pagination

### Network

```text
CDN
Caching
Compression
HTTP optimization
```

### Interview line

> First I measure the bottleneck using profiling and monitoring tools; then I optimize the actual bottleneck instead of guessing.

---

# 5. Git + Deployment + Common

## 31. Git Merge vs Rebase

### Merge

দুই branch-এর history combine করে।

```bash
git checkout main
git merge feature
```

History সাধারণত preserve থাকে।

### Rebase

Feature branch-এর commits নতুন base-এর ওপর replay করে।

```bash
git checkout feature
git rebase main
```

History cleaner/linear হতে পারে।

### Interview line

> Merge preserves branch history, while rebase rewrites commit history to create a cleaner linear history.

---

## 32. Git Conflict কীভাবে solve করবে?

ধরো:

```text
<<<<<<< HEAD
code A
=======
code B
>>>>>>> feature
```

আমি:

1. Conflict file open করব
2. Correct code নির্বাচন/merge করব
3. Conflict markers remove করব
4. Test করব
5. `git add`
6. তারপর merge/rebase continue বা commit করব

Example:

```bash
git add .
git commit
```

Rebase হলে:

```bash
git rebase --continue
```

---

## 33. Vercel frontend + Render backend deploy

Typical architecture:

```text
GitHub
 ├── Frontend → Vercel
 │
 └── Backend → Render
       ↓
    MongoDB Atlas
```

Frontend environment:

```env
VITE_API_URL=https://api.example.com
```

Backend:

```env
PORT=5000
MONGODB_URI=...
JWT_SECRET=...
```

### Important

`.env` secrets GitHub-এ commit করা উচিত না।

`.gitignore`:

```text
.env
.env.local
```

---

## 34. `.env` কী?

Environment variables রাখার জন্য ব্যবহৃত file।

Example:

```env
MONGODB_URI=mongodb+srv://...
JWT_SECRET=super-secret
```

Code:

```js
process.env.MONGODB_URI
```

### Important

Frontend-এর environment variables-এর ক্ষেত্রে framework অনুযায়ী public exposure rules বুঝতে হবে। Browser-এ পাঠানো variable secret হিসেবে ধরা যাবে না।

---

## 35. “একটা live project আছে?”

এখানে interviewer সাধারণত দেখতে চায়:

- তুমি সত্যিই project বানিয়েছ কিনা
- GitHub code quality
- Folder structure
- Responsive design
- API integration
- Authentication
- Database
- Deployment
- তোমার নিজের contribution

### Interview answer

> Yes. I have several deployed projects. I can show you the live demo and GitHub repository, and I can explain the architecture, API flow, database structure, and the challenges I solved.

---

# Bonus Rapid-Fire Questions

## `==` vs `===`

```js
5 == "5"   // true
5 === "5"  // false
```

`===` value এবং type দুটোই check করে।

---

## `null` vs `undefined`

`undefined` → value assign করা হয়নি।

`null` → intentionally empty value।

---

## `map()` vs `forEach()`

`map()` নতুন array return করে।

```js
const result = numbers.map(n => n * 2);
```

`forEach()` সাধারণত শুধু iterate করার জন্য।

---

## Authentication vs Authorization

### Authentication

> তুমি কে?

### Authorization

> তুমি কী করতে পারো?

Example:

```text
Login → Authentication
Admin-only dashboard → Authorization
```

---

## PUT vs PATCH

**PUT** → resource পুরোটা update/replace করার semantics।

**PATCH** → resource-এর নির্দিষ্ট অংশ update।

---

## SQL Injection কী?

Malicious SQL input দিয়ে database query manipulate করার attack।

### Prevention

- Parameterized queries
- ORM/query builders
- Input validation

---

## XSS কী?

Attacker malicious script inject করে অন্য user's browser-এ execute করানোর চেষ্টা করে।

### Prevention

- Output encoding
- Safe rendering
- Content Security Policy
- Proper input handling

---

## SSR vs CSR

### CSR

Browser বেশি rendering করে।

### SSR

Server HTML generate করে client-এ পাঠায়।

Next.js দুই ধরনের rendering strategy-ই বিভিন্নভাবে support করে।

---

# Interview Answer Formula

কোনো question-এর answer না জানলে random কিছু বলবে না।

এই structure follow করো:

**1. Definition → 2. Why → 3. Example → 4. Real project use**

### Example: “JWT কী?”

> JWT হলো token-based authentication mechanism. Login successful হলে backend একটি signed token issue করতে পারে। Client পরবর্তী authenticated request-এ token পাঠায়, আর backend সেটা verify করে user identify করে। আমার project-এ protected routes এবং authentication-এর জন্য এটি ব্যবহার করতে পারি।

এভাবে answer দিলে তুমি শুধু definition না বলে **practical developer হিসেবে** উত্তর দিতে পারবে।

---

# Quick Interview Checklist

Before attending a MERN interview, make sure you can explain:

- [ ] JavaScript fundamentals
- [ ] Scope and closures
- [ ] Promises and async/await
- [ ] Array methods
- [ ] React state and effects
- [ ] Props and state
- [ ] Context API / Zustand
- [ ] Virtual DOM and reconciliation
- [ ] React keys
- [ ] Next.js Server/Client Components
- [ ] REST API
- [ ] Express middleware
- [ ] JWT authentication
- [ ] Cookies vs LocalStorage
- [ ] bcrypt
- [ ] Error handling
- [ ] CORS
- [ ] Rate limiting
- [ ] MongoDB
- [ ] SQL basics
- [ ] Mongoose
- [ ] Relationships
- [ ] Database indexing
- [ ] API architecture
- [ ] File upload
- [ ] Payment integration
- [ ] Performance optimization
- [ ] Git merge/rebase
- [ ] Conflict resolution
- [ ] `.env`
- [ ] Vercel/Render deployment
- [ ] GitHub project explanation

---

# Final Interview Mindset

Don't try to memorize every answer word-for-word.

Instead:

```text
Understand
    ↓
Practice
    ↓
Build Projects
    ↓
Explain Your Code
    ↓
Practice Interview Questions
```

The strongest interview answers are usually connected to your own projects.

For every major technology in your CV, prepare:

1. **What is it?**
2. **Why did you use it?**
3. **How did you implement it?**
4. **What problem did it solve?**
5. **What challenge did you face?**
6. **How would you improve it?**

> **Goal:** Be able to explain not only *what* you used, but also *why* and *how* you used it.
