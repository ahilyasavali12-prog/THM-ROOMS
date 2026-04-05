# 📊 Monday Monitor — TryHackMe Walkthrough

**Room:** [Monday Monitor](https://tryhackme.com/room/mondaymonitor)
**Difficulty:** Easy
**Category:** Blue Team / SIEM / Splunk
**Tags:** Splunk, Log Analysis, SIEM, Blue Team, SOC

---

## 📋 Overview

Monday Monitor is a blue team room using **Splunk** to investigate a security incident. You'll query logs to identify indicators of compromise, trace attacker activity, and answer investigation questions — simulating a real SOC analyst workflow.

---

## 🔧 Tools Used

- **Splunk** — SIEM and log analysis platform (accessible via browser on the target)

---

## 🌐 Step 1: Access Splunk

Navigate to the Splunk instance at `http://<TARGET_IP>:8000`. Log in with the credentials provided in the room.

---

## 🔍 Step 2: Search the Logs

Use the Splunk Search & Reporting app to query the available data. Start with broad searches and narrow down:

```splunk
index=* earliest=-24h
```

Identify available sourcetypes:

```splunk
index=* | stats count by sourcetype
```

---

## 🔎 Step 3: Investigate the Incident

Follow the questions in the room. Common investigation steps include:

**Find failed login attempts:**
```splunk
index=* EventCode=4625 | stats count by Account_Name
```

**Identify suspicious processes:**
```splunk
index=* sourcetype=WinEventLog EventCode=4688 | table _time, Account_Name, New_Process_Name, Process_Command_Line
```

**Look for PowerShell execution:**
```splunk
index=* New_Process_Name="*powershell*" | table _time, Account_Name, Process_Command_Line
```

**Find network connections:**
```splunk
index=* sourcetype=sysmon EventCode=3 | table _time, Image, DestinationIp, DestinationPort
```

---

## 🚩 Capture the Flags

Answer each question by running targeted Splunk queries. The answers and flags are revealed through the log data:

- Who was the user involved in the incident?
- What malicious process was executed?
- What was the attacker's IP address?
- What was the persistence mechanism used?

> 🚩 **Flags:** Extracted from log entries by answering each investigation question.

---

## 📝 Key Takeaways

- **Splunk SPL (Search Processing Language)** is a core skill for SOC analysts.
- Event IDs to know: 4625 (failed login), 4624 (successful login), 4688 (process creation), 4720 (user created).
- Sysmon provides much richer process and network telemetry than default Windows logging.
- Blue team skills — log analysis, threat hunting, and SIEM usage — are just as valuable as offensive skills.

---

## 🔗 References

- [Splunk SPL Cheat Sheet](https://www.splunk.com/en_us/resources/splunk-quick-reference-guide.html)
- [TryHackMe — Monday Monitor Room](https://tryhackme.com/room/mondaymonitor)
- [Walkthrough Reference](https://medium.com/@adeleyeoluwaseyiadedoyin/tryhackme-monday-monitor-walkthrough-49f6fac05d49)
