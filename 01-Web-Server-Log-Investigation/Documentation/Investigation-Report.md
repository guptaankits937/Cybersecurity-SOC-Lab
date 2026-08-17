# Web Server Log Investigation — Investigation Report

## 1. Incident Summary

A controlled SOC-style investigation was performed against Apache web server access logs after multiple requests were observed targeting administrative and potentially sensitive web paths.

The investigation focused on determining whether the activity represented normal browsing, automated probing, reconnaissance, or evidence of a successful compromise.

The observed activity was classified as suspicious reconnaissance. No evidence of successful exploitation or system compromise was identified during the investigation.

---

## 2. Environment

- **Environment:** Controlled Home Lab
- **Web Server:** Apache HTTP Server
- **Server Platform:** Ubuntu Linux
- **Traffic Sources:** Web browser and controlled command-line clients
- **Primary Log Source:** Apache access log
- **Network Analysis:** tcpdump
- **Traffic Generation:** curl

IP addresses in public evidence have been sanitized.

---

## 3. Investigation Objectives

The investigation was designed to answer the following questions:

1. Which client generated the suspicious requests?
2. Which web resources were targeted?
3. What HTTP response codes were returned?
4. Did the requests indicate reconnaissance or directory enumeration?
5. Did the activity occur within a meaningful time window?
6. Could similar traffic be generated automatically?
7. Could User-Agent information be manipulated?
8. Did the available evidence indicate a successful compromise?

---

## 4. Initial Log Review

Apache access logs were reviewed to establish the structure of normal HTTP requests and identify useful investigation fields.

The analysis focused on:

- Client source
- Timestamp
- HTTP method
- Requested resource
- HTTP response status
- User-Agent information

This provided the baseline required for further filtering and correlation.

---

## 5. Suspicious Request Identification

Multiple requests were identified targeting administrative and potentially sensitive paths, including:

- `/admin`
- `/login`
- `/dashboard`
- `/config`
- `/backup`

The requests originated from the same client during the investigated activity.

The combination of multiple targeted paths and unsuccessful requests was considered consistent with reconnaissance or directory-enumeration behavior.

At this stage, the activity was treated as suspicious rather than immediately classified as malicious.

---

## 6. HTTP Status Code Analysis

HTTP response codes were analyzed to understand the outcome of the observed requests.

The investigation included:

- **HTTP 200** — Successful request
- **HTTP 404** — Requested resource not found
- **HTTP 408** — Request timeout

Multiple HTTP 404 responses were observed during probing of different paths.

These responses demonstrated unsuccessful attempts to locate resources, but HTTP 404 responses alone were not considered proof of malicious activity.

---

## 7. HTTP 408 Investigation

During log analysis, an unusual entry initially appeared different from the normal request records.

Rather than assuming the entry was malicious or malformed, the raw Apache log was reviewed.

The entry was identified as:

**HTTP 408 — Request Timeout**

This demonstrated the importance of validating unusual log entries before assigning a security interpretation.

The 408 response was treated as an anomaly requiring explanation, not as evidence of compromise.

---

## 8. Request Frequency Analysis

Linux command-line tools were used to analyze the frequency of requested paths.

The workflow included utilities such as:

- `grep`
- `awk`
- `sort`
- `uniq`
- `wc`

This analysis helped identify repeated requests and understand which resources were being targeted most frequently.

Frequency analysis was used as supporting evidence rather than as a standalone indicator of malicious behavior.

---

## 9. Timeline Analysis

Suspicious requests were correlated by timestamp.

Multiple requests targeting different paths occurred within a short period.

The timing, combined with repeated probing of administrative and sensitive-looking paths, strengthened the hypothesis that the activity represented reconnaissance.

However, timing alone was not considered sufficient to confirm an attack.

---

## 10. Automated Traffic Simulation

Controlled HTTP requests were generated using `curl`.

Repeated requests were intentionally produced to create a recognizable automated traffic pattern in the Apache access log.

This allowed comparison between:

- Normal browser-generated requests
- Repetitive command-line requests
- Previously observed suspicious request patterns

The exercise demonstrated how automation can create consistent and repetitive patterns in server logs.

---

## 11. User-Agent Spoofing Investigation

The investigation then examined whether User-Agent information could reliably identify the client application.

Using `curl`, browser-like User-Agent strings were intentionally supplied to the server.

The Apache access log recorded the supplied browser-style User-Agent.

This demonstrated that:

**User-Agent strings are client-controlled and can be spoofed.**

Therefore, a browser-looking User-Agent should not be treated as proof that the request originated from a real browser.

---

## 12. Network-Level Correlation

`tcpdump` was used to inspect HTTP request headers at the network level.

Browser-generated traffic was compared with controlled command-line traffic using spoofed headers.

This provided an additional evidence source beyond the Apache access log and demonstrated the value of correlating:

- Application logs
- HTTP request headers
- Network traffic
- Request behavior

No single field was treated as definitive evidence by itself.

---

## 13. Key Commands and Techniques

The investigation used commands and techniques including:

```bash
tail
grep
grep -E
awk
sort
uniq
wc
curl
tcpdump
```

These were used for:

- Access-log review
- Status-code filtering
- Client correlation
- Request-path extraction
- Frequency analysis
- Timeline analysis
- Controlled traffic generation
- User-Agent testing
- HTTP header inspection

---

## 14. Key Findings

The investigation identified the following:

1. Multiple administrative and potentially sensitive paths were requested.
2. Several requests resulted in HTTP 404 responses.
3. Requests occurred within a relatively short time window.
4. The observed behavior was consistent with reconnaissance or directory enumeration.
5. Repetitive automated traffic could be reproduced using `curl`.
6. User-Agent strings could be intentionally spoofed.
7. HTTP 408 entries required contextual investigation rather than immediate security classification.
8. Application logs and network evidence provided stronger context when analyzed together.

---

## 15. Analyst Assessment

The observed behavior was suspicious and consistent with web reconnaissance.

However, the available evidence did not demonstrate:

- Successful exploitation
- Unauthorized authentication
- Privilege escalation
- Data access
- Data exfiltration
- Persistent access
- Confirmed system compromise

Therefore, the activity should not be described as a confirmed attack.

### Assessment

- **Classification:** Suspicious Reconnaissance
- **Compromise Status:** Not Confirmed
- **Recommended Action:** Continue Investigation and Monitoring

---

## 16. False-Positive Considerations

Several legitimate activities can produce patterns that resemble reconnaissance, including:

- Administrative testing
- Automated monitoring
- Vulnerability scanning
- Development or QA activity
- Misconfigured applications
- User-generated invalid requests

For this reason, HTTP 404 responses, repeated requests, unusual paths, or browser-like User-Agent strings should not be evaluated independently.

Context and evidence correlation are required before escalating an event as a confirmed security incident.

---

## 17. Recommended Next Steps

In a production investigation, additional evidence should be reviewed before determining whether escalation is required.

Recommended next steps include:

1. Correlate the source with authentication and application logs.
2. Review additional requests from the same source.
3. Check for successful access following reconnaissance activity.
4. Review web server error logs.
5. Correlate activity with firewall, proxy, IDS/IPS, or SIEM telemetry where available.
6. Determine whether the source is associated with an approved scanner or internal system.
7. Continue monitoring for repeated or escalating activity.
8. Preserve relevant evidence if the activity develops into a confirmed incident.

---

## 18. Final Conclusion

The investigation demonstrated a structured SOC-style workflow for analyzing suspicious web traffic.

Apache access logs revealed repeated requests targeting administrative and potentially sensitive paths. Status-code analysis, request-frequency analysis, timeline reconstruction, controlled traffic generation, User-Agent spoofing, and network-level inspection were used to develop and test investigative hypotheses.

The evidence supported a conclusion of **suspicious reconnaissance**, but did not establish that the web server had been compromised.

The key analytical principle demonstrated by the lab was:

> Suspicious activity should be investigated and correlated with multiple evidence sources before being classified as a confirmed security incident.

---

## 19. Evidence References

Supporting sanitized evidence is available in:

`../Screenshots/`

The evidence set includes:

- Apache access-log analysis
- Suspicious path identification
- Directory-enumeration analysis
- HTTP 404 filtering
- Request-frequency analysis
- HTTP 408 investigation
- Timeline reconstruction
- Automated curl traffic
- User-Agent spoofing
- Browser and spoofed-header comparison

---

## 20. Repository Information

- **Repository:** Cybersecurity-SOC-Lab
- **Lab:** 01-Web-Server-Log-Investigation
- **Documentation:** `01-Web-Server-Log-Investigation/Documentation/Investigation-Report.md`
- **Screenshots:** `01-Web-Server-Log-Investigation/Screenshots/`
- **Status:** Completed
- **Version:** 1.0
