# Scope and Methodology 

## 1. Assessment Overview

**Project:** Vulnerability Assessment Report for a Live Website  
**Target:** OWASP Juice Shop  
**Target URL:** `http://127.0.0.1:3000`  
**Assessment Type:** Passive / read-only security assessment  
**Environment:** Locally hosted OWASP Juice Shop training application

This assessment was performed as part of Future Interns Cyber Security Task 1. The OWASP Juice Shop instance was hosted locally to provide an authorized and controlled assessment target.

## 2. Scope

The assessment was limited to the locally hosted web application available at:

`http://127.0.0.1:3000`

The assessment covered:

- Publicly accessible application responses
- HTTP response headers
- Service identification on TCP port 3000
- Passive OWASP ZAP observations
- Browser Developer Tools inspection
- Browser storage and cookie configuration review
- Security-related configuration and information-disclosure observations

No external websites or third-party systems were assessed.

## 3. Testing Methodology

The assessment followed a non-destructive methodology:

### Step 1 — Service Identification

Nmap was used to identify the exposed service on TCP port 3000 and collect basic service/HTTP response information.

### Step 2 — Passive Web Security Assessment

OWASP ZAP was operated in Standard Mode for passive analysis. The generated alerts were reviewed and contextualized rather than treating every scanner alert as a confirmed vulnerability.

### Step 3 — HTTP Header Review

HTTP response headers were reviewed to identify security-related configuration issues such as:

- Content Security Policy
- Cross-Origin Resource Sharing
- Clickjacking protection
- MIME-sniffing protection

### Step 4 — Browser Developer Tools Review

Firefox Developer Tools were used to inspect:

- Network responses and headers
- Local Storage
- Session Storage
- Cookie attributes

Security-sensitive values such as JWT/token contents were not included in the public evidence.

### Step 5 — Finding Validation and Risk Classification

Scanner observations were manually reviewed and categorized as:

- Medium
- Low
- Informational

Where an alert was endpoint-specific or required additional validation, it was explicitly identified as contextual rather than presented as a confirmed exploitable vulnerability.

## 4. Tools Used

| Tool | Purpose |
|---|---|
| Nmap | Service and port identification |
| OWASP ZAP 2.17.0 | Passive web security assessment |
| Firefox Developer Tools | HTTP, storage, and cookie inspection |
| Docker | Local deployment of OWASP Juice Shop |

## 5. Ethical and Safety Boundaries

The assessment was intentionally limited to authorized, non-destructive testing.

The following activities were **not performed as part of the intended assessment methodology**:

- Login bypass
- Exploitation of vulnerabilities
- Brute-force attacks
- Denial-of-service testing
- Destructive requests
- Credential attacks
- Testing of unrelated public systems

The target was a locally hosted training application under the tester's control.

## 6. Evidence Handling

Evidence was organized under the project `evidence/` directory.

Evidence includes:

- Nmap service-scan output
- OWASP ZAP passive report and alert data
- Browser Developer Tools screenshots

Before public GitHub publication, screenshots and files should be checked to ensure that authentication tokens, session identifiers, email addresses, or other unnecessary sensitive values are redacted.

## 7. Assessment Limitations

This assessment was passive and non-exploitative. Therefore, the results indicate observed security weaknesses and configuration issues but do not establish exploitability for every finding.

In particular, endpoint-specific Socket.IO findings require application-context validation before being treated as confirmed authentication or session-management vulnerabilities.

## 8. Methodology Conclusion

The methodology combines service identification, passive automated analysis, manual HTTP header review, and browser-side inspection. This provides a practical security-hardening assessment while maintaining a controlled and non-destructive testing scope.
