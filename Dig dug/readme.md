# 🕹️ Dig Dug — TryHackMe Walkthrough

**Room:** [Dig Dug](https://tryhackme.com/room/digdug)
**Difficulty:** Easy
**Category:** DNS / Networking
**Tags:** DNS, nslookup, dig, FQDN

---

## 📋 Overview

Dig Dug is a quick and educational room focused on **DNS enumeration**. You'll use the `dig` command to query a DNS server and retrieve records that reveal the flag. It reinforces understanding of how DNS works and how to interrogate it directly.

---

## 🔧 Tools Used

- `dig` — DNS query tool
- `nslookup` — Alternate DNS lookup tool

---

## 📡 Step 1: Basic Dig Query

The room provides a target IP and tells you to query it for a specific domain. Use `dig` to query the given DNS server:

```bash
dig @<TARGET_IP> givemetheflag.com
```

The DNS server is configured to return the flag as a DNS record in response to this specific query.

---

## 🚩 Step 2: Retrieve the Flag

The response from the `dig` command will contain an **A record** or **TXT record** with the flag embedded:

```
;; ANSWER SECTION:
givemetheflag.com.   0   IN   A   flag{...}
```

> 🚩 **Flag:** Returned directly in the DNS answer section.

---

## 📝 Key Takeaways

- `dig` is one of the most powerful DNS interrogation tools — it can query specific record types (A, MX, TXT, NS, CNAME).
- You can point `dig` at a specific DNS server using the `@` syntax.
- DNS misconfiguration can expose sensitive information — always audit DNS records.
- **Syntax reminder:**
  ```bash
  dig @<dns_server> <domain> <record_type>
  # Example:
  dig @8.8.8.8 google.com MX
  ```

---

## 🔗 References

- [TryHackMe — Dig Dug Room](https://tryhackme.com/room/digdug)
- [Walkthrough Reference](https://medium.com/@rahulkumarmk/dig-dug-easy-try-hack-me-be291242c4c8)
