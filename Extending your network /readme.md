# 🌐 Extending Your Network — TryHackMe Walkthrough

**Room:** [Extending Your Network](https://tryhackme.com/room/extendingyournetwork)
**Difficulty:** Easy
**Category:** Networking / Learning
**Tags:** Firewalls, VPN, Port Forwarding, NAT, Proxy, Network Security

---

## 📋 Overview

This room covers how networks are extended beyond the local LAN — including **port forwarding**, **firewalls**, **VPNs**, and **proxies**. These concepts are critical for understanding both network infrastructure and how attackers traverse network boundaries.

---

## 🔀 Port Forwarding

Port forwarding allows external traffic to reach an internal service:

```
Internet ──► Router (public IP: 1.2.3.4:80) ──► Internal Server (192.168.1.10:80)
```

Use case: Hosting a web server on a private network, accessible from the internet.

**Security risk:** Every forwarded port is an exposed attack surface. Use only what's necessary.

```
Router rule: External port 80 → Internal 192.168.1.10:80
```

---

## 🔥 Firewalls

Firewalls control traffic based on rules — allowing or blocking based on IP, port, and protocol.

### Types of Firewalls

| Type | How It Works |
|------|-------------|
| **Stateless** | Filters individual packets against rules, no connection state |
| **Stateful** | Tracks connection state — knows if a packet belongs to an established session |
| **Application (WAF)** | Inspects application-layer content (Layer 7) |
| **Next-Gen (NGFW)** | Combines stateful + app inspection + IDS/IPS + threat intelligence |

### Firewall Rules Example

```
ALLOW  TCP  ANY → 192.168.1.10:80   # Allow web traffic in
ALLOW  TCP  ANY → 192.168.1.10:443  # Allow HTTPS in
DENY   ALL  ANY → ANY               # Block everything else (implicit deny)
```

**Implicit deny:** Best practice — deny all by default, only allow what's explicitly needed.

---

## 🔒 VPN (Virtual Private Network)

A VPN creates an **encrypted tunnel** over the public internet, making remote devices act as if they're on the local network.

```
Home PC ──[encrypted tunnel]──► VPN Server ──► Corporate LAN
```

**VPN Protocols:**
| Protocol | Description |
|----------|-------------|
| **OpenVPN** | Open-source, highly configurable |
| **IPSec** | Strong encryption, commonly used in enterprise |
| **WireGuard** | Modern, fast, simple |
| **L2TP/IPSec** | Older, widely supported |

**Security uses of VPNs:**
- Secure remote access to corporate networks
- Bypass geographic restrictions
- Protect traffic on public Wi-Fi
- TryHackMe uses OpenVPN for connecting to lab machines!

---

## 🌐 Proxies

A proxy is an intermediary that forwards requests on behalf of a client:

```
Client ──► Proxy ──► Internet ──► Server
```

| Proxy Type | Purpose |
|-----------|---------|
| **Forward Proxy** | Clients connect to internet via proxy (cache, filter) |
| **Reverse Proxy** | Sits in front of servers (load balancing, WAF, CDN) |
| **Transparent Proxy** | Client unaware traffic is proxied (ISP filtering) |
| **SOCKS Proxy** | Low-level proxy for any traffic type (used in pen testing) |

**Burp Suite** is a web proxy used in security testing to intercept and modify HTTP traffic.

---

## 🔢 NAT (Network Address Translation)

NAT allows multiple private IP devices to share one public IP:

```
Internal devices:
  192.168.1.10  ─┐
  192.168.1.11  ─┤──► Router (NAT) ──► Public IP: 1.2.3.4 ──► Internet
  192.168.1.12  ─┘
```

NAT hides internal network structure from the outside — a side effect that provides some security.

---

## 🚩 Room Answers

- **What allows external traffic to reach internal services?** → Port forwarding
- **What creates an encrypted tunnel over the internet?** → VPN
- **What firewall type tracks connection state?** → Stateful
- **What sits in front of servers to distribute traffic?** → Reverse Proxy

---

## 📝 Key Takeaways

- Every open port is a potential attack vector — minimise exposure with strict firewall rules.
- VPNs are widely used in corporate environments — understanding them helps with both administration and attacks (e.g., VPN credential stuffing).
- **Burp Suite** acts as a forward proxy during web application pentesting.
- NAT provides obscurity but not security — never rely on it as a security control.

---

## 🔗 References

- [TryHackMe — Extending Your Network](https://tryhackme.com/room/extendingyournetwork)
- [Walkthrough Reference](https://medium.com/@cyberFran6/extending-your-network-tryhackme-write-up-walkthrough-9b7799a2697e)
