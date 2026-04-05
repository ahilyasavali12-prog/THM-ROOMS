# 📊 SharePoint Online Monitoring — TryHackMe Walkthrough

**Room:** [SharePoint Online Monitoring](https://tryhackme.com/room/sharepointonlinemonitoring)
**Difficulty:** Medium
**Category:** Blue Team / SIEM / Splunk
**Tags:** Splunk, SharePoint, Microsoft 365, Log Analysis, Threat Detection

---

## 📋 Overview

This room simulates a SOC investigation of **SharePoint Online** activity using **Splunk**. You'll hunt through Microsoft 365 audit logs to detect suspicious file access, exfiltration, and insider threat activity.

---

## 🔧 Tools Used

- **Splunk** — SIEM platform for log analysis
- Microsoft 365 Audit Logs — Source data

---

## 🌐 Step 1: Access Splunk

Navigate to `http://<TARGET_IP>:8000` and log in. Go to Search & Reporting.

---

## 🔍 Step 2: Explore the Data

Identify what's in the index:

```splunk
index=* | stats count by sourcetype
index=* sourcetype="*sharepoint*" | head 10
```

---

## 📋 Step 3: Investigate SharePoint Activity

**Find all file access events:**
```splunk
index=* Operation="FileAccessed" | stats count by UserId, ObjectId | sort -count
```

**Find file downloads:**
```splunk
index=* Operation="FileDownloaded" | table _time, UserId, ObjectId, ClientIP
```

**Find external sharing:**
```splunk
index=* Operation="SharingSet" | table _time, UserId, TargetUserOrGroupName, ObjectId
```

**Detect mass downloads (potential exfiltration):**
```splunk
index=* Operation="FileDownloaded" 
| stats count by UserId 
| where count > 50 
| sort -count
```

**Find anonymous link creation:**
```splunk
index=* Operation="AnonymousLinkCreated" | table _time, UserId, ObjectId
```

---

## 🚨 Step 4: Identify the Threat

Based on your queries, identify:
- Which user performed suspicious actions?
- What files were accessed or exfiltrated?
- Was data shared externally?
- What was the timeline of events?

---

## 🚩 Answer the Questions

Use the Splunk queries above to answer each room question, which typically asks about:
- Specific usernames involved
- File names accessed
- IP addresses used
- Number of events

> 🚩 **Flags:** Derived from specific log values found in Splunk queries.

---

## 📝 Key Takeaways

- Microsoft 365 Unified Audit Logging must be enabled to capture SharePoint activity — it's not on by default in all tenants.
- Key SharePoint operations to monitor: `FileAccessed`, `FileDownloaded`, `SharingSet`, `AnonymousLinkCreated`.
- Insider threats often look like normal activity — volume and timing anomalies reveal them.
- Splunk's `stats`, `table`, `where`, and `sort` commands are essential for log analysis.

---

## 🔗 References

- [Microsoft — SharePoint Audit Events](https://docs.microsoft.com/en-us/microsoft-365/compliance/search-the-audit-log-in-security-and-compliance)
- [Walkthrough Reference](https://josepraveen.medium.com/sharepoint-online-monitoring-tryhackme-splunk-a13bcb87cd9d)
