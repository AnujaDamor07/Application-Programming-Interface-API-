# 🧪 API Penetration Testing — Lab Setup Guide

This is a roundup of deliberately vulnerable API apps that are great for hands-on security practice.

Together, these labs let you practice against:

- 🌐 REST
- 🧩 GraphQL
- 🔐 JWT
- 🛡 OAuth
- 🔎 IDOR
- 🚨 SSRF
- 🔓 BOLA
- ...and more

---

## 🏁 OWASP – crAPI

🔗 [https://github.com/OWASP/crAPI](https://github.com/OWASP/crAPI)

**crAPI ("Completely Ridiculous API")** is one of the richest, most realistic API security labs out there right now.

### 🎯 What It Covers
- The full OWASP API Security Top 10
- BOLA (Broken Object Level Authorization)
- Broken authentication
- Mass assignment
- JWT-related bugs
- SSRF
- Rate-limiting flaws

### 🛠 Setup

```bash
git clone https://github.com/OWASP/crAPI.git
cd crAPI
docker-compose up
```

🐳 Runs entirely on Docker, with both a web UI and the API backend included.

---

## 2️⃣ vAPI

🔗 [https://github.com/roottusk/vapi](https://github.com/roottusk/vapi)

A lightweight, beginner-friendly vulnerable REST API.

### 🎯 What It Covers
- SQL injection
- Authentication bypass
- IDOR
- Weak/missing input validation

### 🛠 Setup

```bash
cd /opt
git clone https://github.com/roottusk/vapi.git
cd vapi
docker-compose up -d
```

🐳 Docker-based.

**Confirm it's running:**
```bash
docker ps
netstat -nltup
```

---

## 3️⃣ VAmPI (Vulnerable API)

🔗 [https://github.com/erev0s/VAmPI](https://github.com/erev0s/VAmPI)

A Flask-based REST API with intentional vulnerabilities baked in.

### 🎯 What It Covers
- JWT weaknesses
- BOLA
- Mass assignment
- Weak/missing rate limiting
- Broken access control

### 🛠 Setup

```bash
git clone https://github.com/erev0s/VAmPI.git
cd VAmPI
docker-compose up -d
```

🐳 Docker-based.

**Confirm it's running:**
```bash
docker ps
netstat -nltup
```

### 🔗 Default Service Access

| Service     | Protocol | Default Port | Example URL                    | Description                     |
|-------------|----------|---------------|----------------------------------|----------------------------------|
| VAmPI API   | HTTP     | 5000          | `http://<host-ip>:5000/`        | Main vulnerable API service     |
| Swagger UI  | HTTP     | 5000          | `http://<host-ip>:5000/ui/`     | Interactive API documentation   |

*(Swap `<host-ip>` for wherever you've actually deployed it — your own VM/container IP, not a fixed address.)*

### 📌 Notes
- **VAmPI** stands for *Vulnerable Adversely Misconfigured Product Interface*, and it's intentionally insecure by design.
- Built specifically for API pentesting practice.
- Full API documentation is browsable through Swagger UI.

### 🛠️ Good Things to Test
- 🔐 Broken authentication
- 🧾 Sensitive data exposure
- 🧩 BOLA
- 📡 Mass assignment
- 🔍 Improper input validation

---

## 4️⃣ Damn Vulnerable Bank

🔗 [https://github.com/rewanthtammana/Damn-Vulnerable-Bank](https://github.com/rewanthtammana/Damn-Vulnerable-Bank)

🔗 Backend: [BackendServer folder](https://github.com/rewanthtammana/Damn-Vulnerable-Bank/tree/master/BackendServer)

A complete banking simulation, frontend and backend included.

### 🎯 What It Covers
- Business logic flaws
- Transaction tampering
- IDOR
- Authentication bypass
- Privilege escalation

### 🛠 Setup

**Backend:**
```bash
cd BackendServer
npm install
npm start
```

**Frontend:**
```bash
cd Frontend
npm install
npm start
```

💡 Great for practicing:
- Advanced API logic attacks
- Exploiting banking-style workflows
- Authorization bypass testing
- Business logic abuse scenarios

---

## 5️⃣ Damn Vulnerable GraphQL Application (DVGA)

🔗 [https://github.com/dolevf/Damn-Vulnerable-GraphQL-Application](https://github.com/dolevf/Damn-Vulnerable-GraphQL-Application)

An intentionally vulnerable lab built specifically around GraphQL.

### 🎯 What It Covers
- GraphQL introspection abuse
- Authorization bypass
- Query batching attacks
- IDOR within GraphQL
- Injection attacks

### 🛠 Setup

```bash
docker run -p 5013:5013 dolevf/dvga
```

🐳 Docker-based.

---

## 🧪 Suggested Lab Environment

If you're already set up for pentesting (Kali, Burp, a recon workflow, etc.), this stack covers everything you'll need:

### 💻 Tooling
- Kali Linux as the attacker box
- Docker
- Burp Suite
- Postman / Insomnia
- ffuf / wfuzz
- sqlmap
- jwt_tool
- mitmproxy *(optional)*

### 🌐 Networking
- Stick to **Host-only** or **NAT** networking in VirtualBox/VMware
- Keep these labs fully isolated from any production network

---

## 🎯 A Sensible Learning Order

1️⃣ **VAmPI** → get comfortable with basic REST vulnerabilities
2️⃣ **vAPI** → practice injection-style attacks
3️⃣ **crAPI** → work through the full OWASP API Top 10
4️⃣ **Damn Vulnerable Bank** → dig into business logic attacks
5️⃣ **DVGA** → focus on GraphQL-specific attack techniques
