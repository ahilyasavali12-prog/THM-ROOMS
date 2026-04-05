# 📡 OSI Model — TryHackMe Walkthrough

**Room:** [OSI Model](https://tryhackme.com/room/osimodelzi)
**Difficulty:** Easy
**Category:** Networking / Learning
**Tags:** OSI, Network Layers, Protocols, Encapsulation, TCP/IP

---

## 📋 Overview

This room covers the **OSI (Open Systems Interconnection) Model** — a conceptual framework that standardises how network communication works across 7 layers. It's one of the most fundamental networking concepts for IT and security professionals.

---

## 🏗️ The 7 Layers of the OSI Model

```
Layer 7 — Application    ← User-facing protocols (HTTP, FTP, DNS, SMTP)
Layer 6 — Presentation   ← Encryption, compression, encoding (TLS, JPEG)
Layer 5 — Session        ← Managing sessions (NetBIOS, RPC)
Layer 4 — Transport      ← End-to-end delivery (TCP, UDP)
Layer 3 — Network        ← Routing between networks (IP, ICMP)
Layer 2 — Data Link      ← Node-to-node on same network (Ethernet, MAC, ARP)
Layer 1 — Physical       ← Raw bits over medium (cables, Wi-Fi signals)
```

**Mnemonic (top to bottom):** *"All People Seem To Need Data Processing"*
**Mnemonic (bottom to top):** *"Please Do Not Throw Sausage Pizza Away"*

---

## 🔍 Layer by Layer Breakdown

### Layer 1 — Physical
- Raw electrical signals, light pulses, or radio waves
- Media: coaxial cable, fibre optic, Wi-Fi (802.11)
- Attacks: physical wiretapping, signal jamming

### Layer 2 — Data Link
- MAC addressing, frame creation, error detection
- Protocols: Ethernet, Wi-Fi, PPP, ARP
- Attacks: ARP poisoning, MAC flooding, VLAN hopping

### Layer 3 — Network
- Logical IP addressing and routing
- Protocols: IPv4, IPv6, ICMP, routing protocols (OSPF, BGP)
- Attacks: IP spoofing, ICMP flood, route hijacking

### Layer 4 — Transport
- Reliable (TCP) or unreliable (UDP) delivery
- Port numbers identify services
- Attacks: SYN flood, port scanning, session hijacking

### Layer 5 — Session
- Establishes, maintains, and terminates sessions
- Protocols: NetBIOS, RPC, SQL sessions
- Attacks: session fixation, session replay

### Layer 6 — Presentation
- Data formatting, encryption, compression
- Protocols: TLS/SSL, JPEG, ASCII, Unicode
- Attacks: SSL stripping, format string attacks

### Layer 7 — Application
- Network services for end-user applications
- Protocols: HTTP, HTTPS, FTP, DNS, SMTP, SSH
- Attacks: SQLi, XSS, DNS poisoning, phishing

---

## 📦 Encapsulation

As data travels down the layers, each layer adds a **header** (and sometimes trailer):

```
Application:   [DATA]
Transport:     [TCP Header][DATA]
Network:       [IP Header][TCP Header][DATA]
Data Link:     [Frame Header][IP Header][TCP Header][DATA][Frame Trailer]
Physical:      10110110101010... (bits)
```

At the receiving end, each layer strips its header — **decapsulation**.

---

## 🆚 OSI vs TCP/IP Model

| OSI Layer | TCP/IP Layer |
|-----------|-------------|
| Application (7) | Application |
| Presentation (6) | Application |
| Session (5) | Application |
| Transport (4) | Transport |
| Network (3) | Internet |
| Data Link (2) | Network Access |
| Physical (1) | Network Access |

The TCP/IP model is what's actually implemented in practice. OSI is the theoretical reference model.

---

## 🚩 Room Answers

- **How many layers does OSI have?** → 7
- **What layer handles routing?** → Layer 3 (Network)
- **What layer handles encryption?** → Layer 6 (Presentation)
- **What layer does HTTP operate at?** → Layer 7 (Application)
- **What layer does MAC addressing operate at?** → Layer 2 (Data Link)

---

## 📝 Key Takeaways

- The OSI model helps **diagnose network problems** by isolating which layer is failing.
- Security attacks often target specific layers — understanding OSI helps map attack types to layers.
- In practice, the **TCP/IP model** is used, but OSI remains the universal reference framework.

---

## 🔗 References

- [TryHackMe — OSI Model](https://tryhackme.com/room/osimodelzi)
- [Walkthrough Reference](https://medium.com/@ddudi/osi-model-tryhackme-walkthrough-0c7e582c148d)
