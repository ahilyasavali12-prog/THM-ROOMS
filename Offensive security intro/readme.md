# ⚔️ Intro to Offensive Security — TryHackMe Walkthrough

**Room:** [Intro to Offensive Security](https://tryhackme.com/room/introtooffensivesecurity)
**Difficulty:** Easy
**Category:** Learning / Offensive Security
**Tags:** Offensive Security, Hacking, GoBuster, Web, Introduction

---

## 📋 Overview

This introductory room demonstrates what offensive security means in practice. You'll use **GoBuster** to find a hidden web page and simulate a simple hack — getting a hands-on feel for penetration testing within a safe, legal environment.

---

## 🔧 Tools Used

- `gobuster` — Directory/file brute-forcing tool

---

## 🤔 What is Offensive Security?

**Offensive security** (also called ethical hacking or penetration testing) involves:
- **Thinking like an attacker** to find vulnerabilities before real criminals do
- Testing systems **with permission** from the owner
- Documenting findings and recommending fixes

This is contrasted with **defensive security**, which focuses on protecting systems.

---

## 🌐 Step 1: Visit the Target

A fake bank website is running on the target machine. Navigate to `http://<TARGET_IP>` to see a simple banking application.

---

## 🔍 Step 2: Run GoBuster

GoBuster discovers hidden pages and directories by brute-forcing URLs:

```bash
gobuster dir -u http://fakebank.com -w /usr/share/wordlists/dirbuster/directory-list-2.3-small.txt
```

**Flags explained:**
- `dir` — directory/file mode
- `-u` — target URL
- `-w` — wordlist to use

GoBuster will output discovered paths, including hidden admin or config pages not linked from the homepage.

---

## 🚩 Step 3: Access the Hidden Page

From GoBuster's results, you'll find a hidden path like `/bank-transfer`. Navigate to it:

```
http://<TARGET_IP>/bank-transfer
```

This simulates finding a hidden, unprotected admin page. In the real world, this could allow an attacker to perform unauthorised actions.

> 🚩 **Flag:** Displayed after accessing the hidden page and completing the simulated action.

---

## ⚖️ Legal and Ethical Note

Hacking without permission is **illegal** and can result in criminal prosecution. Always ensure you have written authorisation before testing any system. Platforms like TryHackMe, HackTheBox, and your own lab provide safe, legal environments to practise.

---

## 📝 Key Takeaways

- **GoBuster** is a fundamental tool for web enumeration — always run directory brute-forcing early in a web pentest.
- Hidden pages exist on many websites; developers often forget to remove or protect them.
- Offensive security skills are used **ethically** to improve security posture.
- Popular wordlists: `/usr/share/wordlists/dirbuster/`, SecLists on GitHub.

---

## 🔗 References

- [GoBuster GitHub](https://github.com/OJ/gobuster)
- [SecLists Wordlists](https://github.com/danielmiessler/SecLists)
- [TryHackMe — Intro to Offensive Security](https://tryhackme.com/room/introtooffensivesecurity)
- [Walkthrough Reference](https://medium.com/@ms3.sooraj.sivadas/intro-to-offensive-security-tryhackme-walkthrough-f988366c4c1b)
