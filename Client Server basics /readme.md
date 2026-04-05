# 🖥️ Client-Server Basics — TryHackMe Walkthrough

**Room:** [Client-Server Basics](https://tryhackme.com/room/clientserverbasics)
**Difficulty:** Easy
**Category:** Networking / Learning
**Tags:** Client, Server, Protocols, Networking Basics

---

## 📋 Overview

This room introduces the **client-server model** — the fundamental architecture of the internet. You'll learn what clients and servers are, how they communicate, and the protocols they use.

---

## 🏗️ The Client-Server Model

```
Client                     Server
  │                           │
  │  ── HTTP Request ──────►  │
  │                           │  (processes request)
  │  ◄── HTTP Response ────── │
  │                           │
```

- **Client** — Makes requests (your browser, a mobile app, curl)
- **Server** — Listens for and responds to requests (web server, database, API)

---

## 🤝 How They Communicate

Communication follows these steps:

1. **Client** initiates a connection to the server's IP address on a specific port
2. **TCP 3-way handshake** establishes the connection:
   ```
   Client → SYN      → Server
   Client ← SYN-ACK  ← Server
   Client → ACK      → Server
   ```
3. **Application protocol** (HTTP, FTP, SMTP) carries the actual data
4. Connection is closed after the exchange

---

## 🔌 Common Ports and Protocols

| Port | Protocol | Use |
|------|----------|-----|
| 21 | FTP | File transfer |
| 22 | SSH | Secure remote shell |
| 25 | SMTP | Sending email |
| 53 | DNS | Domain name resolution |
| 80 | HTTP | Web browsing |
| 443 | HTTPS | Encrypted web browsing |
| 3306 | MySQL | Database |
| 3389 | RDP | Windows remote desktop |

---

## 🌐 Types of Servers

| Server Type | Function |
|-------------|----------|
| **Web Server** | Serves HTML/CSS/JS pages |
| **Database Server** | Stores and retrieves data |
| **Mail Server** | Handles email (SMTP/IMAP/POP3) |
| **DNS Server** | Resolves domain names |
| **File Server** | Stores and shares files |
| **Authentication Server** | Verifies identities (LDAP, Active Directory) |

---

## 🚩 Room Answers

- **What makes requests in the client-server model?** → Client
- **What responds to requests?** → Server
- **What protocol is used for web browsing?** → HTTP/HTTPS
- **What port does SSH use?** → 22

---

## 📝 Key Takeaways

- The client-server model is the backbone of internet communication.
- **Ports** are like doors — each service listens on a specific port number.
- Understanding this model is essential for both development and security testing.
- In penetration testing, enumerating open ports reveals available services to attack.

---

## 🔗 References

- [TryHackMe — Client-Server Basics](https://tryhackme.com/room/clientserverbasics)
- [Walkthrough Reference](https://medium.com/@RosanaFS/client-server-basics-tryhackme-walkthrough-a9f812eadf2b)
