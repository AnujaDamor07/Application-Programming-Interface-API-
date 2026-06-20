# 🌐 Types of APIs

APIs don't fall into just one bucket — they can be grouped in several different ways depending on what angle you're looking at:

- 🏗 **How they're built** (architecture / communication style)
- 🔓 **Who can use them** (exposure level)
- 🌍 **Where they're used** (usage context)
- 🔄 **How they communicate** (sync vs async)

---

## 🏗 1️⃣ Grouped by Architecture Style

## 🌍 1.1 REST API

REST is an architectural style built around using standard HTTP verbs to act on resources.

### 🔑 Core Ideas
- 📭 No server-side session state between requests
- 📦 Resources are addressed via URLs
- 🔁 Relies on standard HTTP methods
- 🧩 One uniform interface across the API
- 💾 Responses can be cached

**Example:**
```http
GET    /api/users/101
POST   /api/users
PUT    /api/users/101
DELETE /api/users/101
```

### 📦 Data Format
- 🧾 Almost always **JSON**
- 🔄 Occasionally XML, HTML, or other formats

### ⭐ Why People Use It
- 🪶 Lightweight
- ⚡ Quick to build and adopt
- 📈 Scales well
- 🌐 Plays nicely with existing web infrastructure

### 🌍 Typical Use Cases
- 🌐 Web apps
- 📱 Mobile backends
- 🏗 Microservices
- 🔓 Public-facing APIs

---

## 🧱 1.2 SOAP API

SOAP is a much stricter protocol with formal rules around message format and structure.

### 🔑 Core Traits
- 📄 Messages are XML
- 📜 Described via WSDL files
- 🌐 Can ride on HTTP, SMTP, or TCP
- 🔐 Has built-in security extensions (WS-Security)
- 🏦 Supports ACID-style transactions

**Example request:**
```xml
<soap:Envelope>
  <soap:Body>
    <GetUser>
      <UserId>101</UserId>
    </GetUser>
  </soap:Body>
</soap:Envelope>
```

### ⭐ Why People Use It
- 🏗 Heavily standardized
- 🔐 Strong enterprise security guarantees
- 📦 Reliable, guaranteed message delivery

### 🌍 Typical Use Cases
- 🏦 Banking
- 🏢 Enterprise software
- 🏛 Government systems

---

## 🔷 1.3 GraphQL API

GraphQL lets the client decide exactly what data comes back, instead of the server dictating the shape of every response.

### 🔑 Core Traits
- 🔗 Just one endpoint (commonly `/graphql`)
- 🎯 Client shapes the response itself
- 🧩 Backed by a strongly typed schema
- 🔄 Supports queries, mutations, and subscriptions

**Example query:**
```graphql
query {
  user(id: 101) {
    name
    email
  }
}
```

### ⭐ Why People Use It
- 🚫 No over-fetching
- 📉 No under-fetching either
- ⚡ Efficient round trips
- 🎨 Great fit for frontend-heavy apps

> 💡 **From a pentesting angle:** pay close attention to introspection being left on, abusive query depth, batched-query attacks, and whether authorization is actually enforced on nested fields — not just the top-level query.

### 🌍 Typical Use Cases
- 🧩 Complex frontends
- 📱 Mobile apps
- 🔗 Apps juggling lots of interrelated data models

---

## 🚀 1.4 gRPC API

gRPC is Google's high-performance RPC framework.

### 🔑 Core Traits
- 🌐 Runs over HTTP/2
- 📦 Uses Protocol Buffers for binary serialization
- 🧩 Strongly typed contracts
- 🔄 Built-in support for client, server, and bidirectional streaming

### ⭐ Why People Use It
- ⚡ Very fast
- ⏱ Low latency
- 🏗 Great fit for microservice-to-microservice calls

### 🌍 Typical Use Cases
- 🔄 Internal service communication
- 📊 High-throughput systems

---

## 🔓 2️⃣ Grouped by Exposure Level

## 🌍 2.1 Public (Open) APIs

- 🌐 Open to outside developers
- 🔑 Usually gated by API keys or OAuth

**Examples:** payment gateways, social media platforms

## 🏢 2.2 Private (Internal) APIs

- 🏭 Used only inside the organization
- 🚫 Never exposed externally

**Common in:** microservices setups, internal dashboards

## 🤝 2.3 Partner APIs

- 🔗 Shared with specific external partners only
- 📜 Access governed by contracts/agreements

**Examples:** logistics integrations, banking partnerships

> 💡 **Security angle:** public APIs face the broadest external attack surface, private APIs risk being abused by an overly trusted internal actor, and partner APIs can leak sensitive data if access scoping is misconfigured.

---

## 🧩 3️⃣ Grouped by Usage Context

## 🌐 3.1 Web APIs

- 🌍 Communicate over HTTP/HTTPS
- 📱 Power both web and mobile apps
- 🚀 The most common API category today

**Examples:** REST APIs, GraphQL APIs, public/private backend APIs

> 💡 These drive SPAs (React, Vue, Angular), mobile backends, and SaaS products.

## 🖥 3.2 Operating System APIs

These let applications hook into OS-level services.

**They control:**
- 📁 File system access
- 🧠 Memory management
- ⚙️ Process control
- 🖨 Device interaction

**Examples:** Windows API, POSIX system calls

> 💡 Critical for system programming, desktop software, and low-level tooling.

## 📚 3.3 Library / Framework APIs

These give reusable functionality inside a programming environment.

**Examples:**
- 🗄 Database connectors
- 🔐 Auth libraries
- 📝 Logging frameworks
- 🌐 HTTP client libraries

```python
import os
os.listdir()
```

> 💡 They save developers from reinventing common functionality and keep code modular.

---

## 🔄 4️⃣ Grouped by Communication Pattern

## ⏳ 4.1 Synchronous APIs

> Client sends a request and **waits** for the response before moving on.

**Traits:**
- ⌛ Client blocks until it gets a response
- 🌐 Most REST APIs work this way
- 🔁 Request and response happen back-to-back

**Example:**
```http
GET /api/orders/1001
```

**Flow:**
1. Client sends the request
2. Server processes it right away
3. Server returns the response
4. Client resumes execution

> 💡 Good fit for: data lookups, login flows, standard CRUD operations.

## 🚀 4.2 Asynchronous APIs

> Client fires off a request, and the actual processing happens later.

**Traits:**
- 📨 Often paired with message queues
- ⚙️ Good for background jobs
- 🔄 Client doesn't sit around waiting for completion

**Examples:** payment processing, sending emails, generating reports, creating invoices

**Flow:**
1. Client submits the request
2. Server acknowledges it (often `202 Accepted`)
3. Work happens in the background
4. Result comes back later via callback, polling, or webhook

---

## 🚀 Why APIs Matter

APIs are what hold modern software together — they make systems flexible, scalable, and able to talk to each other securely.

### 🧩 1️⃣ Modularity
- 🏗 Big systems break down into independent pieces
- 🔄 You can update one part without breaking the rest
- 📦 Naturally supports microservices

> 💡 Result: cleaner architecture, easier upkeep.

### 🔗 2️⃣ Interoperability
- 🌍 Different tech stacks can talk to each other
- 🖥 Frontend ↔ backend ↔ mobile ↔ third-party services
- 🧱 Doesn't matter if it's Java, Python, Node, etc.

> 💡 Result: smooth cross-platform connections.

### 📈 3️⃣ Scalability
- ⚙️ Backend services scale on their own terms
- 📊 Traffic spreads across services
- ☁️ Easier to scale in the cloud

> 💡 Result: holds up better under heavy load.

### 🔐 4️⃣ Security Control

APIs support modern protections like:
- 🔑 Token-based auth (JWT, OAuth)
- 👥 Role-based access control
- ⏱ Rate limiting
- 📜 Logging and monitoring
- 🔒 mTLS and encryption

> 💡 Result: access stays controlled and auditable.

### ⚡ 5️⃣ Faster Development
- 🔌 Plug in existing services instead of building from zero
- 💳 Payments, 📧 email, 🗺 maps, 🔔 notifications — all pluggable

> 💡 Result: less time and money spent building from scratch.

---

## ⚔️ SOAP vs REST

| Feature | 🧼 SOAP | 🌐 REST |
|----------|----------|----------|
| **Type** | Protocol | Architectural style |
| **Data format** | XML only | JSON, XML, etc. |
| **Transport** | HTTP, SMTP, TCP | HTTP |
| **State** | Can be stateful | Stateless |
| **Caching** | Not built-in | Native via HTTP |
| **Performance** | Heavier | Lightweight |
| **Complexity** | High | Moderate |
| **Standardization** | Strict WS-* rules | No official strict standard |
| **Description language** | WSDL | OpenAPI (optional) |
| **Resource model** | Service-oriented | Resource/URL-oriented |

---

## 🚀 REST vs SOAP vs GraphQL vs gRPC

| Feature | 🌐 REST | 🧼 SOAP | 🧩 GraphQL | ⚡ gRPC |
|-----------|----------|----------|------------|---------|
| **Category** | Architectural style | Protocol | Query language | RPC framework |
| **Format** | Mostly JSON | XML | JSON | Protocol Buffers (binary) |
| **Performance** | High | Moderate | High | Very high |
| **Endpoints** | Multiple | Multiple | Single | Multiple |
| **Flexibility** | Medium | Low | High | Medium |
| **Learning curve** | Easy | Steep | Moderate | Moderate |

---

## 📌 Picking the Right Style

Every architecture shines under different conditions — it depends on scale, security needs, and what the system actually has to do.

**🌐 REST** — most widely adopted, lightweight, scales well, great for web/mobile, fits microservices and public APIs.

**🧼 SOAP** — strict WS-* standards, strong enterprise-grade security, common in banking, government, and legacy enterprise systems.

**🧩 GraphQL** — flexible querying, client controls the response shape, avoids over/under-fetching, strong for frontend-heavy apps.

**⚡ gRPC** — very fast, binary protocol via Protocol Buffers, native streaming support, ideal for service-to-service microservice traffic.
