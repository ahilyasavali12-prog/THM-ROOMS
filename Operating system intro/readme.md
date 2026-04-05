# 🐧 Operating System Introduction — TryHackMe Walkthrough

**Room:** [Operating Systems Introduction](https://tryhackme.com/room/osintro)
**Difficulty:** Easy
**Category:** IT Fundamentals / Learning
**Tags:** Operating Systems, Linux, Windows, Kernel, File System

---

## 📋 Overview

This room introduces **Operating Systems (OS)** — the software that manages hardware and provides services for applications. Understanding the OS is essential for cybersecurity, as most attacks target or leverage OS-level features.

---

## 🧱 What is an Operating System?

The OS is the **intermediary between hardware and software**. It provides:

```
Applications (Chrome, VSCode, Nmap)
        ↕
Operating System (Linux, Windows, macOS)
        ↕
Hardware (CPU, RAM, Disk, Network)
```

---

## 🔧 Core OS Components

| Component | Function |
|-----------|----------|
| **Kernel** | Core — manages CPU, memory, and hardware access |
| **Shell** | Interface to interact with the OS (bash, PowerShell) |
| **File System** | Organises how data is stored |
| **Process Manager** | Controls running programs |
| **Memory Manager** | Allocates and frees RAM |
| **Device Drivers** | Translates OS commands to hardware |

---

## 🗂️ File Systems

**Linux:**
```
/           Root — top of the hierarchy
├── /home   User home directories
├── /etc    Configuration files
├── /var    Variable data (logs)
├── /tmp    Temporary files (world-writable!)
├── /bin    Essential binaries
├── /sbin   System binaries (root)
└── /root   Root user's home
```

**Windows:**
```
C:\
├── C:\Windows\       OS files
├── C:\Program Files\ Installed apps
├── C:\Users\         User profiles
└── C:\Temp           Temporary files
```

---

## 👤 User Accounts and Permissions

**Linux:**
- Root (UID 0) — superuser with full access
- Regular users — restricted access
- Groups — users can belong to groups with shared permissions

```bash
whoami          # Current user
id              # User ID and group memberships
sudo command    # Run as root
su - username   # Switch user
```

**Windows:**
- Administrator — full access
- Standard User — restricted
- SYSTEM — highest privilege (above Administrator)

---

## 🔒 Security Relevance

| Concept | Security Importance |
|---------|-------------------|
| **File permissions** | Misconfigured permissions = privilege escalation |
| **SUID binaries** | Executables that run as owner (root) |
| **Crontabs** | Scheduled tasks — common privesc vector |
| **Services** | Background processes — misconfigurations can be exploited |
| **/tmp** | World-writable — used for staging payloads |

---

## 🚩 Room Answers

- **What manages hardware resources?** → Kernel
- **What is the top of the Linux directory hierarchy?** → / (root)
- **What user has UID 0?** → root
- **What stores OS files in Windows?** → C:\Windows

---

## 📝 Key Takeaways

- The OS kernel is the most privileged software on a system — kernel exploits are extremely powerful.
- File system knowledge is essential for finding flags, credentials, and privilege escalation paths.
- Linux is the dominant OS in servers, cloud infrastructure, and security tools.
- Understanding both Linux and Windows is important for a well-rounded security professional.

---

## 🔗 References

- [Linux Filesystem Hierarchy Standard](https://refspecs.linuxfoundation.org/FHS_3.0/fhs-3.0.html)
- [TryHackMe — OS Introduction Room](https://tryhackme.com/room/osintro)
- [Walkthrough Reference](https://medium.com/h7w/operating-systems-introduction-tryhackme-d2b89084a6b2)
