# Project SecOps — Security Operations Sprint

A comprehensive security operations project documenting the investigation, containment, and remediation of a coordinated cyber attack campaign targeting a fictional organisation. Completed as part of the CyBlack Security Operations Programme.

> **Note:** All organisational details, staff names, email addresses, and log data used in this project are fictional and were provided as part of the CyBlack SecOps Sprint lab environment. The WannaCry malware sample referenced in the malware analysis is a publicly documented threat; no real systems were compromised in this investigation.

---

Read all about it on my medium https://medium.com/@tobibabalola21/how-i-uncovered-a-coordinated-ransomware-campaign-hidden-across-three-separate-security-incidents-32e2e7b0d623

##  What This Project Covers

Over the course of this sprint, three interconnected security incidents were identified and investigated — ultimately assessed as a **single coordinated attack campaign** rather than isolated events:

| Incident | Description |
|---|---|
| Coordinated Phishing Campaign |  5 staff targeted across departments within 90 minutes. 4 confirmed malicious. NjRAT delivery, Google Drive malware, Adobe brand impersonation, display name spoofing. |
| Account Compromise & Data Exfiltration | Financial Analyst credentials compromised. Logins from Bulgarian bulletproof VPS and Chinese Tencent Cloud. Payroll data and internal vulnerability report exfiltrated. |
| WannaCry Ransomware Delivery | WannaCry binary (diskpart.exe) delivered via RAR attachment to VIP user. Email gateway missed it. EDR caught it. 68/72 VirusTotal detections. Joe's Sandbox score: 100/100. |

The exfiltrated vulnerability report from Incident 2 is assessed to have directly informed the attack vector used in Incident 3 — a pattern consistent with **Storm-0978 (RomCom)**, a Russian-linked threat actor known to target finance and government sectors.

---

##  Repository Structure

```
project-secops/
├── README.md
├── reports/
│   ├── active-email-threats-phishing-analysis.pdf
│   ├── suspicious-sign-in-investigation-report.pdf
│   ├── malware-analysis-report.pdf
│   ├── vulnerability-management-report.pdf
│   ├── incidence-response-playbook-report.pdf
│   ├── mobile-device-vulnerability-management-report.pdf
│   ├── sentinel-alerts.pdf
│   └── comprehensive-report-overview.pdf
├── sentinel-rules/
│   ├── rule1-phishing-sender-domain.kql
│   ├── rule2-malicious-domain-c2.kql
│   ├── rule3-foreign-ip-signin.kql
│   ├── rule4-wannacry-hash-process.kql
│   ├── rule5-wannacry-registry-vss.kql
│   ├── rule6-wannacry-c2-tor-smb.kql
│   ├── rule7-account-compromise-behaviour.kql
│   └── rule8-coordinated-phishing-campaign.kql
└── iocs/
    └── master-ioc-list.csv
```

---

##  Reports

### [Phishing Analysis](reports/active-email-threats-phishing-analysis.pdf)
Investigation of 5 reported phishing emails using VirusTotal, URLScan.io, and email header analysis. Confirmed NjRAT delivery, Google Drive malware hosting, Adobe brand impersonation with URL masking, and browser-based attack toolkit. Cross-campaign threat actor correlation identified.

### [Suspicious Sign-In Investigation](reports/suspicious-sign-in-investigation-report.pdf)
Account compromise investigation using Entra ID sign-in logs and activity logs. IP geolocation confirmed logins from Bulgarian bulletproof hosting (VenomDC), Chinese Tencent Cloud (confirmed malicious — HTTP bruteforce history), and Thai ISP. Payroll and vulnerability data exfiltration confirmed.

### [Malware Analysis Report](reports/malware-analysis-report.pdf)
Full static and dynamic analysis of WannaCry ransomware delivered as diskpart.exe inside Catalogue.rar. Static analysis via VirusTotal (68/72 detections). Dynamic analysis via Joe's Sandbox (score 100/100, confidence 100%). Process tree, dropped files, Tor C2 addresses, SMB lateral movement, MITRE ATT&CK mapping documented.

### [Vulnerability Management — Microsoft Patch Tuesday](reports/vulnerability-management-report.pdf)
Assessment of Microsoft's July 2023 Patch Tuesday (132 CVEs, 37 RCEs, 6 actively exploited). Prioritised patch plan, CVE-2023-36884 interim mitigations (no patch available at release), and formal patch management SLA policy.

### [Incident Response Playbooks](reports/incidence-response-playbook-report.pdf)
Three NIST SP 800-61 aligned playbooks: Phishing Email, Account Compromise, and Ransomware/Malware Delivery. Each covers Prepare → Detect & Analyse → Contain, Eradicate & Recover → Post-Incident phases with checklists, role assignments, and communication templates.

### [Mobile Device Vulnerability Management](reports/mobile-device-vulnerability-management-report.pdf)
Risk assessment of 500 Android devices (50% outdated, no MDM). CVE analysis: CVE-2023-2136 (CVSS 9.8), CVE-2021-29256 (CVSS 8.8), CVE-2023-26083 (CISA KEV). MDM solution evaluation including Microsoft Intune recommendation. Device lifecycle policy.

### [Sentinel Alert Rules](reports/sentinel-alerts.pdf)
8 custom Microsoft Sentinel analytics rules derived from sprint IOCs. Covers phishing detection, C2 contact, foreign IP sign-in, WannaCry hash/process/registry/network detection, account compromise behavioural pattern, and coordinated campaign detection.

### [Comprehensive Report](reports/comprehensive-report-overview.pdf)
Full final report covering unified incident timeline, MITRE ATT&CK campaign mapping, threat actor assessment (Storm-0978 hypothesis), cross-incident correlation analysis, detection gap analysis, prioritised controls recommendations, lessons learned, and recovery next steps.

---

## 🔍 Sentinel Detection Rules

8 custom KQL analytics rules deployed in Microsoft Sentinel. Each rule is provided as a standalone `.kql` file for direct import.

| Rule | Severity | Detects |
|---|---|---|
| [rule1-phishing-sender-domain.kql](sentinel-rules/rule1-phishing-sender-domain.kql) | High | Inbound email from confirmed phishing sender domains/addresses |
| [rule2-malicious-domain-c2.kql](sentinel-rules/rule2-malicious-domain-c2.kql) | Critical | Outbound network connection to malicious domains and C2 IPs |
| [rule3-foreign-ip-signin.kql](sentinel-rules/rule3-foreign-ip-signin.kql) | Critical | Entra ID sign-in from known malicious IPs or non-UK geolocation |
| [rule4-wannacry-hash-process.kql](sentinel-rules/rule4-wannacry-hash-process.kql) | Critical | WannaCry file hashes, dropped filenames, active encryption, masquerading |
| [rule5-wannacry-registry-vss.kql](sentinel-rules/rule5-wannacry-registry-vss.kql) | Critical | WannaCry registry persistence and Volume Shadow Copy deletion |
| [rule6-wannacry-c2-tor-smb.kql](sentinel-rules/rule6-wannacry-c2-tor-smb.kql) | Critical | Tor onion C2 communication and EternalBlue SMB lateral movement |
| [rule7-account-compromise-behaviour.kql](sentinel-rules/rule7-account-compromise-behaviour.kql) | Critical | Sensitive file access post foreign login, admin tool abuse, impossible travel |
| [rule8-coordinated-phishing-campaign.kql](sentinel-rules/rule8-coordinated-phishing-campaign.kql) | High | Multi-target phishing campaigns and display name spoofing |

---

##  Tools Used

| Category | Tools |
|---|---|
| Threat Intelligence | VirusTotal, URLScan.io, IPinfo.io, Joe's Sandbox |
| SIEM & Detection | Microsoft Sentinel, KQL |
| Email Analysis | MXToolbox, Email header analysis, .eml inspection |
| Malware Analysis | Joe's Sandbox (dynamic), VirusTotal (static) |
| Identity & Access | Microsoft Entra ID, Azure Active Directory |
| Cloud Security | Microsoft Azure, Microsoft Defender for Endpoint |
| Vulnerability Management | MSRC, CISA KEV, NVD, Android Security Bulletin |

---

##  Skills Demonstrated

- **Phishing Investigation** — email header analysis, URL/domain reputation, IOC extraction, campaign correlation
- **Account Compromise Investigation** — sign-in log analysis, IP geolocation, activity log correlation, data exfiltration detection
- **Malware Analysis** — static analysis (hashes, PE metadata), dynamic sandbox analysis, process tree reading, MITRE ATT&CK mapping
- **Vulnerability Management** — CVE prioritisation, patch planning, risk assessment, long-term strategy
- **Detection Engineering** — KQL query writing, Sentinel analytics rule creation, entity mapping, alert tuning
- **Incident Response** — NIST SP 800-61 framework, playbook development, stakeholder communication
- **Threat Intelligence** — IOC management, threat actor assessment, campaign attribution

---

##  IOC Reference

All indicators of compromise identified across this project are here in [`iocs/master-ioc-list.csv`](iocs/master-ioc-list.csv).

Categories: Email senders, malicious domains, IP addresses, file hashes, Tor C2 addresses, registry keys, filenames.

---

## 🔗 Related Projects

- [Project Citadel](https://github.com/blvckbillgates/project-citadel) — Azure secure environment build (Entra ID, Sentinel, Defender for Cloud)

---


