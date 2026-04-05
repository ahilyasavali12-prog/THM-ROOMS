# ☁️ Virtualisation Basics — TryHackMe Walkthrough

**Room:** [Virtualisation Basics](https://tryhackme.com/room/virtualisationbasics)
**Difficulty:** Easy
**Category:** IT Fundamentals / Learning
**Tags:** Virtualisation, VMs, Hypervisor, Cloud, Containers

---

## 📋 Overview

This room introduces **virtualisation** — the technology that enables multiple operating systems to run on one physical machine. It's foundational to cloud computing, DevOps, and cybersecurity labs.

---

## 🖥️ What is Virtualisation?

Virtualisation creates **Virtual Machines (VMs)** — software-based simulations of physical computers. Each VM has its own OS, CPU, RAM, and storage — but shares the physical host's hardware.

```
Physical Machine (Host)
├── Hypervisor
│   ├── VM 1 (Windows 10)
│   ├── VM 2 (Ubuntu Linux)
│   └── VM 3 (Kali Linux)
```

---

## 🔧 Hypervisors

The **hypervisor** is the software layer that creates and manages VMs:

| Type | Description | Examples |
|------|-------------|---------|
| **Type 1 (Bare Metal)** | Runs directly on hardware — no host OS | VMware ESXi, Hyper-V, Xen |
| **Type 2 (Hosted)** | Runs on top of a host OS | VirtualBox, VMware Workstation |

**Type 1** hypervisors are used in enterprise data centres. **Type 2** is what you use on your laptop for security labs.

---

## 📦 Containers vs VMs

| Feature | VM | Container |
|---------|----|---------:|
| Isolation | Full OS | Shared kernel |
| Size | GBs | MBs |
| Startup | Minutes | Seconds |
| Overhead | High | Low |
| Tool | VirtualBox, VMware | Docker, Podman |

Containers share the host OS kernel — they're faster and lighter but offer less isolation than VMs.

---

## ☁️ Virtualisation in Cloud Computing

Cloud providers use virtualisation to offer:
- **IaaS** (Infrastructure as a Service) — Virtual machines on demand (AWS EC2, Azure VMs)
- **PaaS** (Platform as a Service) — Managed environments (Heroku, Google App Engine)
- **SaaS** (Software as a Service) — Applications delivered via browser (Gmail, Office 365)

---

## 🔒 Security Benefits of Virtualisation

- **Sandboxing** — Run malware in a VM without infecting the host
- **Snapshots** — Revert to a clean state after testing
- **Isolation** — Compromise of one VM doesn't directly affect others
- **Lab environments** — TryHackMe, HackTheBox all use VMs!

---

## 🚩 Room Answers

- **What manages VMs?** → Hypervisor
- **What type runs directly on hardware?** → Type 1
- **What is lighter than a VM?** → Container
- **What takes a point-in-time copy of a VM?** → Snapshot

---

## 📝 Key Takeaways

- Virtualisation is the foundation of modern cloud infrastructure.
- Type 1 hypervisors are more efficient; Type 2 hypervisors are more accessible.
- Containers are faster and lighter than VMs but offer less isolation.
- Security professionals use VMs constantly — for malware analysis, penetration testing labs, and safe browsing.

---

## 🔗 References

- [TryHackMe — Virtualisation Basics](https://tryhackme.com/room/virtualisationbasics)
- [Walkthrough Reference](https://medium.com/@RosanaFS/virtualisation-basics-tryhackme-walkthrough-52fd323e7293)
