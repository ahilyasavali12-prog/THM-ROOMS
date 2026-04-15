# 🌐 What is Networking? — TryHackMe Walkthrough

**Room:** [What is Networking?](https://tryhackme.com/room/whatisnetworking)
**Difficulty:** Easy
**Category:** Networking / Learning
**Tags:** Networking, IP, MAC, Internet, LAN, WAN

---

## 📋 Overview

This foundational room introduces **computer networking** — what networks are, how devices communicate, and the key concepts that underpin all of IT and cybersecurity. Part of the Pre-Security path.

---

## 🔗 What is a Network?

A network is two or more devices connected together so they can share data. Networks range in size:

| Type | Scope | Example |
|------|-------|---------|
| **PAN** | Personal Area | Bluetooth devices |
| **LAN** | Local Area | Home / office Wi-Fi |
| **MAN** | Metropolitan Area | City-wide network |
| **WAN** | Wide Area | The Internet |

---

## 📡 How Devices Are Identified

Every device on a network has two addresses:

### IP Address (Internet Protocol)
- A logical address — can change
- Used to route data across networks
- Two versions:

```
IPv4: 192.168.1.1     (32-bit, ~4 billion addresses)
IPv6: 2001:db8::1     (128-bit, virtually unlimited)
```

**Private IP ranges (not routable on the internet):**
```
10.0.0.0    – 10.255.255.255
172.16.0.0  – 172.31.255.255
192.168.0.0 – 192.168.255.255
```

### MAC Address (Media Access Control)
- A physical address burned into the network card
- Used for communication within the same network
- Format: `AA:BB:CC:DD:EE:FF` (6 bytes, hexadecimal)
- First 3 bytes = manufacturer (OUI), last 3 = device ID
  
---

## 🔄 How Data Travels

Data is broken into **packets** — small chunks with:
- **Header** — source IP, destination IP, protocol
- **Payload** — the actual data
- **Trailer** — error checking

Packets travel independently and are reassembled at the destination.

---

## 🌍 The Internet

The internet is a global network of networks — millions of LANs connected together via routers and undersea cables.

```
Your Device → Home Router → ISP Network → Internet Backbone → Destination Server
```

---

## 🔒 Security Relevance

| Concept | Security Importance |
|---------|-------------------|
| IP addresses | Attackers spoof IPs; used for geolocation and attribution |
| MAC addresses | MAC spoofing bypasses MAC-based access controls |
| Packet inspection | Firewalls and IDS/IPS work by inspecting packets |
| Private IPs | Internal network ranges are targets after initial access |

---

## 🚩 Room Answers

- **What does LAN stand for?** → Local Area Network
- **What is used to identify devices on a network?** → IP address
- **What is the physical hardware address?** → MAC address
- **How many bits is an IPv4 address?** → 32

---

## 📝 Key Takeaways

- Networking is the backbone of everything in IT — understanding it deeply makes you a better attacker and defender.
- IP addresses identify where traffic goes; MAC addresses identify who's who on a local network.
- Packets are the fundamental unit of network communication.

---

## 🔗 References

- [TryHackMe — What is Networking?](https://tryhackme.com/room/whatisnetworking)
- [Walkthrough Reference](https://medium.com/@ms3.sooraj.sivadas/what-is-networking-tryhackme-walkthrough-ca3acceed632)
