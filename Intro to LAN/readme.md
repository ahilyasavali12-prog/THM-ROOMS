# 🔌 Intro to LAN — TryHackMe Walkthrough

**Room:** [Intro to LAN](https://tryhackme.com/room/introtolan)
**Difficulty:** Easy
**Category:** Networking / Learning
**Tags:** LAN, Topology, Switches, Routers, ARP, DHCP, Subnetting

---

## 📋 Overview

This room dives deeper into **Local Area Networks (LANs)** — how they're physically and logically structured, the devices that make them work, and protocols like ARP and DHCP that operate within them.

---

## 🕸️ Network Topologies

How devices are connected defines the topology:

| Topology | Layout | Pros | Cons |
|----------|--------|------|------|
| **Star** | All connect to central switch | Easy to manage, fault-tolerant | Switch = single point of failure |
| **Bus** | Single cable backbone | Simple, cheap | One break = whole network down |
| **Ring** | Devices in a loop | Predictable performance | One failure disrupts ring |
| **Mesh** | Every device connects to every other | Highly redundant | Very expensive, complex |

Most modern networks use **star topology** with switches.

---

## 🔧 Key LAN Devices

### Switch
- Operates at **Layer 2** (Data Link)
- Connects devices within a LAN
- Uses **MAC address table** to forward frames only to the correct port
- Reduces network congestion compared to hubs

### Router
- Operates at **Layer 3** (Network)
- Connects **different networks** together
- Uses IP addresses to route packets
- Your home router connects your LAN to the ISP's WAN

### Hub (Legacy)
- Broadcasts all traffic to all ports — no intelligence
- Replaced by switches in modern networks

---

## 📋 Subnetting

Subnetting divides a large network into smaller logical networks:

```
Network:    192.168.1.0/24
Subnet Mask: 255.255.255.0

Usable hosts: 192.168.1.1 – 192.168.1.254 (254 hosts)
Network addr: 192.168.1.0
Broadcast:    192.168.1.255
```

**CIDR notation:** `/24` means 24 bits are the network portion, 8 bits for hosts.

| CIDR | Subnet Mask | Hosts |
|------|------------|-------|
| /24 | 255.255.255.0 | 254 |
| /25 | 255.255.255.128 | 126 |
| /16 | 255.255.0.0 | 65,534 |

---

## 🔄 ARP (Address Resolution Protocol)

ARP resolves an IP address to a MAC address within a LAN:

```
"Who has 192.168.1.1? Tell 192.168.1.50"  ← ARP Request (broadcast)
"192.168.1.1 is at AA:BB:CC:DD:EE:FF"     ← ARP Reply (unicast)
```

**ARP Cache Poisoning:** An attacker sends fake ARP replies to redirect traffic — a foundation of **Man-in-the-Middle** attacks.

```bash
# View ARP cache:
arp -a
```

---

## 🏷️ DHCP (Dynamic Host Configuration Protocol)

DHCP automatically assigns IP configuration to devices:

```
DHCP Discover  → (broadcast) "I need an IP!"
DHCP Offer     ← Server: "Take 192.168.1.100"
DHCP Request   → "Yes, I'll take that IP"
DHCP ACK       ← "It's yours for 24 hours (lease time)"
```

DHCP assigns: IP address, subnet mask, default gateway, DNS server.

---

## 🚩 Room Answers

- **What topology connects all to a central switch?** → Star
- **What device connects different networks?** → Router
- **What protocol assigns IP addresses automatically?** → DHCP
- **What resolves IPs to MAC addresses?** → ARP

---

## 📝 Key Takeaways

- Switches are the core of modern LANs — they use MAC tables for efficient forwarding.
- Subnetting is essential for network design and security segmentation.
- ARP is fundamental but insecure — ARP spoofing is a classic LAN attack.
- DHCP starvation and rogue DHCP servers are common network attacks.

---

## 🔗 References

- [TryHackMe — Intro to LAN](https://tryhackme.com/room/introtolan)
- [Walkthrough Reference](https://medium.com/@MrBadasss/intro-to-lan-tryhackme-1ac242731dce)
