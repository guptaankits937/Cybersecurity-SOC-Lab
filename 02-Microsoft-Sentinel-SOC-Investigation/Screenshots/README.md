# Evidence Screenshots

This directory contains sanitized evidence captured during the Microsoft Sentinel SOC Investigation lab.

The screenshots document the investigation workflow from Microsoft Sentinel and Log Analytics deployment through Windows endpoint onboarding, security-event collection, KQL analysis, and failed-logon investigation.

## Evidence Index

| # | Screenshot | Evidence |
|---|---|---|
| 01 | `01-Log-Analytics-Workspace-Deployment-Success.png` | Successful deployment of the Log Analytics Workspace used for the Microsoft Sentinel lab. |
| 02 | `02-Log-Analytics-Workspace-Overview.png` | Log Analytics Workspace overview showing the configured Sentinel lab environment. |
| 03 | `03-Sentinel-Free-Trial-Activated.png` | Microsoft Sentinel free trial successfully activated for the Log Analytics Workspace. |
| 04 | `04-Windows-Security-Events-Solution-Installed.png` | Windows Security Events solution successfully installed for security-log collection. |
| 05 | `05-Azure-Arc-Onboarding-Successful.png` | Successful Azure Arc onboarding of the Windows endpoint. |
| 06 | `06-Azure-Arc-Machine-Connected.png` | Azure Arc machine showing Connected status after endpoint onboarding. |
| 07 | `07-DCR-Deployment-Success.png` | Successful deployment of the Data Collection Rule for Windows Security Events. |
| 08 | `08-Windows-Security-Events-KQL-Verification.png` | KQL verification confirming Windows Security Events were being collected in the Log Analytics Workspace. |
| 09 | `09-Sentinel-Failed-Logon-Event-4625.png` | Windows Event ID 4625 identified after generating controlled failed logon attempts. |
| 10 | `10-Sentinel-4625-Failed-Logon-Investigation.png` | Detailed investigation of Event ID 4625 including authentication and failed-logon information. |
| 11 | `11-Sentinel-4625-Failed-Logon-Count.png` | KQL aggregation confirming two failed logon attempts on the monitored Windows endpoint. |

### Evidence Summary

The screenshots demonstrate:

- Microsoft Sentinel and Log Analytics workspace deployment.
- Windows endpoint onboarding through Azure Arc.
- Windows Security Event collection configuration.
- Successful security log ingestion.
- KQL-based investigation of Windows Event ID `4625`.
- Detailed failed authentication event analysis.
- Aggregation and verification of two controlled failed login attempts.

> **Note:** Scheduled Analytics Rule and Sentinel incident creation remain pending due to the Microsoft Sentinel / Microsoft Defender portal redirect issue.
