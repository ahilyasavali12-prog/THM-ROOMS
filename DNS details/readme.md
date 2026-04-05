# 🔍 DNS in Detail — TryHackMe Walkthrough

**Room:** [DNS in Detail](https://tryhackme.com/room/dnsindetail)
**Difficulty:** Easy
**Category:** Networking / Learning
**Tags:** DNS, nslookup, dig, Records, Domain Names

---

## 📋 Overview

This room provides a thorough understanding of **DNS (Domain Name System)** — the internet's phonebook. You'll learn about DNS record types, the resolution process, and how to query DNS using command-line tools.

---

## 🌐 What is DNS?

DNS translates human-readable domain names into IP addresses:
```
tryhackme.com  →  104.22.55.228
```

Without DNS, you'd need to remember IP addresses to visit websites.

---

## 🏗️ DNS Hierarchy

```
Root (.)
  └── Top-Level Domain (TLD): .com, .org, .uk
        └── Second-Level Domain: tryhackme.com
              └── Subdomain: www.tryhackme.com
```

---

## 📋 DNS Record Types

| Record | Purpose | Example |
|--------|---------|---------|
| **A** | Maps domain → IPv4 address | `tryhackme.com → 104.22.55.228` |
| **AAAA** | Maps domain → IPv6 address | `tryhackme.com → 2606:4700::...` |
| **CNAME** | Alias for another domain | `www → tryhackme.com` |
| **MX** | Mail server for domain | `mail.tryhackme.com` |
| **TXT** | Arbitrary text (used for SPF, DKIM, verification) | `v=spf1 include:...` |
| **NS** | Nameservers for domain | `ns1.cloudflare.com` |
| **SOA** | Start of Authority — zone info | Serial, refresh, retry timers |

---

## 🔄 DNS Resolution Process

```
1. Your browser checks local cache
2. OS checks /etc/hosts
3. Query sent to Recursive DNS Resolver (your ISP or 8.8.8.8)
4. Resolver checks its cache
5. Resolver queries Root Nameserver → gets TLD nameserver
6. Resolver queries TLD Nameserver → gets Authoritative nameserver
7. Resolver queries Authoritative Nameserver → gets the IP
8. IP returned to your browser, cached with TTL
```

**TTL (Time To Live):** How long a DNS record is cached (in seconds). Lower TTL = faster propagation of changes.

---

## 🛠️ Querying DNS

**Using `nslookup`:**
```bash
nslookup tryhackme.com           # A record
nslookup -type=MX tryhackme.com  # MX record
nslookup -type=TXT tryhackme.com # TXT record
```

**Using `dig`:**
```bash
dig tryhackme.com              # A record
dig tryhackme.com MX           # MX record
dig tryhackme.com TXT          # TXT record
dig @8.8.8.8 tryhackme.com     # Query specific DNS server
dig tryhackme.com +short        # Short output (IP only)
```

---

## 🚩 Room Answers

- **What does DNS stand for?** → Domain Name System
- **What record type is used for email?** → MX
- **What record creates an alias?** → CNAME
- **What field controls caching duration?** → TTL

---

## 📝 Key Takeaways

- DNS is a hierarchical, distributed system — no single server knows everything.
- **MX records** direct email; misconfigured MX records can cause email delivery failure or interception.
- **TXT records** are used for domain ownership verification, SPF, DKIM, and DMARC.
- DNS attacks include: DNS spoofing/cache poisoning, DNS amplification DDoS, DNS hijacking.

---

## 🔗 References

- [RFC 1034 — DNS Concepts](https://datatracker.ietf.org/doc/html/rfc1034)
- [TryHackMe — DNS in Detail](https://tryhackme.com/room/dnsindetail)
- [Walkthrough Reference](https://medium.com/@hammaadmughal/tryhackme-dns-in-detail-8789f601da6e)
