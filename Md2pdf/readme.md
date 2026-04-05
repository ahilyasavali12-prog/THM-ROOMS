# 📄 MD2PDF — TryHackMe Walkthrough

**Room:** [MD2PDF](https://tryhackme.com/room/md2pdf)
**Difficulty:** Easy
**Category:** Web / SSRF / XSS
**Tags:** SSRF, Server-Side Request Forgery, Markdown, PDF generation

---

## 📋 Overview

MD2PDF is a web room built around a Markdown-to-PDF converter application. The vulnerability lies in **Server-Side Request Forgery (SSRF)** — where the server-side PDF generator can be tricked into making requests to internal resources, exposing the flag.

---

## 🔧 Tools Used

- Browser / Burp Suite — Request manipulation
- `curl` — HTTP requests

---

## 🌐 Step 1: Explore the Application

Navigate to `http://<TARGET_IP>`. The site presents a Markdown editor that converts input to PDF. Experiment with basic Markdown to confirm it works.

---

## 🔍 Step 2: Test for SSRF

The PDF is likely generated server-side by a headless browser (like wkhtmltopdf or Puppeteer). These tools follow links and load resources — making them vulnerable to SSRF.

Inject an HTML `<iframe>` or `<img>` tag pointing to an internal service:

```markdown
<iframe src="http://localhost/"></iframe>
```

Or attempt to access internal admin endpoints:

```markdown
<iframe src="http://localhost:8080/admin"></iframe>
```

---

## 🚩 Step 3: Access Internal Resources

The application likely has an internal admin page that is not publicly accessible. Use SSRF to reach it:

```markdown
<iframe src="http://localhost/admin" width="800" height="600"></iframe>
```

When the PDF is generated, the server fetches the internal `/admin` page and embeds it in the PDF — revealing the flag.

> 🚩 **Flag:** Visible in the generated PDF, loaded from the internal admin page.

---

## 📝 Key Takeaways

- **SSRF** allows attackers to make the server issue requests to unintended destinations — including internal services not exposed to the internet.
- PDF generators that use headless browsers are especially susceptible because they render full HTML including embedded resources.
- Always restrict outbound connections from PDF generation services to prevent SSRF.
- Common SSRF targets: `localhost`, `169.254.169.254` (AWS metadata), internal IPs.

---

## 🔗 References

- [OWASP — SSRF](https://owasp.org/www-community/attacks/Server_Side_Request_Forgery)
- [TryHackMe — MD2PDF Room](https://tryhackme.com/room/md2pdf)
- [Walkthrough Reference](https://medium.com/@janijay007/md2pdf-tryhackme-walkthrough-73d612b907b3)
