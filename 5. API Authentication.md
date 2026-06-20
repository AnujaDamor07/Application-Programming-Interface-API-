# 🔐 API Authentication

API authentication is how a system confirms that a user or application making a request is actually who they claim to be.

Authentication answers one question:

> **Who are you? 👤**

Authorization is a separate question that comes right after:

> **What are you allowed to do? 🛡**

---

## 🔑 Common Ways to Authenticate API Requests

1. Basic Authentication
2. API Keys
3. OAuth 2.0
4. JWT (JSON Web Tokens)
5. Certificate-Based Auth (Mutual TLS / mTLS)
6. HMAC (Hash-Based Message Authentication Code)
7. Session-Based Authentication

---

## 🔓 1. Basic Authentication

### ⚙️ How It Works

The client packs `username:password` into a single string, Base64-encodes it, and drops it into the `Authorization` header.

```http
Authorization: Basic dXNlcjpwYXNzd29yZA==
```
*(this decodes to `user:password`)*

### ✅ Strengths
- Dead simple to set up
- Fine for internal tooling or low-risk APIs
- Handy for quick testing

### ❌ Weaknesses
- Credentials travel on **every** request
- Useless for security without HTTPS
- Easy to intercept if traffic isn't encrypted

### 🛡 Things to Keep in Mind
- Only ever run this over **HTTPS**
- Don't use it for public production APIs
- Pair it with rate limiting, or it's wide open to brute-forcing

---

## 🗝 2. API Keys

### ⚙️ How It Works

Each client gets a unique key, which gets sent via:
- 📩 A header
- 🔎 A query parameter
- 📦 Occasionally the request body

```http
GET /api/data
x-api-key: abc123xyz
```

### ✅ Strengths
- Easy to wire up
- Works well for service-to-service calls
- Decent for read-only endpoints

### ❌ Weaknesses
- It's a static credential
- Carries no real notion of "who" the user is
- Hard to do fine-grained permissions with it
- Frequently leaks into client-side JS by accident

### ⚠️ Watch Out For
- Keys accidentally committed into source code
- Keys visible in browser DevTools
- Keys ending up in logs
- No built-in expiration

---

## 🌐 3. OAuth 2.0

OAuth 2.0 is technically an **authorization framework**, not a pure authentication protocol — it's built to let third-party apps access resources without ever touching the user's actual password.

### ⚙️ How It Works

1. The user authenticates with an authorization server.
2. That server hands back an access token.
3. The client attaches the token to subsequent API calls.

```http
Authorization: Bearer access_token_here
```

### 🔄 Common Grant Types
- Authorization Code — the safest option for web apps
- Client Credentials — machine-to-machine
- Device Code
- Refresh Token
- *(Implicit and Password grants are considered outdated now)*

### ✅ Strengths
- Fine-grained access via scopes
- Tokens expire and can be refreshed
- Industry-standard, broad support

### ❌ Weaknesses
- Non-trivial to implement correctly
- Easy to misconfigure
- Tokens can leak if handled carelessly

### 🚨 Risks to Test For
- Missing `state` parameter validation
- Tokens being reused where they shouldn't be
- Open redirect bugs
- Scopes not actually being enforced server-side

---

## 🎟 4. JWT (JSON Web Tokens)

JWTs are compact, URL-safe tokens that carry their own data — no server lookup required to read them.

### 🧩 What's Inside a JWT

```
Header.Payload.Signature
```

### 1️⃣ Header
Declares the token type and the signing algorithm.
```json
{
  "alg": "HS256",
  "typ": "JWT"
}
```

### 2️⃣ Payload
Holds the actual claims — user ID, roles, expiry, issuer, etc.
```json
{
  "sub": "101",
  "role": "admin",
  "exp": 1700000000
}
```

### 3️⃣ Signature
Produced by signing the header + payload with a secret or private key.

### ✅ Strengths
- Stateless — no session storage needed server-side
- Quick to validate
- A natural fit for microservices

### ❌ Weaknesses
- Hard to revoke unless you're separately tracking them
- Token size balloons with too many claims
- Signed, not encrypted — anyone can read the payload

### 🚨 Risks to Test For
- Algorithm-confusion attacks
- Servers that accept `alg: none`
- Weak signing secrets
- Sensitive data sitting unencrypted in the payload
- Expiration not actually being checked

---

## 🔐 5. Certificate-Based Auth (Mutual TLS / mTLS)

### ⚙️ How It Works

Both sides — client and server — present certificates, and the server verifies the client's cert before letting the request through.

### ✅ Strengths
- Extremely strong identity guarantees
- Backed by real cryptographic proof
- Great fit for high-sensitivity environments

### ❌ Weaknesses
- Certificate lifecycle is a pain to manage
- Revocation needs its own process
- More operational overhead overall

### 🌍 Where You'll See It
- 🏦 Banking
- 🏢 Enterprise system integrations
- 🧩 Internal microservice traffic

---

## 🔏 6. HMAC Authentication

HMAC signs each request using a secret shared between client and server.

### ⚙️ How It Works

1. 🔐 The client builds a hash from the secret key plus parts of the request (method, path, timestamp, body).
2. ✅ The server recomputes that same hash and checks it matches.

```http
Authorization: HMAC signature_here
```

### ✅ Strengths
- Guarantees the request wasn't tampered with
- Resistant to replay attacks when timestamps are included

### ❌ Weaknesses
- Managing the shared secret is its own challenge
- Adds some computational overhead

### 🌍 Where You'll See It
- ☁️ Cloud provider APIs
- 💳 Financial APIs
- 🔏 AWS-style signed requests

---

## 🍪 7. Session-Based Authentication

### ⚙️ How It Works

1. User logs in.
2. Server generates a session ID.
3. That session is stored server-side.
4. The session ID is handed to the client in a cookie.
5. The cookie rides along with every future request.

### ✅ Strengths
- Sessions are easy to kill server-side
- Server stays fully in control of session lifetime
- A long-standing pattern in traditional web apps

### ❌ Weaknesses
- Doesn't scale cleanly without a shared session store
- Awkward fit for pure REST APIs
- Requires the server to hold state

---

## 🍪 A Closer Look: Cookie-Based Auth

Cookie-based auth boils down to storing a session identifier in the browser's cookie jar.

### 🔄 Flow
1. User logs in.
2. Server creates a session ID.
3. That ID comes back via the `Set-Cookie` header.
4. The browser hangs onto the cookie.
5. The cookie auto-attaches to every later request.

```http
Set-Cookie: sessionId=abc123; HttpOnly; Secure
```

### 🛡 Good Habits
- HTTPS only, no exceptions
- Always set the `Secure` flag
- Always set `HttpOnly`
- Use the `SameSite` attribute
- Generate strong, unpredictable session IDs
- Give sessions an expiration
- Invalidate the session the moment someone logs out
- Rotate the session ID right after login

### 🚨 Risks to Test For
- 🕵️ Session hijacking
- 💉 XSS
- 🔁 CSRF
- 🎭 Session fixation
- 🔢 Guessable session IDs

---

## 🎟 A Closer Look: Token-Based Auth

Token-based auth swaps the server-side session for a portable token.

### 🔄 Flow
1. 👤 User logs in.
2. 🎟 Server issues a token.
3. 💾 Client holds onto it (cookie or local storage).
4. 📡 Client sends it along in a header on every call.

```http
Authorization: Bearer eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...
```

### ✅ Strengths
- Stateless
- Scales easily
- Great match for APIs, SPAs, and microservices

### 🛡 Good Habits
- HTTPS everywhere
- Always set an expiration
- Validate the signature on every request
- Check the issuer and audience claims
- Build in token rotation
- Invalidate tokens on logout
- Be careful where you store it — avoid plain `localStorage` if you can

### ⚠️ Risks to Test For
- Token theft through XSS
- Tokens that live too long
- Expiration that isn't actually validated
- Servers accepting unsigned tokens
- Sensitive data sitting in the token payload

---

## 🍪 Cookie-Based vs 🎟 Token-Based — Side by Side

| Feature          | 🍪 Cookie-Based  | 🎟 Token-Based                     |
| ---------------- | ---------------- | ---------------------------------- |
| **Server state** | Stateful         | Stateless                          |
| **Where it lives** | Browser cookie   | Header or client storage           |
| **Scalability**  | Limited          | High                               |
| **Revocation**   | Easy             | Harder                             |
| **CSRF risk**    | Higher           | Lower (when sent via header)       |
| **XSS risk**     | Lower (with HttpOnly) | Higher (if stuck in localStorage) |

---

## 🧭 Picking the Right Method

The right call depends on:
- 🏗 What kind of app you're building
- 🔐 How strict your security needs are
- 📈 How much you need to scale
- 🧩 Monolith vs microservices architecture
- 🔗 Whether third parties need to integrate

### 📌 Rough Rule of Thumb
- 🏢 **Internal APIs** → mTLS or HMAC
- 🌍 **Public APIs** → OAuth 2.0 + JWT
- 🖥 **Traditional web apps** → Session + cookies
- 🧬 **Microservices** → JWT or mTLS
- 🛡 **High-security environments** → mTLS + OAuth
