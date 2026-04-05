# 🧩 Putting It All Together — TryHackMe Walkthrough

**Room:** [Putting It All Together](https://tryhackme.com/room/puttingitalltogether)
**Difficulty:** Easy
**Category:** Web Fundamentals / Learning
**Tags:** HTTP, DNS, Web Servers, CDN, Databases, Web Stack

---

## 📋 Overview

This room consolidates everything from the Web Fundamentals path — DNS, HTTP, web servers, and databases — explaining how they all work together when you visit a website. It's a conceptual room with quiz-style questions.

---

## 🔄 The Full Web Request Journey

When you type `https://tryhackme.com` in your browser:

```
1. Browser checks local DNS cache
       ↓
2. DNS Resolver queries Root → TLD → Authoritative DNS
       ↓
3. Gets IP address for tryhackme.com
       ↓
4. Browser opens TCP connection (3-way handshake)
       ↓
5. TLS handshake (for HTTPS)
       ↓
6. Browser sends HTTP GET request
       ↓
7. Web server (Nginx/Apache) processes request
       ↓
8. Application layer (PHP/Python/Node) runs logic
       ↓
9. Database query (if needed)
       ↓
10. Server sends HTTP response (HTML/JSON)
       ↓
11. Browser renders the page
```

---

## 🧱 Components of the Web Stack

| Component | Role | Examples |
|-----------|------|---------|
| DNS | Translates domain → IP | Cloudflare DNS, Google DNS |
| Load Balancer | Distributes traffic | HAProxy, AWS ALB |
| CDN | Caches static content globally | Cloudflare, Akamai |
| Web Server | Handles HTTP requests | Nginx, Apache |
| Application Server | Runs business logic | PHP, Python, Node.js |
| Database | Stores persistent data | MySQL, PostgreSQL, MongoDB |
| Cache | Speeds up repeated queries | Redis, Memcached |

---

## 🚩 Room Answers

The room asks questions about each component's role in the web stack:

- **What loads when you visit a website?** → HTML, CSS, JavaScript
- **What resolves domain names?** → DNS
- **What serves static content close to users?** → CDN
- **What processes requests and talks to databases?** → Back-end / Application server

---

## 📝 Key Takeaways

- A modern website involves many components working in concert — not just a single server.
- Understanding the full stack helps with both development and security testing.
- Each component can be a potential attack surface: DNS hijacking, SSRF to internal services, SQL injection in the database layer, etc.

---

## 🔗 References

- [TryHackMe — Putting It All Together](https://tryhackme.com/room/puttingitalltogether)
- [Walkthrough Reference](https://medium.com/@Cyb3r_Si3rr4/tryhackme-room-17-putting-it-all-together-15c466eba120)
