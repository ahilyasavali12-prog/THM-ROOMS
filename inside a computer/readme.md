# 🖥️ Inside a Computer — TryHackMe Walkthrough

**Room:** [Inside a Computer](https://tryhackme.com/room/insideacomputer)
**Difficulty:** Easy
**Category:** Computer Fundamentals / Learning
**Tags:** Hardware, CPU, RAM, Storage, Motherboard

---

## 📋 Overview

This room explores the physical and logical components inside a computer — CPU, RAM, storage, and more. It's part of the pre-security path and gives essential background for understanding how systems work.

---

## 🧠 CPU (Central Processing Unit)

The CPU is the **brain** of the computer. It:
- Executes instructions from programs
- Performs arithmetic and logic operations
- Manages data flow between components

**Key specs:**
- **Clock speed** (GHz) — How many cycles per second
- **Cores** — Independent processing units (quad-core = 4 CPUs)
- **Cache** — Ultra-fast onboard memory (L1, L2, L3)
- **Architecture** — x86 (32-bit), x86-64 (64-bit), ARM

---

## 💾 RAM (Random Access Memory)

RAM is **temporary, volatile memory** — it stores data currently in use by the OS and programs. When the computer turns off, RAM data is lost.

- More RAM = more programs can run simultaneously
- Measured in GB (8GB, 16GB, 32GB)
- Much faster than storage (SSD/HDD)

---

## 💿 Storage

| Type | Speed | Volatility | Use |
|------|-------|-----------|-----|
| **RAM** | Very fast | Volatile | Active processes |
| **SSD** | Fast | Non-volatile | OS, programs, files |
| **HDD** | Slow | Non-volatile | Mass storage |
| **Cache** | Fastest | Volatile | CPU frequently-used data |

---

## 🔌 Motherboard

The motherboard connects all components:
- CPU socket
- RAM slots
- PCIe slots (GPU, NVMe SSD)
- SATA connectors (HDD/SSD)
- USB, audio, network headers

The **BIOS/UEFI** is firmware stored on the motherboard that initialises hardware at boot.

---

## 🌐 Network Interface Card (NIC)

The NIC connects the computer to a network. It has a **MAC address** — a unique hardware identifier:
```
Format: AA:BB:CC:DD:EE:FF (6 bytes, hexadecimal)
```

---

## 🔒 Security Relevance

Understanding hardware is important for security:
- **RAM forensics** — volatile memory can contain encryption keys, passwords, and running process data
- **Cold boot attacks** — RAM retains data briefly after power-off
- **Physical access** = game over in most security models
- **BIOS/UEFI attacks** — firmware-level persistence is extremely hard to remove

---

## 🚩 Room Answers

- **What processes instructions?** → CPU
- **What is temporary memory?** → RAM
- **What is the hardware address of a network card?** → MAC address
- **What stores data permanently?** → HDD / SSD

---

## 📝 Key Takeaways

- Every layer of a computer can be relevant in security — from physical hardware to the OS.
- RAM is especially important in digital forensics and incident response.
- The motherboard's firmware (BIOS/UEFI) is a target for advanced persistent threats.

---

## 🔗 References

- [TryHackMe — Inside a Computer](https://tryhackme.com/room/insideacomputer)
- [Walkthrough Reference](https://medium.com/@ImAltyb26/tryhackme-inside-a-computer-system-walkthrough-32f7425041d4)
