# Web Server Log Investigation

## Overview

This lab demonstrates a practical SOC-style investigation of Apache web server access logs.

The objective was to identify suspicious HTTP activity, analyze request patterns, distinguish reconnaissance from normal traffic, investigate unusual HTTP responses, and correlate application-layer logs with network traffic.

The investigation was performed in a controlled home-lab environment.

---

## Investigation Scenario

Web server logs contained multiple requests targeting administrative and potentially sensitive application paths.

The investigation focused on determining:

- Which client generated the requests
- Which resources were targeted
- Whether the activity followed a reconnaissance pattern
- Which HTTP response codes were returned
- Whether requests appeared automated
- Whether User-Agent information could be trusted
- Whether the available evidence demonstrated an actual compromise

---

## Investigation Workflow

### 1. Apache Access Log Analysis

Apache access logs were reviewed to understand normal request structure and identify client requests, requested resources, HTTP status codes, and User-Agent information.

### 2. Suspicious Request Identification

Requests targeting paths such as:

- `/admin`
- `/login`
- `/dashboard`
- `/config`
- `/backup`

were identified and investigated.

Multiple unsuccessful requests and repeated probing of different paths were treated as potential reconnaissance indicators.

### 3. HTTP Status Analysis

HTTP response codes were filtered and analyzed.

Particular attention was given to:

- `200` — Successful request
- `404` — Resource not found
- `408` — Request timeout

An unusual log entry was investigated and identified as an HTTP 408 Request Timeout rather than being assumed malicious.

### 4. Request Frequency and Timeline Analysis

Command-line log-processing tools were used to count requested paths and correlate suspicious requests by timestamp.

This helped identify repeated probing occurring within a short time window.

### 5. Automated Traffic Simulation

Controlled `curl` requests were generated to create repetitive traffic and compare automated request patterns with normal browser activity.

### 6. User-Agent Spoofing Investigation

Browser-like User-Agent strings were intentionally spoofed using `curl`.

This demonstrated that User-Agent information alone is not reliable evidence of the originating client application.

### 7. Network-Level Correlation

`tcpdump` was used to inspect HTTP request headers and compare browser-generated traffic with spoofed command-line requests.

Application logs and network-level observations were correlated before reaching an analyst conclusion.

---

## Key Tools and Techniques

- Apache HTTP Server access logs
- Linux command-line utilities
- `grep`
- `awk`
- `sort`
- `uniq`
- `wc`
- `curl`
- `tcpdump`
- HTTP status-code analysis
- Timeline analysis
- User-Agent analysis
- Log and network evidence correlation

---

## Analyst Findings

The observed traffic showed behavior consistent with web reconnaissance:

- Multiple administrative and sensitive-looking paths were requested
- Several requests resulted in HTTP 404 responses
- Requests occurred within a short time window
- Automated request patterns could be reproduced using `curl`
- User-Agent strings could be easily spoofed

However, the available evidence did **not** demonstrate successful exploitation or system compromise.

The correct analyst conclusion was therefore to classify the activity as **suspicious reconnaissance requiring further investigation**, rather than claiming a confirmed attack.

---

## Evidence

Sanitized screenshots documenting the investigation are available in:

[`Screenshots/`](Screenshots/)

The evidence set covers the investigation from initial Apache log review through User-Agent spoofing and network-header comparison.

---

## Detailed Investigation Report

A detailed technical investigation report is maintained in:

[`Documentation/Investigation-Report.md`](Documentation/Investigation-Report.md)

---

## Skills Demonstrated

- Web server log analysis
- SOC-style investigation
- Suspicious activity identification
- HTTP request and status-code analysis
- Linux log filtering and correlation
- Timeline reconstruction
- Automated traffic analysis
- User-Agent spoofing awareness
- Network traffic inspection
- Evidence-based security analysis
- Avoiding unsupported conclusions and false positives

---

## Outcome

The lab demonstrated a structured investigation workflow combining web server logs, Linux command-line analysis, controlled traffic generation, and network-level evidence.

Most importantly, the investigation distinguished **suspicious behavior from confirmed compromise**, demonstrating evidence-driven SOC analyst reasoning.


---

## Repository Information

- **Repository:** Cybersecurity-SOC-Lab
- **Section:** 01-Web-Server-Log-Investigation
- **Lab:** Web Server Log Investigation
- **Documentation:** `01-Web-Server-Log-Investigation/Documentation/Investigation-Report.md`
- **Screenshots:** `01-Web-Server-Log-Investigation/Screenshots/`
- **Status:** Completed
- **Environment:** Controlled Home Lab
