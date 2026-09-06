# Findings Summary — Future Interns Task 1

## Assessment Target

**Target:** OWASP Juice Shop  
**URL:** http://127.0.0.1:3000  
**Environment:** Locally hosted training application  
**Assessment type:** Passive / read-only vulnerability assessment

## Scope and Methodology

The assessment was performed only against the locally hosted OWASP Juice Shop instance. The work was limited to passive observation, service identification, HTTP response/header review, OWASP ZAP passive scanning, and browser Developer Tools inspection.

No login bypass, exploitation, brute force, denial-of-service, or other harmful testing was performed as part of the intended assessment methodology.

## Findings Matrix

| ID | Finding | Severity | Evidence / Source | Assessment |
|---|---|---|---|---|
| F-01 | Content Security Policy (CSP) Header Not Set | Medium | OWASP ZAP | The main application response did not include a `Content-Security-Policy` header. A CSP can provide an additional layer of protection against XSS and data-injection risks. |
| F-02 | Permissive Cross-Origin Resource Sharing (CORS) | Medium | OWASP ZAP + HTTP headers | The response included `Access-Control-Allow-Origin: *`. A wildcard policy can allow cross-origin requests from arbitrary origins. The practical impact depends on whether sensitive resources are accessible without appropriate authentication and authorization. |
| F-03 | Missing Anti-clickjacking Header on Socket.IO polling endpoint | Medium* | OWASP ZAP + Browser/HTTP inspection | ZAP identified a Socket.IO polling response without the expected anti-clickjacking protection. This is endpoint-specific. The main homepage response was observed to include `X-Frame-Options: SAMEORIGIN`, so this finding should not be described as a site-wide absence of clickjacking protection. |
| F-04 | Session Identifier in Socket.IO URL | Medium* | OWASP ZAP | ZAP detected a `sid` parameter in the Socket.IO polling URL. This appears to be a Socket.IO connection identifier rather than a confirmed application authentication-session identifier. Exposure through URLs can create risks through browser history, logs, or referrers, so the finding requires validation before treating it as a confirmed authentication-session vulnerability. |
| F-05 | Authentication-related data stored in browser localStorage | Low | OWASP ZAP + Firefox Developer Tools | Browser storage inspection showed `email` and `token` keys in Local Storage. Storing authentication-related tokens in JavaScript-accessible browser storage increases exposure if client-side script execution is compromised. Actual token values must not be included in the public repository. |
| F-06 | Private IP Address Disclosure | Low | OWASP ZAP | A response exposed private/internal addresses such as `192.168.99.100:3000` and `192.168.99.100:4200`. Internal network information can provide useful reconnaissance context to an attacker. |
| F-07 | Missing X-Content-Type-Options Header on specific responses | Low | OWASP ZAP | ZAP reported missing `X-Content-Type-Options` protection on three responses. The main homepage response was observed to include `X-Content-Type-Options: nosniff`, so this should be treated as an endpoint-specific configuration issue rather than a site-wide absence. |

*Contextual finding / requires validation before being presented as a confirmed exploitable vulnerability.

## Informational ZAP Alerts

OWASP ZAP also reported the following informational observations:

- Authentication Request Identified
- Information Disclosure — Information in Browser localStorage
- Information Disclosure — Information in Browser sessionStorage
- Information Disclosure — Suspicious Comments
- Modern Web Application

These were not treated as confirmed vulnerabilities in this assessment.

## ZAP Scan Summary

The passive ZAP report identified:

- **High:** 0
- **Medium:** 4
- **Low:** 4
- **Informational:** 5
- **Alert categories:** 13
- **Endpoints observed:** 49

The four Medium alerts are scanner classifications. They should not automatically be interpreted as four confirmed exploitable vulnerabilities because some findings are endpoint-specific or require application-context validation.

## Remediation Recommendations

### F-01 — CSP Header
Implement a restrictive `Content-Security-Policy` appropriate to the application's required scripts, styles, images, connections, and other resources. Begin with a report-only policy where practical and tune it before enforcement.

### F-02 — CORS
Avoid a wildcard `Access-Control-Allow-Origin: *` where cross-origin access is not required. Restrict allowed origins to trusted domains and ensure sensitive APIs require appropriate authentication and authorization.

### F-03 — Anti-clickjacking Protection
Ensure relevant application responses include appropriate clickjacking protection using `Content-Security-Policy: frame-ancestors` and/or `X-Frame-Options`, including applicable endpoints that can be rendered in a browser context.

### F-04 — Socket.IO URL Identifier
Review the Socket.IO transport configuration and determine whether the `sid` is only a transient connection identifier. Avoid placing sensitive authentication/session identifiers in URLs. Where authentication session identifiers are involved, prefer secure cookie-based mechanisms.

### F-05 — Browser Storage
Avoid storing sensitive authentication material in JavaScript-accessible Local Storage when a safer mechanism is available. Review token handling and consider secure, appropriately configured cookies for authentication state.

### F-06 — Private IP Disclosure
Remove unnecessary internal/private IP addresses and infrastructure details from client-visible responses.

### F-07 — `X-Content-Type-Options`
Set `X-Content-Type-Options: nosniff` consistently on applicable HTTP responses, particularly responses that may be interpreted as browser resources.

## Evidence Files

Recommended public evidence structure:

```text
evidence/
├── nmap/
│   └── 01-basic-service-scan.txt
├── zap/
│   ├── zap-report.html
│   └── zap-alerts.json
└── browser-devtools/
    ├── 01-response-headers.png
    ├── 02-local-storage.png
    └── 03-token-cookie-attributes.png
```

Before publishing screenshots to GitHub, redact any JWT/token values, email addresses, session identifiers, or other unnecessary sensitive values.

## Conclusion

The assessment found several security-hardening issues in the locally hosted OWASP Juice Shop application. The most notable observations were the absence of a CSP header and the permissive CORS policy. Other findings were endpoint-specific or contextual and require validation before being treated as confirmed exploitable vulnerabilities.

The results demonstrate how passive network/service identification, HTTP header inspection, OWASP ZAP, and browser Developer Tools can be combined to identify security weaknesses without performing exploitation or harmful testing.
