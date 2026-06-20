# 🌐 The HTTP Protocol

- HTTP = **Hypertext Transfer Protocol**.
- It's the core protocol that lets clients and servers talk to each other across the web.

---

## 🔑 Key Traits

- 🏗 Lives at the **application layer**
- 💬 Works through a **message-based** request/response model
- ♻️ Is **stateless**
- 🔌 Usually rides on top of **TCP**

### 🔢 Default Ports
- 🌍 HTTP → **80**
- 🔐 HTTPS → **443**

---

## ⚙️ Core Behaviors of HTTP

### 1️⃣ Message-Based Exchange

HTTP follows a simple request–response loop:

- 📤 Client sends a request
- ⚙️ Server processes it
- 📥 Server sends back a response

Each exchange is treated as a standalone interaction.

### 2️⃣ Statelessness

HTTP doesn't remember anything between requests:
- 🧠 The server keeps no memory of earlier requests
- 📦 Every request has to carry whatever context it needs, on its own

**State is usually carried via:**
- 🍪 Cookies
- 🆔 Session IDs
- 🔑 JWTs
- 💾 Client-side local storage

### 3️⃣ Built on TCP

Running over TCP gives HTTP:
- ✅ Reliable delivery
- 🔄 In-order transmission
- 🛡 Error detection

🔐 HTTPS layers TLS/SSL encryption on top of that same TCP connection for secure traffic.

---

## 🌐 What an HTTP Exchange Looks Like

Every exchange is made up of:
- 📤 A request
- 📥 A response
- 🏷 Headers
- 📦 An optional body

---

## 📤 Anatomy of a Request

A request has three parts:
1. **Request line**
2. **Headers**
3. **Optional body**

```http
GET /api/users/101 HTTP/1.1
Host: example.com
Authorization: Bearer token123
User-Agent: Mozilla/5.0
```

---

## 📥 Anatomy of a Response

A response also has three parts:
1. **Status line**
2. **Headers**
3. **Optional body**

```http
HTTP/1.1 200 OK
Content-Type: application/json
Content-Length: 45

{
  "id": 101,
  "name": "Rahul"
}
```

---

## 🔄 HTTP Methods

Each method represents a distinct kind of operation:

| Method      | What it does                          |
| ----------- | --------------------------------------|
| **GET**     | Fetch a resource                      |
| **POST**    | Create a new resource                 |
| **PUT**     | Replace a resource entirely           |
| **DELETE**  | Remove a resource                     |
| **HEAD**    | Get headers only, no body             |
| **OPTIONS** | List the supported communication options |
| **TRACE**   | Loop-back diagnostic test              |
| **PATCH**   | Apply a partial update                |
| **CONNECT** | Open a tunnel                         |

---

### 🔹 GET
- 📥 Pulls data
- 🚫 Shouldn't change anything on the server
- 🔗 Parameters typically ride in the URL

```http
GET /products?id=10
```

### 🔹 POST
- 📤 Submits data to the server
- ➕ Usually creates something new
- 📦 Payload goes in the request body

```http
POST /users
Content-Type: application/json
```

### 🔹 PUT
- 🔁 Swaps out the entire resource
- ♻️ Idempotent — repeating it produces the same end state

```http
PUT /users/101
```

### 🔹 DELETE
- 🗑 Deletes the targeted resource

```http
DELETE /users/101
```

### 🔹 PATCH
- 🛠 Updates only part of a resource
- 🎯 Only the fields you specify get changed

```http
PATCH /users/101
```

### 🔹 HEAD
- 📄 Behaves like GET but skips the response body
- 📊 Handy for grabbing metadata only

```http
HEAD /users/101
```

### 🔹 OPTIONS
- 📋 Reports back which methods are supported
- 🌐 Shows up a lot in CORS preflight checks

```http
OPTIONS /users
```

### 🔹 TRACE
- 🔁 Echoes back whatever request it received
- 🛠 Meant for diagnostics
- ⚠️ Usually turned off because of the security risk it carries

### 🔹 CONNECT
- 🔐 Opens up a tunnel to the destination server
- 🌐 Commonly used for HTTPS traffic going through a proxy

---

## 📊 HTTP Status Code Families

Responses fall into five broad categories.

### 1️⃣ 1XX — Informational
- ℹ️ Request was received, processing is ongoing

**Examples:** 100 Continue, 101 Switching Protocols

### 2️⃣ 2XX — Success
- ✅ Everything worked

**Examples:** 200 OK, 201 Created, 204 No Content

### 3️⃣ 3XX — Redirection
- 🔀 Extra steps are needed before the request is fully done

**Examples:** 301 Moved Permanently, 302 Found, 304 Not Modified

### 4️⃣ 4XX — Client Errors
- ❌ Something about the client's request was wrong

**Examples:** 400 Bad Request, 401 Unauthorized, 403 Forbidden, 404 Not Found, 405 Method Not Allowed, 429 Too Many Requests

### 5️⃣ 5XX — Server Errors
- 🔥 The server couldn't fulfil an otherwise valid request

**Examples:** 500 Internal Server Error, 502 Bad Gateway, 503 Service Unavailable, 504 Gateway Timeout

> 📌 5XX errors point to issues on the **server's** side, not the client's.

---

## 🔐 Security Notes Worth Knowing (Pentesting Angle)

### ⚠️ Risky Method Configurations

If methods like PUT are left enabled where they shouldn't be:

```http
PUT /uploads/shell.php
```

⚡ That can open the door to arbitrary file upload vulnerabilities.

### 🔎 TRACE Left On

If `TRACE` is active:
- 🧪 It can be leveraged for Cross-Site Tracing (XST) attacks
- ⚠️ Most hardened environments disable it outright

### ❌ Mismatched Status Codes

Watch for things like:
- Returning `200 OK` for an action that should've been blocked with `403 Forbidden`
- Leaking a full stack trace inside a `500` response

> 💡 Sloppy status codes can mask real authorization bugs or hand over internal system details for free.

### 🛡 Missing Security Headers

Worth checking for:
- `Content-Security-Policy`
- `X-Frame-Options`
- `X-Content-Type-Options`
- `Strict-Transport-Security`

> 💡 Without these, an app can be left open to clickjacking, MIME-sniffing tricks, or MITM-style attacks.

> 🚀 During API security testing, always double-check the methods allowed, the status codes returned, and which security headers are actually present.

---

## 📌 Quick Recap

**HTTP, in short, is:**
- ♻️ Stateless
- 💬 Message-driven
- 🔌 Built on top of TCP
- 🔄 Always request → response

**Why it's worth knowing inside out:**
- 🌍 Web development
- 🏗 API design
- 🔐 Web app penetration testing

✨ Solid HTTP fundamentals are basically the foundation everything else in API security and web pentesting is built on.
