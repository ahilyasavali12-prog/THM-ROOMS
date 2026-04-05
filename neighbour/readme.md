# 👥 Neighbour — TryHackMe Walkthrough

**Room:** [Neighbour](https://tryhackme.com/room/neighbour)
**Difficulty:** Easy
**Category:** Web / IDOR
**Tags:** IDOR, Broken Access Control, Web Enumeration

---

## 📋 Overview

Neighbour is a short web security room focused on **IDOR (Insecure Direct Object Reference)** — a common web vulnerability where user-supplied input is used to access objects without proper authorisation checks. The objective is to access another user's profile to retrieve the flag.

---

## 🔧 Tools Used

- Browser / Burp Suite — Request inspection
- Developer Tools — Viewing cookies and URL parameters

---

## 🌐 Step 1: Visit the Web Application

Navigate to `http://<TARGET_IP>`. You'll be presented with a login page. Use the **guest** credentials provided on the room page:

```
Username: guest
Password: guest
```

---

## 🔍 Step 2: Inspect the URL After Login

After logging in, look at the URL. You'll notice something like:

```
http://<TARGET_IP>/profile?user=guest
```

The `user` parameter directly references the account being viewed — this is an IDOR vulnerability.

---

## 🚩 Step 3: Exploit the IDOR

Change the `user` parameter in the URL to `admin`:

```
http://<TARGET_IP>/profile?user=admin
```

The server doesn't verify that the logged-in user is authorised to view the admin profile, and returns the admin's page — which contains the flag.

> 🚩 **Flag:** Found on the admin profile page.

---

## 📝 Key Takeaways

- **IDOR** occurs when an application uses user-controlled input to access objects directly without verifying permissions.
- Always test user-controlled parameters (URL params, cookies, form fields) for unauthorised access.
- This vulnerability is ranked in the **OWASP Top 10** under **Broken Access Control**.
- Fixes include server-side authorisation checks on every request.

---

## 🔗 References

- [OWASP — IDOR](https://owasp.org/www-chapter-ghana/assets/slides/IDOR.pdf)
- [TryHackMe — Neighbour Room](https://tryhackme.com/room/neighbour)
- [Walkthrough Reference](https://robitka.medium.com/neighbour-tryhackme-walkthrough-cf21cc7bf608)
