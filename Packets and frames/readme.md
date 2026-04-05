# 📦 Packets & Frames — TryHackMe Walkthrough

**Room:** [Packets & Frames](https://tryhackme.com/room/packetsframes)
**Difficulty:** Easy
**Category:** Networking / Learning
**Tags:** TCP, UDP, IP, Packets, Frames, Three-Way Handshake, Wireshark

---

## 📋 Overview

This room explains how data is packaged for network transmission — the difference between **packets** (Layer 3) and **frames** (Layer 2), how TCP and UDP work, and the mechanics of the TCP handshake.

---

## 📦 Packets vs Frames

| Term | Layer | Contains | Addressing |
|------|-------|----------|-----------|
| **Frame** | Layer 2 (Data Link) | Data + MAC headers | MAC addresses |
| **Packet** | Layer 3 (Network) | Frame payload + IP headers | IP addresses |

A frame is used for communication **within the same network**. A packet is used when data needs to be **routed across networks**.

---

## 🌐 IP Packets

An IP packet header contains:

```
Version | Header Length | DSCP | Total Length
Identification | Flags | Fragment Offset
TTL | Protocol | Header Checksum
Source IP Address
Destination IP Address
[Options if present]
[Data Payload]
```

Key fields:
- **TTL (Time To Live):** Decremented at each router hop — prevents infinite loops. When it hits 0, the packet is discarded (ICMP TTL Exceeded sent back).
- **Protocol:** Identifies the encapsulated protocol (6 = TCP, 17 = UDP, 1 = ICMP)
- **Flags:** Control fragmentation (DF = Don't Fragment, MF = More Fragments)

---

## 🔀 TCP vs UDP

| Feature | TCP | UDP |
|---------|-----|-----|
| Connection | Connection-oriented | Connectionless |
| Reliability | Guaranteed delivery, retransmits | Best-effort, no retransmit |
| Order | In-order delivery | No ordering |
| Speed | Slower (overhead) | Faster (less overhead) |
| Use cases | Web, email, SSH, FTP | DNS, VoIP, gaming, streaming |

---

## 🤝 TCP Three-Way Handshake

Before TCP data transfer, a connection is established:

```
Client                      Server
  │                            │
  │──── SYN (seq=0) ──────────►│  "I want to connect"
  │                            │
  │◄─── SYN-ACK (seq=0,ack=1) ─│  "OK, I'm ready"
  │                            │
  │──── ACK (ack=1) ──────────►│  "Great, let's go"
  │                            │
  │═══════ Data Transfer ══════│
```

**TCP Termination (Four-Way):**
```
FIN → ACK → FIN → ACK
```

---

## 🛡️ TCP Flags

| Flag | Meaning |
|------|---------|
| **SYN** | Synchronise — initiate connection |
| **ACK** | Acknowledgement |
| **FIN** | Finish — close connection |
| **RST** | Reset — abort connection |
| **PSH** | Push — send data immediately |
| **URG** | Urgent data |

**Security note:** Port scanners like Nmap manipulate these flags to discover open ports:
- SYN scan (`-sS`) — sends SYN, doesn't complete handshake (stealth)
- FIN scan — sends FIN to closed ports (returns RST)

---

## 🔍 Analysing with Wireshark

Wireshark captures and dissects packets:

```bash
# Capture on interface eth0
wireshark -i eth0

# Common display filters:
tcp                         # All TCP traffic
http                        # HTTP requests/responses
ip.addr == 192.168.1.1      # Traffic to/from specific IP
tcp.port == 443             # HTTPS traffic
tcp.flags.syn == 1          # SYN packets only
```

---

## 🚩 Room Answers

- **What protocol is connection-oriented?** → TCP
- **What are the three steps in the TCP handshake?** → SYN, SYN-ACK, ACK
- **What flag initiates a connection?** → SYN
- **What decrements at each router hop?** → TTL
- **What protocol does not guarantee delivery?** → UDP

---

## 📝 Key Takeaways

- TCP guarantees delivery at the cost of speed; UDP is fast but unreliable.
- The three-way handshake is the foundation of every TCP connection — and a target for attacks like SYN floods.
- Understanding packet structure is essential for Wireshark analysis, IDS/IPS rule writing, and network forensics.
- TTL values can hint at OS type (Windows default: 128, Linux default: 64).

---

## 🔗 References

- [Wireshark User Guide](https://www.wireshark.org/docs/wsug_html_chunked/)
- [TryHackMe — Packets & Frames](https://tryhackme.com/room/packetsframes)
- [Walkthrough Reference](https://medium.com/@MrBadasss/packets-frames-tryhackme-3286299bc4eb)
