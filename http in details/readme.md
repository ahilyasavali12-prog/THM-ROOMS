# 🌍 HTTP in Detail — TryHackMe Walkthrough

**Room:** [HTTP in Detail](https://tryhackme.com/room/httpindetail)
**Difficulty:** Easy
**Category:** Networking / Learning
**Tags:** HTTP, HTTPS, Methods, Status Codes, Headers, Cookies

---

## 📋 Overview

This room covers **HTTP (HyperText Transfer Protocol)** — the foundation of all web communication. You'll learn about HTTP methods, status codes, headers, cookies, and how HTTPS secures communication.

---

## 📡 What is HTTP?

HTTP is a **request-response** protocol used to transfer web content. When your browser visits a website, it sends an HTTP **request** and receives an HTTP **response**.

**HTTPS** = HTTP + TLS/SSL encryption. The "S" means the data is encrypted in transit.

---

## 📤 HTTP Request Structure

```http
GET /page HTTP/1.1
Host: tryhackme.com
User-Agent: Mozilla/5.0
Accept: text/html
Cookie: session=abc123
```

Components:
- **Method** — What action to perform (GET, POST, etc.)
- **Path** — The resource to request (`/page`)
- **HTTP Version** — Protocol version (1.1 or 2)
- **Headers** — Additional metadata
- **Body** — Data sent with POST/PUT requests

---

## 📥 HTTP Response Structure

```http
HTTP/1.1 200 OK
Content-Type: text/html
Set-Cookie: session=xyz789; HttpOnly
Content-Length: 1234

<html>...</html>
```

---

## 🔠 HTTP Methods

| Method | Purpose |
|--------|---------|
| **GET** | Retrieve a resource |
| **POST** | Submit data (login, forms) |
| **PUT** | Update/replace a resource |
| **DELETE** | Delete a resource |
| **PATCH** | Partially update a resource |
| **HEAD** | Like GET, but no body |
| **OPTIONS** | List supported methods |

---

## 📊 HTTP Status Codes

| Range | Meaning | Examples |
|-------|---------|---------|
| **1xx** | Informational | 100 Continue |
| **2xx** | Success | 200 OK, 201 Created |
| **3xx** | Redirection | 301 Moved, 302 Found |
| **4xx** | Client Error | 400 Bad Request, 401 Unauthorised, 403 Forbidden, 404 Not Found |
| **5xx** | Server Error | 500 Internal Server Error, 503 Service Unavailable |

---

## 🍪 Cookies

Cookies store small pieces of data in the browser and are sent with every request to that domain.

```http
Set-Cookie: session=abc123; HttpOnly; Secure; SameSite=Strict
```

| Flag | Purpose |
|------|---------|
| **HttpOnly** | Prevents JavaScript access (blocks XSS cookie theft) |
| **Secure** | Only sent over HTTPS |
| **SameSite** | Prevents CSRF attacks |

---

## 🔑 Key HTTP Headers

| Header | Purpose |
|--------|---------|
| `Host` | Specifies the target domain |
| `User-Agent` | Identifies the browser/client |
| `Content-Type` | Format of the request body |
| `Authorization` | Authentication credentials |
| `Set-Cookie` | Server sets a cookie |
| `Location` | Redirect destination |

---

## 🚩 Room Answers

- **What HTTP method retrieves data?** → GET
- **What status code means "Not Found"?** → 404
- **What status code means success?** → 200
- **What flag stops JavaScript reading cookies?** → HttpOnly
- **What does HTTPS provide?** → Encryption (TLS)

---

## 📝 Key Takeaways

- HTTP is **stateless** — each request is independent. Cookies and sessions add state.
- Always use **HTTPS** — plain HTTP sends everything in cleartext.
- HTTP methods have semantic meanings — but servers must enforce them properly.
- Understanding HTTP is essential for web security testing.

---

## 🔗 References

- [MDN — HTTP Overview](https://developer.mozilla.org/en-US/docs/Web/HTTP/Overview)
- [TryHackMe — HTTP in Detail](https://tryhackme.com/room/httpindetail)
- [Walkthrough Reference](https://medium.com/@kawsaruddin238/http-in-detail-tryhackme-fb726e12d4a3)
