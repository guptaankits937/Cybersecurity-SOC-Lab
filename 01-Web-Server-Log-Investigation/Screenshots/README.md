# Evidence Screenshots

This directory contains sanitized evidence captured during the Web Server Log Investigation lab.

The screenshots document the investigation workflow from initial Apache access-log analysis through reconnaissance detection, HTTP status-code investigation, automated request analysis, and User-Agent/header correlation.

## Evidence Index

| # | Screenshot | Evidence |
|---|---|---|
| 01 | `01-apache-access-log-client-http-request.png` | Apache access log showing client HTTP requests and response status codes. |
| 02 | `02-suspicious-admin-login-requests.png` | Suspicious requests targeting administrative and login-related paths. |
| 03 | `03-reconnaissance-directory-enumeration.png` | Multiple requests to potentially sensitive paths indicating reconnaissance or directory enumeration behavior. |
| 04 | `04-filter-client-404-requests.png` | Filtering of HTTP 404 responses to identify unsuccessful requests. |
| 05 | `05-filter-specific-client-404-requests.png` | Correlation of HTTP 404 responses with a specific client during the investigation. |
| 06 | `06-request-path-frequency-analysis.png` | Frequency analysis of requested paths using command-line log-processing utilities. |
| 07 | `07-http-408-timeout-log-investigation.png` | Investigation of an unusual log entry identified as an HTTP 408 Request Timeout. |
| 08 | `08-suspicious-request-timeline-analysis.png` | Timeline analysis showing multiple suspicious requests occurring within a short time window. |
| 09 | `09-automated-curl-request-pattern.png` | Repetitive requests generated with curl to demonstrate an automated request pattern. |
| 10 | `10-user-agent-spoofing-analysis.png` | Demonstration that a command-line client can spoof a browser-like User-Agent string. |
| 11 | `11-browser-vs-spoofed-user-agent-header-comparison.png` | Packet/header comparison used to distinguish normal browser traffic from spoofed command-line traffic. |

## Evidence Handling

IP addresses visible in the public evidence have been sanitized to avoid exposing lab network details.

Original evidence is retained locally and is not included in this public repository.
