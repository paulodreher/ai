---
name: owasp-top10-api
description: Review APIs against the OWASP API Security Top 10 (2023). Use when the user wants to audit REST, GraphQL, gRPC, or WebSocket APIs for authorization flaws, resource abuse, and API-specific attack surfaces.
---

Perform a security review mapped to the OWASP API Security Top 10 (2023).

Assess each category in turn, then report only categories where findings exist:

1. **API1 – Broken Object Level Authorization (BOLA)**: Missing per-object ownership checks, user A can access user B's resources by changing an ID, no row-level security enforcement
2. **API2 – Broken Authentication**: Missing or weak token validation, JWT algorithm confusion (none/HS256 downgrade), tokens without expiry, API keys in URLs or logs, no rate limiting on auth endpoints
3. **API3 – Broken Object Property Level Authorization**: Mass assignment vulnerabilities, excessive data exposure (returning full object when only partial is needed), missing field-level access control
4. **API4 – Unrestricted Resource Consumption**: No pagination limits, missing rate limiting and throttling, unbounded query parameters (e.g., `limit=999999`), no request size caps, no cost-aware query controls (GraphQL depth/complexity)
5. **API5 – Broken Function Level Authorization**: Administrative endpoints accessible to regular users, HTTP method differences not enforced (`GET` vs `DELETE`), missing role checks on sensitive operations
6. **API6 – Unrestricted Access to Sensitive Business Flows**: No bot/abuse detection on high-value flows (checkout, OTP, account creation), missing CAPTCHA or behavioral analysis, business logic exploitable via automation
7. **API7 – Server-Side Request Forgery (SSRF)**: User-controlled URLs fetched server-side, no SSRF allowlist, internal service or cloud metadata endpoint reachable
8. **API8 – Security Misconfiguration**: Unnecessary HTTP methods enabled, missing security headers (CORS too permissive, no HSTS), default error messages exposing stack traces, TLS not enforced
9. **API9 – Improper Inventory Management**: Shadow/undocumented APIs, deprecated API versions still active, missing API gateway enforcement, no API catalog or versioning strategy
10. **API10 – Unsafe Consumption of APIs**: Third-party API responses trusted without validation, redirect following without verification, data from external APIs written to DB without sanitization

For each finding report:
- **Category**: APIX – Name
- **Severity**: Critical / High / Medium / Low
- **Location**: endpoint, file, and line (if applicable)
- **Description**: what the issue is
- **Risk**: what an attacker can do
- **Remediation**: corrected code snippet, header config, or architectural fix

Group by severity (Critical → High → Medium → Low). End with a summary table: category → finding count → highest severity.
