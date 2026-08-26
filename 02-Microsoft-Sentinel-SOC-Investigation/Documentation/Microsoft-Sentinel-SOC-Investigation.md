# Microsoft Sentinel SOC Investigation

## Incident Summary

This lab documents a Microsoft Sentinel SOC investigation using Windows Security Event logs collected from a Windows endpoint through Azure Arc and Azure Monitor Agent.

The investigation focused on validating Windows Security log ingestion, identifying failed authentication attempts, analysing Windows Event ID 4625, and using KQL to confirm and investigate the activity.

Two controlled failed login attempts were generated on the Windows endpoint and successfully identified in Microsoft Sentinel.

---

## Environment

- Microsoft Azure
- Microsoft Sentinel
- Log Analytics Workspace: `LAW-Sentinel-SOC-Lab`
- Resource Group: `RG-Sentinel-Lab`
- Region: Sweden Central
- Windows Endpoint: `DESKTOP-SBLJ6VF`
- Azure Arc
- Azure Monitor Agent
- Data Collection Rule: `DCR-Windows-Security-Events`
- Windows Security Event Logs
- Kusto Query Language (KQL)

---

## Investigation Objectives

The objectives of this investigation were to:

- Deploy and configure a Microsoft Sentinel lab environment.
- Connect a Windows endpoint to Azure using Azure Arc.
- Configure Windows Security Event collection.
- Verify successful log ingestion into Log Analytics.
- Generate controlled failed authentication attempts.
- Identify Windows Event ID 4625.
- Analyse failed logon event details.
- Use KQL to count and correlate failed authentication attempts.
- Attempt to create a Sentinel analytics rule for detection.

---

## Initial Log Review

After connecting the Windows endpoint through Azure Arc and configuring the Azure Monitor Agent, log collection was validated through the Log Analytics workspace.

The Azure Monitor Agent heartbeat confirmed that the endpoint was communicating successfully with Azure.

Windows Security events were then reviewed to confirm that security logs were being collected from the endpoint.

---

## Security Event Collection Verification

The Data Collection Rule was configured to collect:

- Security Audit Success events
- Security Audit Failure events

The destination for the collected events was:

`LAW-Sentinel-SOC-Lab`

An initial query against the `SecurityEvent` table did not return the expected results.

Further investigation showed that the Windows Security Event data collected through the configured Windows Event Logs data source was available in the `Event` table.

Example verification:

```kusto
Event
| where EventLog == "Security"
```

Security events were successfully returned, confirming that log ingestion was working.

---

## Failed Logon Event Identification

To generate test authentication activity, two controlled failed Windows login attempts were performed on the endpoint.

The following KQL query was used to search for Windows failed logon Event ID 4625:

```kusto
Event
| where EventLog == "Security"
| where EventID == 4625
| where TimeGenerated > ago(30m)
| project TimeGenerated, Computer, EventID, Source, RenderedDescription
| order by TimeGenerated desc
```

The query successfully returned two Event ID 4625 records.

---

## Event 4625 Investigation

Windows Event ID `4625` represents a failed account logon attempt.

The collected event details included:

- Event ID: `4625`
- Logon Type: `2`
- Source Network Address: `127.0.0.1`
- Source Port: `0`
- Logon Process: `User32`
- Authentication Package: `Negotiate`
- Caller Process: `C:\Windows\System32\svchost.exe`
- Failure Reason: `An Error occurred during Logon`
- Status: `0xC000006D`
- Sub Status: `0xC0000380`

Logon Type 2 indicated an interactive/local authentication attempt.

The event details were reviewed to understand the source, authentication process, failure information, and affected endpoint.

---

## Failed Logon Frequency Analysis

The following KQL query was used to count failed authentication attempts by computer:

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

This confirmed that both controlled failed login attempts were successfully captured by the monitoring environment.

---

## Timeline Analysis

The events were ordered by `TimeGenerated` to review the sequence of authentication failures.

This allowed the analyst to determine:

- When the failed authentication attempts occurred.
- How many attempts were recorded.
- Which endpoint generated the events.
- Whether multiple authentication failures occurred within the investigated period.

---

## Controlled Authentication Failure Simulation

The authentication failures used in this investigation were intentionally generated in the lab environment.

This provided a known test condition that could be compared with the collected Windows Security events.

The controlled activity helped validate:

- Endpoint connectivity.
- Security log collection.
- Data ingestion.
- KQL filtering.
- Event correlation.
- Failed authentication detection.

---

## KQL Query Investigation

Kusto Query Language was used throughout the investigation to filter and analyse collected security events.

The investigation used techniques including:

- Filtering by event log.
- Filtering by Event ID.
- Filtering by time range.
- Selecting relevant fields.
- Ordering events chronologically.
- Aggregating events using `summarize`.
- Counting failed authentication attempts.

Example:

```kusto
Event
| where EventLog == "Security"
| where EventID == 4625
| where TimeGenerated > ago(1h)
| summarize FailedAttempts=count() by Computer
| order by FailedAttempts desc
```

---

## Security Event Detail Analysis

The Event ID 4625 records were expanded and reviewed individually.

The investigation focused on fields that could help determine the context of a failed authentication attempt, including:

- Computer
- Event ID
- Authentication package
- Logon type
- Source network address
- Source port
- Logon process
- Caller process
- Failure reason
- Status
- Sub status

These fields would be useful during a real SOC investigation when determining whether failed authentication activity represents normal user behaviour, incorrect credentials, automated activity, or potentially suspicious authentication attempts.

---

## Analytics Rule / Incident Creation

An attempt was made to continue the investigation by creating a Microsoft Sentinel scheduled analytics rule.

The Microsoft Sentinel Analytics page redirected the workflow to Microsoft Defender.

The Defender portal then redirected to the SIEM Workspaces configuration page instead of providing the expected analytics-rule creation workflow.

Because of this portal behaviour, the scheduled analytics rule and Sentinel incident creation were not completed during this phase of the lab.

### Status

**Pending – Portal Redirect Issue**

The analytics rule and incident workflow can be completed later when the expected rule-creation interface becomes available.

---

## Key Commands and Techniques

### Verify Windows Security Events

```kusto
Event
| where EventLog == "Security"
```

### Identify Failed Logons

```kusto
Event
| where EventLog == "Security"
| where EventID == 4625
| where TimeGenerated > ago(30m)
| project TimeGenerated, Computer, EventID, Source, RenderedDescription
| order by TimeGenerated desc
```

### Count Failed Login Attempts

```kusto
Event
| where EventLog == "Security"
| where EventID == 4625
| where TimeGenerated > ago(1h)
| summarize FailedAttempts=count() by Computer
| order by FailedAttempts desc
```

---

## Key Findings

- The Windows endpoint was successfully connected through Azure Arc.
- Azure Monitor Agent communication was successfully validated.
- Windows Security Event logs were successfully collected.
- Security events were available in the `Event` table.
- Two controlled failed authentication attempts were generated.
- Both attempts produced Windows Event ID 4625.
- KQL successfully identified and counted both failed authentication events.
- Event details provided authentication context including logon type, source address, authentication package, and failure status.
- Scheduled analytics-rule creation could not be completed because of the Microsoft Sentinel/Defender portal redirect behaviour.

---

## Analyst Assessment

The investigation confirmed that the Windows endpoint was successfully sending Security Event data into the Microsoft Sentinel environment.

The two Event ID 4625 records corresponded with the intentionally generated failed login attempts.

No conclusion of malicious activity was made because the authentication failures were generated as controlled lab activity.

The investigation demonstrated the basic SOC workflow of:

**Generate Activity → Collect Logs → Search Events → Analyse Evidence → Correlate Activity → Document Findings**

---

## False-Positive Considerations

A failed authentication event does not automatically indicate malicious activity.

Event ID 4625 may occur because of:

- Incorrect user credentials.
- Accidental password entry.
- Application or service authentication issues.
- Configuration problems.
- Automated activity.
- Potential password attacks.

Additional context should therefore be reviewed before classifying failed authentication activity as malicious.

Useful contextual information includes:

- Number of attempts.
- Source IP address.
- Target account.
- Time pattern.
- Affected systems.
- Authentication type.
- Related successful logins.
- Other security events occurring around the same time.

---

## Recommended Next Steps

Future investigation steps include:

1. Create a Microsoft Sentinel scheduled analytics rule for repeated failed authentication attempts.
2. Generate activity that triggers the detection rule.
3. Create and review a Sentinel security incident.
4. Investigate related entities and authentication events.
5. Correlate failed and successful authentication activity.
6. Document incident triage and analyst conclusions.
7. Close the incident with appropriate classification.

These steps remain pending because of the current Sentinel/Defender portal redirect issue.

---

## Final Conclusion

The Microsoft Sentinel SOC lab successfully demonstrated Windows Security Event collection and failed authentication investigation.

A Windows endpoint was connected using Azure Arc and Azure Monitor Agent, security events were collected into the Log Analytics workspace, and KQL was used to investigate Event ID 4625.

Two controlled failed login attempts were successfully identified and correlated.

The analytics-rule and incident-generation phase remains pending because of the Microsoft Sentinel to Microsoft Defender portal redirect issue.

The completed work demonstrates practical experience with security log collection, KQL investigation, Windows authentication events, event correlation, and SOC-style analysis.

---

## Evidence References

Evidence for this investigation is stored in:

`02-Microsoft-Sentinel-SOC-Investigation/Screenshots/`

The evidence set contains screenshots numbered `01` through `11`, covering:

- Log Analytics Workspace deployment
- Log Analytics Workspace overview
- Microsoft Sentinel activation
- Windows Security Events solution installation
- Azure Arc onboarding
- Azure Arc machine connection
- Data Collection Rule deployment
- Windows Security Event ingestion verification
- Event ID 4625 identification
- Event ID 4625 detailed investigation
- Failed logon count verification

---

## Repository Information

**Repository:** Cybersecurity-SOC-Lab  
**Section:** 02-Microsoft-Sentinel-SOC-Investigation  
**Lab:** Microsoft Sentinel SOC Investigation  
**Documentation:** `02-Microsoft-Sentinel-SOC-Investigation/Documentation/Microsoft-Sentinel-SOC-Investigation.md`  
**Screenshots:** `02-Microsoft-Sentinel-SOC-Investigation/Screenshots/`  
**Status:** Investigation Completed – Analytics Rule Pending Due to Portal Redirect Issue  
**Version:** 1.0
