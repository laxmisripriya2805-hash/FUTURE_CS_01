# FUTURE_CS_01 — Vulnerability Assessment Report

## Future Interns Cyber Security Internship — Task 1

A passive vulnerability assessment of the OWASP Juice Shop training application using Nmap, OWASP ZAP, and Firefox Developer Tools.

## Target

- **Application:** OWASP Juice Shop
- **URL:** `http://127.0.0.1:3000`
- **Environment:** Locally hosted training application
- **Assessment Type:** Passive / read-only assessment

The application was hosted locally to provide a controlled and authorized assessment environment.

## Objective

The objective of this task was to identify common web security weaknesses, classify their business risk, document supporting evidence, and provide practical remediation recommendations.

## Scope

The assessment covered:

- Publicly accessible application responses
- TCP port/service identification
- HTTP response headers
- Passive OWASP ZAP alerts
- Browser Developer Tools
- Browser Local Storage, Session Storage, and cookie attributes
- Security configuration and information-disclosure observations

### Out of Scope

The assessment did not include:

- Login bypass
- Exploitation
- Brute-force attacks
- Denial-of-service testing
- Destructive requests
- Credential attacks
- Testing unrelated public systems

## Tools Used

| Tool | Purpose |
|---|---|
| Nmap | Service and port identification |
| OWASP ZAP 2.17.0 | Passive web security assessment |
| Firefox Developer Tools | Header, storage, and cookie inspection |
| Docker | Local deployment of the target application |

## Key Results

OWASP ZAP reported:

| Severity | Count |
|---|---:|
| High | 0 |
| Medium | 4 |
| Low | 4 |
| Informational | 5 |

ZAP identified **13 alert categories** across **49 observed endpoints**.

These scanner alerts were manually reviewed. Some were determined to be informational or endpoint-specific observations rather than confirmed exploitable vulnerabilities.

### Key Findings

- **F-01 — Missing Content Security Policy (CSP):** Medium
- **F-02 — Permissive CORS policy:** Medium
- **F-03 — Missing anti-clickjacking protection on a Socket.IO polling endpoint:** Medium, contextual
- **F-04 — Session identifier in Socket.IO URL:** Medium, contextual / requires validation
- **F-05 — Authentication-related data in browser Local Storage:** Low
- **F-06 — Private IP address disclosure:** Low
- **F-07 — Missing `X-Content-Type-Options` on specific responses:** Low, endpoint-specific

See [`findings/findings-summary.md`](findings/findings-summary.md) for the detailed findings and remediation recommendations.

## Methodology

1. Identified the locally exposed web service with Nmap.
2. Performed passive analysis using OWASP ZAP in Standard Mode.
3. Reviewed HTTP security headers.
4. Inspected browser-side storage and cookie attributes using Firefox Developer Tools.
5. Reviewed and contextualized automated scanner alerts.
6. Classified findings by risk and documented practical remediation.

Full methodology is available in [`scope/scope-and-methodology.md`](scope/scope-and-methodology.md).

## Evidence

```text
evidence/
├── nmap/
│   ├── 01-basic-service-scan.txt
│   └── 01-nmap-terminal.png
├── zap/
│   ├── zap-report.html
│   └── zap-alerts.json
└── browser-devtools/
    ├── 01-response-headers.png
    ├── 02-local-storage.png
    └── 03-token-cookie-attributes.png
```

**Important:** Authentication tokens, JWT values, session identifiers, and other unnecessary sensitive values must be redacted before publishing evidence to GitHub.

## Report

The final professional assessment report is available at:

`report/Vulnerability_Assessment_Report.pdf`

## Ethics

This assessment was performed against a locally hosted OWASP Juice Shop training application in a controlled environment. Testing was intentionally limited to passive and non-destructive techniques.

## Conclusion

The assessment identified several security-hardening opportunities, particularly around CSP, CORS configuration, browser-side token handling, and endpoint-specific security headers. The results are documented with evidence and remediation guidance while avoiding exploitative testing.

---

**Future Interns — Cyber Security Internship | Task 1**
