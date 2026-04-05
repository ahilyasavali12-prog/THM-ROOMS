# 🛡️ Defensive Security Intro — TryHackMe Walkthrough

**Room:** [Defensive Security Intro](https://tryhackme.com/room/defensivesecurityintro)
**Difficulty:** Easy
**Category:** Learning / Blue Team
**Tags:** Blue Team, SOC, SIEM, DFIR, Threat Intelligence, IDS

---

## 📋 Overview

This room introduces **defensive security** — the practice of protecting systems from attacks. You'll learn about key blue team concepts including SOC, SIEM, Digital Forensics, and Incident Response, and complete a hands-on simulation of triaging a SIEM alert.

---

## 🔵 What is Defensive Security?

While offensive security finds vulnerabilities, **defensive security** focuses on:
- **Preventing** breaches through hardening and monitoring
- **Detecting** attacks in progress via SIEM and IDS
- **Responding** to incidents to contain and recover
- **Investigating** to understand what happened

---

## 🏢 Security Operations Centre (SOC)

A SOC is a team of analysts who monitor an organisation's security 24/7:

| SOC Tier | Role |
|----------|------|
| **Tier 1** | Alert triage — review SIEM alerts, filter false positives |
| **Tier 2** | Deep investigation — hunt threats, investigate incidents |
| **Tier 3** | Threat hunting, red team collaboration, advanced analysis |
| **SOC Manager** | Oversees operations and reporting |

---

## 📡 Key Defensive Tools

| Tool | Purpose |
|------|---------|
| **SIEM** | Aggregates logs, detects patterns (Splunk, Microsoft Sentinel) |
| **IDS/IPS** | Detects/prevents suspicious network traffic (Snort, Suricata) |
| **EDR** | Endpoint Detection & Response (CrowdStrike, SentinelOne) |
| **Firewall** | Controls network traffic based on rules |
| **Honeypot** | Decoy system to detect and study attackers |

---

## 🔍 Threat Intelligence

Understanding who is attacking and how:
- **IOCs** (Indicators of Compromise): IPs, domains, file hashes linked to attacks
- **TTPs** (Tactics, Techniques, Procedures): How attackers operate
- **MITRE ATT&CK Framework**: A knowledge base of real-world adversary techniques

---

## 🔬 Digital Forensics and Incident Response (DFIR)

**Digital Forensics:**
- Collecting and preserving evidence after an incident
- Analysing disk images, memory dumps, network captures
- Maintaining chain of custody for legal proceedings

**Incident Response (IR) Phases:**
```
1. Preparation      → Policies, playbooks, tools ready
2. Identification   → Detect and confirm incident
3. Containment      → Stop spread (short-term + long-term)
4. Eradication      → Remove malware, fix vulnerability
5. Recovery         → Restore systems to normal
6. Lessons Learned  → Post-incident review
```

---

## 💻 Hands-On: SIEM Alert Triage

The room includes a simulated SIEM interface. You'll review alerts and determine whether they represent:
- **True Positive** — Real attack, needs investigation
- **False Positive** — Legitimate activity flagged incorrectly

Common triage questions:
- What is the source IP? Is it internal or external?
- What is the destination? Is it a critical asset?
- What is the alert type? Does the activity make sense?
- Is this a known IOC?

> 🚩 **Flag:** Found by correctly triaging the SIEM alert in the room's interactive component.

---

## 📝 Key Takeaways

- Defensive security is not passive — it requires active monitoring, hunting, and response.
- The **MITRE ATT&CK** framework is the industry standard for understanding adversary behaviour.
- SOC analysts must quickly distinguish true positives from the noise of false positives.
- DFIR skills are in high demand — every breach requires investigation.

---

## 🔗 References

- [MITRE ATT&CK Framework](https://attack.mitre.org/)
- [TryHackMe — Defensive Security Intro](https://tryhackme.com/room/defensivesecurityintro)
- [Walkthrough Reference](https://medium.com/@TRedEye/defensive-security-intro-tryhackme-walkthrough-13be116cc3e2)
