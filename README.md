# Cybersecurity SOC Lab

## Overview

This repository contains hands-on cybersecurity and SOC investigation labs built in a controlled home-lab environment.

The purpose of this portfolio is to demonstrate practical security investigation skills, evidence-based analysis, log correlation, incident triage, and technical documentation.

The labs focus on understanding what the available evidence actually demonstrates and avoiding unsupported conclusions when investigating suspicious activity.

---

## Current Labs

### 01 — Web Server Log Investigation

SOC-style investigation of suspicious HTTP activity using Apache access logs and network-level evidence.

The investigation includes:

- Apache access-log analysis
- Suspicious path and reconnaissance identification
- HTTP 404 and 408 investigation
- Request-frequency and timeline analysis
- Automated traffic simulation using `curl`
- User-Agent spoofing analysis
- HTTP header inspection using `tcpdump`
- Evidence correlation
- Analyst assessment and false-positive considerations

**Investigation Result:** Suspicious reconnaissance identified; no confirmed system compromise.

➡️ [View Web Server Log Investigation](01-Web-Server-Log-Investigation/)

---

## Investigation Approach

The labs in this repository follow a structured investigation methodology:

1. Identify the suspicious activity.
2. Collect and preserve relevant evidence.
3. Establish a timeline.
4. Analyze logs and observable behavior.
5. Correlate multiple evidence sources.
6. Test investigative hypotheses where appropriate.
7. Consider legitimate or false-positive explanations.
8. Document findings based on evidence.
9. Avoid claiming compromise without supporting evidence.

---

## Technologies and Tools

Current and planned lab work may include:

- Linux
- Apache HTTP Server
- Windows Event Logs
- Bash
- PowerShell
- `grep`, `awk`, `sort`, `uniq`
- `curl`
- `tcpdump`
- Network and HTTP analysis
- Security event investigation
- SOC alert triage
- Microsoft Sentinel and KQL

> Tools listed as part of future labs will only be presented as hands-on skills after the corresponding practical work has been completed.

---

## Portfolio Direction

This repository is designed to progressively cover:

- Web and application log investigation
- Linux security investigation
- Windows Event investigation
- Authentication activity analysis
- Process and PowerShell investigation
- SOC alert triage
- Incident timeline reconstruction
- Evidence correlation
- SIEM investigation workflows
- Microsoft Sentinel and KQL
- Digital forensics and SOC investigation integration

---

## Repository Structure

```text
Cybersecurity-SOC-Lab/
│
├── 01-Web-Server-Log-Investigation/
│   ├── README.md
│   ├── Documentation/
│   │   └── Investigation-Report.md
│   └── Screenshots/
│       ├── README.md
│       └── Sanitized investigation evidence
│
└── README.md
```

---

## Evidence and Documentation

Each completed lab is designed to contain:

- A recruiter-friendly main README
- Detailed technical investigation documentation
- Sanitized evidence screenshots
- An evidence index

Sensitive information and unnecessary lab-network details are excluded from public evidence.

---

## Portfolio Note

These projects represent hands-on home-lab and portfolio work.

They are intended to demonstrate practical investigation methodology, technical troubleshooting, evidence analysis, and security reasoning. They do not represent paid production SOC experience.
