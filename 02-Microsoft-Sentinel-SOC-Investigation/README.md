# Microsoft Sentinel SOC Investigation

## Overview

This project demonstrates a hands-on Microsoft Sentinel SOC investigation using Windows Security Event logs collected from a Windows endpoint.

The investigation focused on security-event collection, KQL-based log analysis, controlled failed-login simulation, Windows Event ID `4625` investigation, authentication-event correlation, and analyst assessment.

The activity was performed in a controlled lab environment and represents hands-on portfolio experience rather than a real production incident.

---

## Scenario

A Windows endpoint was connected to Azure and configured to send Windows Security Event logs to a Log Analytics workspace.

Two controlled failed Windows login attempts were generated to create authentication-failure events for investigation.

The objective was to verify event collection, identify failed logons, investigate Event ID `4625`, analyse authentication details, and confirm the number of failed attempts using KQL.

---

## Environment

| Component               | Configuration                                     |
| ----------------------- | ------------------------------------------------- |
| Resource Group          | `RG-Sentinel-Lab`                                 |
| Log Analytics Workspace | `LAW-Sentinel-SOC-Lab`                            |
| Region                  | Sweden Central                                    |
| Windows Endpoint        | `DESKTOP-SBLJ6VF`                                 |
| Endpoint Connection     | Azure Arc                                         |
| Monitoring Agent        | Azure Monitor Agent                               |
| Data Collection Rule    | `DCR-Windows-Security-Events`                     |
| Collected Events        | Security Audit Success and Security Audit Failure |

---

## Tasks Completed

* Deployed and configured a Log Analytics workspace
* Activated Microsoft Sentinel for the lab workspace
* Installed the Windows Security Events solution
* Onboarded the Windows endpoint through Azure Arc
* Connected the endpoint using Azure Monitor Agent
* Configured a Data Collection Rule for Windows Security Events
* Verified Windows Security Event ingestion using KQL
* Generated two controlled failed Windows login attempts
* Identified Windows Event ID `4625`
* Investigated authentication and failure details
* Counted failed authentication attempts using KQL
* Performed analyst assessment and false-positive consideration
* Documented investigation findings and supporting evidence

---

## Key Queries

### Verify Windows Security Events

```kusto
Event
| where EventLog == "Security"
```

The configured Windows Event Logs data source delivered the relevant security events to the `Event` table.

### Identify Failed Logon Events

```kusto
Event
| where EventLog == "Security"
| where EventID == 4625
| where TimeGenerated > ago(30m)
| project TimeGenerated, Computer, EventID, Source, RenderedDescription
| order by TimeGenerated desc
```

This query identified exactly two Event ID `4625` records generated during the controlled failed-login test.

### Count Failed Authentication Attempts

```kusto
Event
| where EventLog == "Security"
| where EventID == 4625
| where TimeGenerated > ago(1h)
| summarize FailedAttempts=count() by Computer
| order by FailedAttempts desc
```

Result:

```text
Computer: DESKTOP-SBLJ6VF
FailedAttempts: 2
```

---

## Investigation Summary

Detailed review of Event ID `4625` showed:

* **Logon Type:** `2`
* **Source Network Address:** `127.0.0.1`
* **Source Port:** `0`
* **Logon Process:** `User32`
* **Authentication Package:** `Negotiate`
* **Caller Process:** `C:\Windows\System32\svchost.exe`
* **Status:** `0xC000006D`
* **Sub Status:** `0xC0000380`

Logon Type `2` represents an interactive/local logon.

The two failed authentication events matched the intentionally generated lab activity. No evidence was identified that justified classifying the events as malicious.

---

## Analytics Rule / Incident Status

A Scheduled Analytics Rule was planned as the next stage of the investigation.

During configuration, Microsoft Sentinel redirected to Microsoft Defender, where the workflow remained on the SIEM Workspaces page instead of opening the expected analytics-rule creation workflow.

Therefore:

* Scheduled Analytics Rule creation is **not completed**
* Sentinel incident creation is **not completed**

**Status:** Pending – Portal Redirect Issue

---

## Evidence

Sanitized screenshots documenting the Microsoft Sentinel investigation are available in:

[Screenshots/](Screenshots/)

The evidence set covers the investigation from Log Analytics workspace deployment and Windows endpoint onboarding through Windows Security Event collection, KQL-based Event ID `4625` investigation, and failed-logon count verification.

---

## Detailed Investigation Report

A detailed technical investigation report is maintained in:

[Documentation/Microsoft-Sentinel-SOC-Investigation.md](Documentation/Microsoft-Sentinel-SOC-Investigation.md)

---

## Skills Demonstrated

* Microsoft Sentinel
* Log Analytics
* Azure Arc
* Azure Monitor Agent
* Windows Security Event analysis
* Kusto Query Language (KQL)
* Event ID `4625` investigation
* Authentication-event analysis
* Log filtering and aggregation
* SOC investigation methodology
* False-positive assessment
* Security investigation documentation

---

## Outcome

Windows Security Events were successfully collected and analysed using KQL.

Two controlled failed login attempts generated two Event ID `4625` records. The events were investigated and correlated with the controlled test activity and were therefore not classified as malicious.

The security-event collection and KQL investigation stages are complete.

The Scheduled Analytics Rule and Sentinel incident workflow remain pending due to the Microsoft Sentinel / Microsoft Defender portal redirect issue.

---

## Repository Information

* **Repository:** `Cybersecurity-SOC-Lab`
* **Section:** `02-Microsoft-Sentinel-SOC-Investigation`
* **Lab:** Microsoft Sentinel SOC Investigation
* **Documentation:** `Documentation/Microsoft-Sentinel-SOC-Investigation.md`
* **Screenshots:** `Screenshots/`
* **Status:** Investigation Completed – Analytics Rule Pending Due to Portal Redirect Issue
