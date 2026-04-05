

**Room:** [Easy Peasy](https://tryhackme.com/room/easypeasyctf)
**Difficulty:** Easy
**Category:** CTF / Web Enumeration / Steganography
**Tags:** Nmap, GoBuster, Steganography, Hash Cracking, SSH

---

## 📋 Overview

Easy Peasy is a beginner CTF room involving web enumeration, hash cracking, and steganography. You'll use Nmap and GoBuster to discover hidden directories, decode Base62 hashes, crack passwords with a custom wordlist, and extract a hidden flag from an image file before logging in via SSH.

---

## 🔧 Tools Used

- `nmap` — Port scanning
- `gobuster` — Directory enumeration
- `CyberChef` — Hash/encoding decoding
- `steghide` — Steganography extraction
- `john` / `hashcat` — Password cracking
- `ssh` — Remote login

---

## 📡 Step 1: Nmap Scan

```bash
nmap -sV -sC -p- <TARGET_IP>
```

**Results:**
| Port | Service |
|------|---------|
| 80 | nginx 1.16.1 |
| 6498 | SSH |
| 65524 | Apache httpd |

- **3 ports open**
- Nginx version: **1.16.1**
- Highest port (65524) runs **Apache**

---

## 🌐 Step 2: Web Enumeration — Port 80 (Nginx)

Run GoBuster to find hidden directories:

```bash
gobuster dir -u http://<TARGET_IP> -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Found: `/hidden` → further enumerate:

```bash
gobuster dir -u http://<TARGET_IP>/hidden -w /usr/share/wordlists/dirbuster/directory-list-2.3-medium.txt
```

Found: `/hidden/whatever`

Visit `http://<TARGET_IP>/hidden/whatever` and **view the page source**. You'll find a Base62-encoded string. Decode it using [CyberChef](https://gchq.github.io/CyberChef/) with the **From Base62** recipe.

> 🚩 **Flag 1:** `flag{f1rs7_fl4g}`

---

## 🌐 Step 3: Apache Enumeration — Port 65524

Check the robots.txt file:

```bash
curl http://<TARGET_IP>:65524/robots.txt
```

You'll find an unusual `User-Agent` string that is an MD5 hash. Decode it using an MD5 reverse lookup (e.g., [md5hashing.net](https://md5hashing.net)).

> 🚩 **Flag 2:** `flag{1m_s3c0nd_fl4g}`

Now view the source of the Apache homepage (`http://<TARGET_IP>:65524`). You'll find Flag 3 embedded directly in the HTML:

> 🚩 **Flag 3:** `flag{9fdafbd64c47471a8f54cd3fc64cd312}`

Also in the page source, look for a hidden Base62-encoded string that reveals a hidden directory path:

Decode it → `/n0th1ng3ls3m4tt3r`

---

## 🔒 Step 4: Crack the Hash

Navigate to `http://<TARGET_IP>:65524/n0th1ng3ls3m4tt3r`. You'll see a page with a binary image and, in the source, a hash.

Use the provided custom wordlist **easypeasy.txt** and John the Ripper:

```bash
john --wordlist=easypeasy.txt hash.txt --format=GOST
```

This reveals the password for the steganography step.

---

## 🖼️ Step 5: Steganography

Download the image from the page. Use `steghide` to extract the hidden data:

```bash
steghide extract -sf image.jpg
```

Enter the cracked password when prompted. This reveals a file containing a **username** and a **Base64-encoded password**.

Decode the password:
```bash
echo "<base64_string>" | base64 -d
```

---

## 🔑 Step 6: SSH Login & Final Flag

Use the extracted credentials to log in via SSH on port **6498**:

```bash
ssh <username>@<TARGET_IP> -p 6498
```

Find the user flag in the home directory:
```bash
cat user.txt
```

Check the crontab for a privilege escalation path:
```bash
cat /etc/crontab
```

A cronjob runs a script as root. Modify the script to spawn a reverse shell or read the root flag:

```bash
cat /root/.root.txt
```

---

## 📝 Key Takeaways

- Always check `robots.txt` and page source during web enumeration — flags and hashes are often hidden there.
- Steganography is a common CTF technique — tools like `steghide`, `binwalk`, and `stegsolve` are essential.
- Custom wordlists (like `easypeasy.txt`) are often required to crack non-standard hashes.
- Crontab misconfigurations are a classic privilege escalation vector.

---

## 🔗 References

- [TryHackMe — Easy Peasy Room](https://tryhackme.com/room/easypeasyctf)
- [Walkthrough Reference](https://medium.com/@ezeprecious.uche/tryhackme-easy-peasy-ctf-walkthrough-77cd7a11c4cb)
