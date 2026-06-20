
# 🧩 Objects vs Actions in APIs

APIs are mostly about two things: the **resources** they expose and the **operations** you can run on those resources. Getting comfortable with this split makes it a lot easier to spot authorization and logic bugs while testing.

- 📦 **Object** → the *thing* the API deals with (a noun)
- ⚡ **Action** → what you're *doing* to that thing (a verb)

---

## 1️⃣ Objects in APIs

An object is simply a resource or entity that lives in the system. Objects are nouns, and they usually show up directly in the URL path.

### 📚 Typical Objects You'll See
- users
- accounts
- orders
- products
- invoices
- payments
- tickets
- comments
- files
- projects

### 🧪 Example — User Object
```
GET /api/users/007
```
- 📦 Object → `users`
- 🆔 ID → `007`

**Response:**
```json
{
  "id": 7,
  "username": "rahul",
  "email": "rahul@example.com",
  "role": "user"
}
```
This payload represents a single User object.

### 🧾 Example — Order Object
```
GET /api/orders/9001
```
**Response:**
```json
{
  "order_id": 9001,
  "user_id": 101,
  "amount": 1500,
  "status": "shipped"
}
```
Object here → `order`.

### 🧩 Example — Nested Object
```
GET /api/users/101/orders
```
- 👨‍👩‍👧 Parent → `users`
- 📦 Child → `orders`

This pulls every order that belongs to user 101.

---

## 2️⃣ Actions in APIs

An action is the operation being carried out on an object. APIs typically express actions in one of two ways:

- 🌐 Through the **HTTP method** (the REST way)
- 🎯 Through a dedicated **action endpoint** (the RPC-style way)

### 🔄 REST-Style Actions (via HTTP Methods)

| 🛠 Method | ⚙️ Meaning      | 📌 Example     |
|-----------|------------------|-----------------|
| GET       | Read             | `/users/101`    |
| POST      | Create           | `/users`        |
| PUT       | Full update      | `/users/101`    |
| PATCH     | Partial update   | `/users/101`    |
| DELETE    | Remove           | `/users/101`    |

**Create a user:**
```
POST /api/users
```
```json
{
  "username": "testuser",
  "email": "test@example.com"
}
```
Object → `users`, Action → create (via POST)

**Update an order:**
```
PUT /api/orders/9001
```
Object → `orders`, Action → update

**Delete a comment:**
```
DELETE /api/comments/555
```
Object → `comments`, Action → delete

### ⚡ Action-Based (Non-REST) Endpoints

Some APIs spell the action out directly in the URL instead of relying on the HTTP verb alone.

**Reset a password:**
```
POST /api/users/007/reset-password
```
Object → `users`, Action → `reset-password`

**Cancel an order:**
```
POST /api/orders/9001/cancel
```
Object → `orders`, Action → `cancel`

**Transfer funds:**
```
POST /api/accounts/2001/transfer
```
Object → `accounts`, Action → `transfer`

**Approve an invoice:**
```
POST /api/invoices/300/approve
```
Object → `invoices`, Action → `approve`

### 💡 REST vs Non-REST in a Nutshell

**REST style** — the verb is baked into the HTTP method:
```
DELETE /users/101
```

**Non-REST style** — the verb is spelled out in the path:
```
POST /users/101/delete
```

---

## 🧠 Real-World Object + Action Patterns

### 🏦 Banking API
```
GET  /accounts/5001
POST /accounts
POST /accounts/5001/withdraw
POST /accounts/5001/deposit
POST /accounts/5001/close
```
- 📦 Object: `accounts`
- ⚙️ Actions: `withdraw`, `deposit`, `close`

`/accounts/5001` points to one specific account, while `/withdraw`, `/deposit`, and `/close` are sensitive business actions that absolutely need solid authorization and validation behind them.

### ☁️ SaaS Platform Example
```
GET    /projects/10
POST   /projects
POST   /projects/10/archive
POST   /projects/10/add-member
DELETE /projects/10
```
- 📦 Object: `projects`
- ⚙️ Actions: `archive`, `add-member`, `delete`

In SaaS-style systems, locking down object-level access really matters — action endpoints like `add-member` or `archive` are common spots where privilege escalation sneaks in.

---

## ⚖️ Object vs Action at a Glance

| 🧩 Aspect   | 📦 Object        | ⚙️ Action               |
|-------------|------------------|--------------------------|
| Type        | Noun             | Verb                     |
| Represents  | A resource       | An operation             |
| Examples    | user, order      | create, delete, cancel   |

---

## 📌 TL;DR

- 📦 **Object** = what the API manages
- ⚙️ **Action** = what gets done to it
- 🧱 Objects are nouns, 🏃 actions are verbs

---

## 🔐 Why This Matters for Security Testing

Every request really needs two checks, not one:

1. 🔑 Is the caller even allowed to touch this **object**?
2. 🛡 Is the caller allowed to perform this specific **action** on it?

Skip either check and you typically end up with:

- Broken Access Control
- Privilege Escalation
- IDOR / BOLA-style vulnerabilities

### ✅ The Golden Rule

When testing any endpoint, always ask:

> Can I access this object? Can I perform this action on it?
