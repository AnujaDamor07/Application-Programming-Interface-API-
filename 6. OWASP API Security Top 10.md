<img width="1200" height="1687" alt="image" src="https://github.com/user-attachments/assets/870033eb-c7c4-410e-9df3-b75b1967010b" />

# 🛡 OWASP API Security Top 10

The **OWASP API Security Top 10** is maintained by **OWASP (Open Web Application Security Project)** and calls out the most significant security risks specific to APIs.

Latest stable edition:
- [**OWASP API Security Top 10 (2023)**](https://owasp.org/API-Security/editions/2023/en/0x11-t10/)

Unlike the general OWASP Top 10, this list zeroes in purely on **API-specific risks**, not generic web app vulnerabilities.

---

## 🚨 OWASP API Security Top 10 (2023)

### 🔓 API1: Broken Object Level Authorization (BOLA)

#### 📌 What It Is
APIs constantly expose objects — users, orders, accounts, you name it. If the API doesn't properly check **who actually owns** the object being requested, an attacker can simply swap out an ID and reach someone else's data.

#### 💻 Example
```http
GET /api/users/124
```
If user `123` can pull up user `124`'s data just by editing the ID in the request, that's a textbook **BOLA** bug.

#### 💥 Impact
- Sensitive data exposure
- Account takeover
- Financial fraud

#### 🛡 How to Prevent It
- Validate object ownership **on the server**, every time
- Never trust client-side checks for this
- Run requests through proper authorization middleware

---

## 🔐 API2: Broken Authentication

#### 📌 What It Is
The authentication layer itself is implemented poorly, giving attackers a way to compromise tokens or credentials.

#### ⚠️ Common Culprits
- Weak JWT signing secrets
- Tokens with no expiration
- Login endpoints with no rate limiting, inviting brute force
- Servers that accept unsigned JWTs (`alg: none`)

#### 💥 Impact
- Account takeover
- Privilege escalation

#### 🛡 How to Prevent It
- Use strong, properly random token secrets
- Always enforce token expiration
- Add MFA where it matters
- Rate-limit login endpoints

---

## 🔓 API3: Broken Object Property Level Authorization

#### 📌 What It Is
Also commonly called a **Mass Assignment** issue. The API lets a client modify fields that should never be under their control in the first place.

#### 💻 Example
```json
{
  "username": "john",
  "role": "admin"
}
```
If a regular user can sneak `role: admin` into a request and have it actually applied, that's a direct path to **privilege escalation**.

#### 💥 Impact
- Privilege escalation
- Corrupted or tampered data

#### 🛡 How to Prevent It
- Whitelist exactly which fields a client is allowed to send
- Use DTOs (Data Transfer Objects) instead of binding raw input straight to your models
- Never auto-bind an entire incoming object

---

## 🚨 API4: Unrestricted Resource Consumption

#### 📌 What It Is
The API places no real limits on how much of its resources a client can consume, leaving the door open for abuse.

#### 🔎 What This Looks Like
- No rate limiting anywhere
- Oversized payload uploads accepted
- Queries that are expensive to run
- Pagination with no upper bound

#### 💥 Impact
- Denial of Service (DoS)
- Ballooning infrastructure costs

#### 🛡 How to Prevent It
- Put real rate limiting in place
- Apply throttling on top of that
- Cap payload sizes
- For GraphQL specifically, enforce query depth/complexity limits

---

## 🔐 API5: Broken Function Level Authorization

#### 📌 What It Is
Sensitive, privileged functions aren't properly gated behind the right authorization checks.

#### 💻 Example
```http
POST /api/admin/deleteUser
```
If a plain, non-admin user can hit this endpoint successfully, that's **Broken Function-Level Authorization** in action.

#### 💥 Impact
- Full system compromise
- Destructive, irreversible data loss

#### 🛡 How to Prevent It
- Implement proper Role-Based Access Control (RBAC)
- Check roles **server-side**, never trust the client to hide a button and call it secure
- Keep admin routes clearly separated from regular user routes
