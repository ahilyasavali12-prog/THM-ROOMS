# 🌐 How Websites Work — TryHackMe Walkthrough

**Room:** [How Websites Work](https://tryhackme.com/room/howwebsiteswork)
**Difficulty:** Easy
**Category:** Web Fundamentals / Learning
**Tags:** HTML, JavaScript, Cookies, Client-Side, Web Basics

---

## 📋 Overview

This room introduces the fundamentals of how websites work, covering HTML structure, JavaScript execution, and basic client-side security issues. It's part of TryHackMe's pre-security path and is designed for absolute beginners.

---

## 📚 Key Concepts

### How Websites Load

When you visit a website, your browser:
1. Sends an **HTTP request** to the server
2. Receives an **HTTP response** containing HTML, CSS, and JavaScript
3. **Renders** the HTML into a visual page
4. Executes any **JavaScript** to add interactivity

---

## 🏗️ HTML Basics

HTML (HyperText Markup Language) structures web content using **tags**:

```html
<!DOCTYPE html>
<html>
  <head>
    <title>My Website</title>
  </head>
  <body>
    <h1>Welcome!</h1>
    <p>This is a paragraph.</p>
    <a href="https://tryhackme.com">Visit TryHackMe</a>
    <img src="image.png" alt="An image">
  </body>
</html>
```

Key tags: `<html>`, `<head>`, `<body>`, `<h1>`–`<h6>`, `<p>`, `<a>`, `<img>`, `<form>`, `<input>`

---

## ⚙️ JavaScript

JavaScript adds dynamic behaviour to web pages:

```javascript
// Alert popup
alert("Hello World!");

// Change page content
document.getElementById("demo").innerHTML = "Changed!";

// Read a cookie
console.log(document.cookie);
```

From a security perspective, JavaScript running in the browser can:
- Read and modify the DOM
- Access cookies (unless `HttpOnly` is set)
- Make network requests (AJAX/fetch)

---

## 🔍 Sensitive Data in Source

Always check the page source (`Ctrl+U` or right-click → View Source) for:
- Hidden form fields containing sensitive data
- Commented-out code with credentials
- API keys hardcoded in JavaScript
- Admin endpoints or hidden directories

---

## 🚩 Room Answers

The room asks questions about HTML structure, JavaScript, and client-side security:

- **What tag makes text bold?** → `<b>` or `<strong>`
- **What tag is used for an image?** → `<img>`
- **What attribute specifies where a link goes?** → `href`
- **Where can you find hidden comments?** → Page source

---

## 📝 Key Takeaways

- Websites are built with HTML (structure), CSS (style), and JavaScript (behaviour).
- The browser processes all of this **client-side** — users can inspect and modify it.
- **Never trust client-side validation alone** — always validate server-side.
- Sensitive data should never be in client-visible HTML, comments, or JavaScript.

---

## 🔗 References

- [MDN Web Docs — HTML](https://developer.mozilla.org/en-US/docs/Web/HTML)
- [TryHackMe — How Websites Work](https://tryhackme.com/room/howwebsiteswork)
- [Walkthrough Reference](https://medium.com/@Cyb3r_Si3rr4/tryhackme-room-16-how-websites-work-519cf56086d6)
