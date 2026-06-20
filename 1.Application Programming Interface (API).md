
### 📘 What is an API?

An **API** is a contract that lets two separate pieces of software talk to each other through a defined set of rules, requests, and responses.

The internal implementation stays hidden — only the usable functionality is exposed to the outside world.

> 💡 **Analogy:** An API works like a restaurant menu — you see your options and place an order, but you never need to know how the kitchen actually cooks the food.

### 🍽️ Real-World Analogy

| Role | API World |
|------|-----------|
| 👤 You | Client making a request |
| 📋 Menu | The API (interface/contract) |
| 🧑‍🍳 Kitchen | Server (does the actual work) |

---

## 🧩 Core Building Blocks of an API

### 1️⃣ Endpoint

An endpoint is just a URL that points to a specific resource or action.

```http
GET https://api.example.com/users/123
```

**Breakdown:**
- `/users` → the resource collection
- `/123` → a single item inside that collection

---

### 2️⃣ HTTP Methods

| 🛠 Method | 🎯 What it does       | 📌 Sample path |
|-----------|------------------------|----------------|
| GET       | Read/fetch data        | `/products`    |
| POST      | Create something new   | `/users`       |
| PUT       | Replace/update fully   | `/users/123`   |
| PATCH     | Update partially       | `/users/123`   |
| DELETE    | Remove a resource      | `/users/123`   |

---

### 3️⃣ Request Structure

A typical request can carry:

- 🧾 **Headers** — auth tokens, content type, etc.
- 🔎 **Query parameters** — filters, search terms
- 📦 **Body** — the actual payload (usually JSON)
- 🍪 **Cookies** — session-related data

```bash
curl -X POST https://api.example.com/login \
  -H "Content-Type: application/json" \
  -d '{"username":"admin","password":"1234"}'
```

---

### 4️⃣ Response Structure

A response generally has:

- 🔢 A **status code**
- 🧾 **Headers**
- 📦 A **body**, usually JSON

```json
{
  "id": 123,
  "name": "John Doe",
  "email": "john@example.com"
}
```

---

## 🔢 HTTP Status Codes

Status codes fall into five families:

### ℹ️ 1xx — Informational
Request received, still processing.
- **100** Continue
- **101** Switching Protocols
- **102** Processing

### ✅ 2xx — Success
Request worked as expected.
- **200** OK
- **201** Created
- **202** Accepted
- **204** No Content

### 🔁 3xx — Redirection
Extra steps are needed to finish the request.
- **301** Moved Permanently
- **302** Found
- **304** Not Modified
- **307** Temporary Redirect

### ❌ 4xx — Client Errors
Something's wrong with the request itself.
- **400** Bad Request
- **401** Unauthorized
- **403** Forbidden
- **404** Not Found
- **429** Too Many Requests

### 💥 5xx — Server Errors
The server couldn't handle a valid request.
- **500** Internal Server Error
- **502** Bad Gateway
- **503** Service Unavailable
- **504** Gateway Timeout

**Quick reference table:**

| 🔢 Code | 📖 Meaning            |
|---------|------------------------|
| 200     | Success                |
| 201     | Created                |
| 204     | No Content             |
| 400     | Bad Request            |
| 401     | Unauthorized           |
| 403     | Forbidden              |
| 404     | Not Found              |
| 429     | Too Many Requests      |
| 500     | Internal Server Error  |

More detail: [MDN HTTP status reference](https://developer.mozilla.org/en-US/docs/Web/HTTP/Reference/Status)

---

## 🌍 Where APIs Show Up

### 1️⃣ Web Apps
- Pulling in third-party services (maps, payments, auth)
- SPA frameworks talking to REST backends
- AJAX-driven dynamic pages

### 2️⃣ Mobile Apps
- Login flows
- Push notifications
- Fetching/updating profile data

### 3️⃣ Microservices
- Services calling each other internally
- Event-driven pipelines

### 4️⃣ IoT
- Smart home devices syncing state
- Remote sensor/monitoring data

---

### ⭐ Why APIs Matter

| 🎯 Benefit          | 📝 Why it helps                              |
|--------------------|------------------------------------------------|
| 🧱 Modularity       | Splits big systems into smaller, independent parts |
| ♻️ Reusability      | Don't rebuild the same logic repeatedly        |
| 📈 Scalability      | Scale individual services on their own         |
| 🔗 Integration      | Hook into CRMs, ERPs, payment gateways, etc.   |
| 🔐 Security         | Access controlled via tokens/scopes            |
| 🌍 Ecosystem        | Lets outside developers build on top           |

---

### 🔐 Common Authentication Methods

| 🔑 Method   | 📖 How it works                  | 🛡 Security |
|-------------|-----------------------------------|-------------|
| API Key     | Static key sent in header/URL    | Low–Medium  |
| Basic Auth  | Base64-encoded `user:pass`       | Low         |
| OAuth2      | Token + scopes                   | High        |
| JWT         | Signed token carrying claims     | High        |
| mTLS        | Both client & server verify certs| Very High   |

**Example header:**
```
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

---

## ⚔️ API-Driven Apps vs Traditional Web Apps

| 🧩 Aspect       | 🌐 API-based                     | 🖥 Traditional        |
|-----------------|------------------------------------|------------------------|
| 🎨 Rendering    | Client-side (React/Vue/Angular)   | Server-side (PHP/JSP)  |
| 📦 Data format  | JSON                              | HTML                   |
| 🏗 Architecture | Decoupled frontend/backend        | Monolithic             |
| 📈 Scaling      | Easy, granular                    | Harder, all-or-nothing |
| 🔐 State        | Token-based                       | Session-based          |

---

## 🔌 Types of API Protocols

**REST**
- Stateless
- Resource-oriented
- Standard HTTP verbs
- Usually JSON

**SOAP**
- XML-based
- Rigid schema via WSDL
- Found mostly in legacy enterprise systems

**GraphQL**
- Client picks exactly which fields it wants
- One single endpoint for everything
- Avoids over-fetching data
- Authorization logic can get tricky

---

## 🔐 API Security — Key Risks for Pentesters

### 1️⃣ Broken Object Level Authorization (BOLA / IDOR)
Tampering with an object ID to reach someone else's data.

```http
GET /api/v1/users/43
```
> ⚠️ Swapping the ID could expose another user's resource if access checks are missing.

### 2️⃣ Broken Authentication
- Weak or missing JWT signature checks
- Accepting `alg: none`
- Tokens that never expire

### 3️⃣ Mass Assignment
Sending extra fields in the JSON body to overwrite things like `role` or `isAdmin` that shouldn't be client-controlled.

### 4️⃣ Rate Limiting Gaps
Opens the door to brute-forcing credentials or enumerating valid accounts/IDs.

### 5️⃣ Excessive Data Exposure
API returns more than it should — e.g. `passwordHash`, internal flags, debug info.

### 6️⃣ CORS Misconfiguration
Allowing arbitrary origins via a loose `Access-Control-Allow-Origin` opens the door to cross-origin abuse.

### 7️⃣ Injection via JSON
- SQL Injection
- NoSQL Injection
- Command Injection

> ⚠️ Test every field — including nested JSON objects and arrays — not just top-level params.

---

### 🛠 Useful Tools for API Testing

- 🐝 **Burp Suite** — intercept & manipulate requests
- 📬 **Postman** — manual API exploration
- 🖥 **curl** — quick command-line requests
- 🚀 **ffuf** — endpoint/parameter fuzzing
- 🔎 **mitmproxy** — traffic inspection
- 🔐 **JWT Editor** — tampering with tokens
- 📘 **Swagger UI** — mapping out the API surface

**Pro tips:**
- Use Burp for interception/manipulation
- Use ffuf for fuzzing endpoints and params
- Use JWT Editor for token tampering tests
- Pull Swagger/OpenAPI specs to map the full attack surface
