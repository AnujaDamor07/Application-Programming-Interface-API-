<!-- 🌌 Animated Header -->
<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:ff0080,100:7928ca&height=220&section=header&text=🔐%20API%20Security%20%26%20Pentesting%20Notes&fontSize=36&fontColor=ffffff&animation=fadeIn&fontAlignY=35"/>
</p>

<!-- ✨ Typing Animation -->
<p align="center">
  <img src="https://readme-typing-svg.herokuapp.com?size=22&duration=3000&color=FF4FD8&center=true&vCenter=true&width=800&lines=APIs+%7C+HTTP+%7C+Auth+%7C+OWASP+Top+10;Recon+%E2%86%92+Exploit+%E2%86%92+Document+%E2%86%92+Repeat;REST+%7C+GraphQL+%7C+SOAP+%7C+gRPC;Lab+Notes+from+Beginner+to+Advanced" />
</p>

<p align="center">
  <img src="https://img.shields.io/badge/Status-Actively%20Updated-7928ca?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Focus-API%20Security-ff0080?style=for-the-badge" />
  <img src="https://img.shields.io/badge/Level-Beginner%20to%20Advanced-00c896?style=for-the-badge" />
</p>

---

## 📌 What's in This Repo

This is a personal collection of study notes built while learning **API security and penetration testing** — covering the fundamentals, the underlying protocols, and hands-on exploitation of intentionally vulnerable lab applications.

- 🌐 Core API concepts and architecture
- 🔄 HTTP protocol internals
- 🔑 Authentication & authorization mechanisms
- 🛡 OWASP API Security Top 10, explained with examples
- 🧪 Step-by-step lab setups (crAPI, vAPI, VAmPI, Damn Vulnerable Bank, and more)
- 💉 Real exploitation walkthroughs (BOLA, BFLA, SSRF, SQLi, NoSQLi, Mass Assignment...)

> ⚠️ All examples are based on **intentionally vulnerable lab applications**. Nothing here targets production systems — these notes are for learning, practicing, and referencing later.

---

## 🗂 Notes Index

### 🧩 API Fundamentals

| 📄 Note | 📖 Covers |
|---|---|
| [API Basics](./API-Notes.md) | What an API is, core components, status codes, use cases |
| [Objects & Actions in APIs](./Object-Action-API-Notes.md) | Resource modeling — nouns vs. verbs |
| [Types of APIs](./Types-of-APIs-Notes.md) | REST, SOAP, GraphQL, gRPC, and how they compare |
| [HTTP Protocol Deep Dive](./HTTP-Protocol-Notes.md) | Methods, status codes, headers, security implications |
| [API Authentication](./API-Authentication-Notes.md) | Basic Auth, API Keys, OAuth2, JWT, mTLS, HMAC, sessions |
| [Same-Origin Policy (SOP)](./SOP-Notes.md) | Origins, browser security boundaries, CORS exceptions |

### 🛡 Security Foundations

| 📄 Note | 📖 Covers |
|---|---|
| [OWASP API Security Top 10](./OWASP-API-Security-Top10-Notes.md) | API1–API5 with examples and fixes |
| [Endpoint Discovery & Fuzzing](./Endpoint-Discovery-API-Fuzzing-Notes.md) | Recon methodology, ffuf usage, Juice Shop endpoint map |
| [API Pentest Methodology](./API-Pentest-Methodology-Notes.md) | Full testing workflow + worksheet template |

### 🧪 Vulnerability Deep Dives

| 📄 Note | 📖 Covers |
|---|---|
| [BOLA (Broken Object Level Authorization)](./BOLA-Notes.md) | Classic IDOR-style access control failures |
| [Broken User Authentication](./Broken-User-Authentication-Notes.md) | OTP/password-reset binding flaws |
| [Excessive Data Exposure](./Excessive-Data-Exposure-Notes.md) | Over-permissive API responses |
| [Rate Limiting](./Rate-Limiting-Notes.md) | Abuse prevention, throttling, bypass techniques |
| [BFLA (Broken Function Level Authorization)](./BFLA-Notes.md) | Privilege escalation via unguarded admin functions |
| [Mass Assignment](./Mass-Assignment-Notes.md) | Privilege escalation through unfiltered fields |
| [SSRF](./SSRF-Notes.md) | Server-side request forgery, cloud metadata, internal pivoting |
| [NoSQL Injection](./NoSQL-Injection-Notes.md) | MongoDB operator injection, auth bypass |
| [SQL Injection](./SQL-Injection-Notes.md) | Time-based SQLi, full sqlmap exploitation walkthrough |

### 🧰 Lab Setup Guides

| 📄 Note | 📖 Covers |
|---|---|
| [General Lab Setup Overview](./API-Pentest-Labs-Setup-Notes.md) | crAPI, vAPI, VAmPI, DVB, DVGA at a glance |
| [crAPI Lab Setup](./crAPI-Lab-Setup-Notes.md) | Full install, architecture, service access |
| [vAPI Lab Setup](./vAPI-Lab-Setup-Testing-Notes.md) | Install + enumeration + testing checklist |
| [VAmPI Lab Setup & Testing](./VAmPI-Lab-Setup-Testing-Notes.md) | JWT-focused vulnerable REST API |
| [VAmPI SQL Injection Walkthrough](./VAmPI-SQL-Injection-Notes.md) | Practical sqlmap exploitation on VAmPI |
| [Damn Vulnerable Bank](./Damn-Vulnerable-Bank-Lab-Notes.md) | Business logic & financial API attacks |
| [Vulnerable E-Commerce API](./Vulnerable-Ecommerce-API-Lab-Notes.md) | Node.js-based e-commerce lab, test accounts |
| [Docker Command Reference](./Docker-Commands-Notes.md) | Container management for running labs |

---

## 🧠 Architecture at a Glance

APIs sit at the center of nearly every modern application — connecting frontends, backends, mobile apps, and third-party services through a shared contract of requests and responses.

```
 Client (Web / Mobile / Third-Party)
        │
        ▼
   ┌─────────┐      Auth Layer       ┌──────────────┐
   │   API   │ ───────────────────▶ │   Backend /   │
   │ Gateway │ ◀─────────────────── │  Microservices │
   └─────────┘      JSON / REST      └──────────────┘
        │
        ▼
   Database(s)
```

---

## 🎯 What This Covers

### 🌐 Foundations
- How APIs actually work under the hood
- HTTP request/response mechanics
- Authentication vs. authorization

### 🛡 Offensive Security
- 🔓 BOLA / IDOR
- 🔑 Broken authentication flows
- 🚫 Rate limiting bypass
- 💉 SQL & NoSQL injection
- 🌍 SSRF
- ⚠️ Mass assignment & privilege escalation

---

## 🛠 Tools Referenced Throughout

<p align="center">
  <img src="https://skillicons.dev/icons?i=js,nodejs,react,graphql,docker,kubernetes,python,postgres" />
</p>

- 🐝 Burp Suite
- 🔎 OWASP ZAP
- 📬 Postman
- ⚡ curl
- 🎯 ffuf / wfuzz
- 🧨 sqlmap
- 🔐 jwt_tool

---

## 👥 Who Might Find This Useful

- 🧑‍💻 Developers wanting to understand API security from the attacker's side
- 🔐 Aspiring security researchers / pentesters
- 🕵️ Bug bounty hunters building a methodology
- 📚 Anyone studying for the OWASP API Security Top 10

---

## ⭐ Why These Notes

A mix of:
- 📖 Theory, explained plainly
- ⚙️ Hands-on lab walkthroughs
- 🧠 Real attack scenarios and request/response examples
- 🛡 A consistent focus on *why* each vulnerability matters, not just *how* to trigger it

---

## 📖 Disclaimer & Credit

All testing referenced in these notes was performed against **intentionally vulnerable, locally-hosted lab applications** built for educational purposes. None of this targets production systems or real users.

> 💡 Notes compiled while learning API security fundamentals and practicing on open-source vulnerable lab projects (crAPI, vAPI, VAmPI, Damn Vulnerable Bank, DVGA, and others — credit to their respective maintainers).

---

<!-- 🌌 Footer -->
<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:7928ca,100:ff0080&height=120&section=footer"/>
</p>
