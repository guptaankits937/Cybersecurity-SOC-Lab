## Evidence / Screenshots

The following screenshots document the complete Microsoft Sentinel SOC investigation workflow.

| # | Evidence | Screenshot |
|---|---|---|
| 01 | Log Analytics Workspace deployment completed successfully | [View Screenshot](Screenshots/01-Log-Analytics-Workspace-Deployment-Success.png) |
| 02 | Log Analytics Workspace overview and configuration | [View Screenshot](Screenshots/02-Log-Analytics-Workspace-Overview.png) |
| 03 | Microsoft Sentinel free trial activated | [View Screenshot](Screenshots/03-Sentinel-Free-Trial-Activated.png) |
| 04 | Windows Security Events solution installed | [View Screenshot](Screenshots/04-Windows-Security-Events-Solution-Installed.png) |
| 05 | Windows endpoint successfully onboarded through Azure Arc | [View Screenshot](Screenshots/05-Azure-Arc-Onboarding-Successful.png) |
| 06 | Azure Arc machine connected successfully | [View Screenshot](Screenshots/06-Azure-Arc-Machine-Connected.png) |
| 07 | Data Collection Rule deployment completed | [View Screenshot](Screenshots/07-DCR-Deployment-Complete.png) |
| 08 | Windows Security Events verified using KQL | [View Screenshot](Screenshots/08-Windows-Security-Events-KQL-Verification.png) |
| 09 | Windows failed logon Event ID 4625 identified | [View Screenshot](Screenshots/09-Sentinel-Failed-Logon-Event-4625.png) |
| 10 | Event ID 4625 detailed failed logon investigation | [View Screenshot](Screenshots/10-Sentinel-4625-Failed-Logon-Investigation.png) |
| 11 | KQL query confirming two failed logon attempts | [View Screenshot](Screenshots/11-Sentinel-4625-Failed-Logon-Count.png) |

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
