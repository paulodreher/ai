---
name: owasp-top10
description: Review code and design against the OWASP Top 10 (2021) for web applications. Use when the user wants a structured audit covering broken access control, injection, misconfigurations, and the other nine OWASP web risks.
---

Perform a security review mapped to the OWASP Top 10 (2021) for web applications.

Assess each category in turn, then report only categories where findings exist:

1. **A01 – Broken Access Control**: Missing authorization checks, IDOR, path traversal, CORS misconfiguration, privilege escalation, forced browsing
2. **A02 – Cryptographic Failures**: Sensitive data in plaintext, weak/deprecated algorithms (MD5, SHA-1, DES), missing TLS, hardcoded keys, insufficient key length
3. **A03 – Injection**: SQL, NoSQL, OS command, LDAP, XPath, template, and expression-language injection; unsanitized user input reaching interpreters
4. **A04 – Insecure Design**: Missing threat modeling artifacts, absent rate limiting, insecure business logic, lack of security controls in design, no defense-in-depth
5. **A05 – Security Misconfiguration**: Default credentials, unnecessary features enabled, verbose error messages, missing security headers, open cloud storage
6. **A06 – Vulnerable and Outdated Components**: Dependencies with known CVEs, unpinned versions, abandoned libraries, missing SCA in CI
7. **A07 – Identification and Authentication Failures**: Weak passwords allowed, missing MFA, broken session management, credential exposure in URLs or logs
8. **A08 – Software and Data Integrity Failures**: Unsigned or unverified updates, insecure deserialization, CI/CD pipeline without integrity checks, untrusted CDN resources
9. **A09 – Security Logging and Monitoring Failures**: Missing audit logs, logs containing sensitive data, no alerting on failures, logs not protected from tampering
10. **A10 – Server-Side Request Forgery (SSRF)**: Unvalidated user-supplied URLs fetched server-side, missing SSRF allowlists, internal metadata endpoint exposure

For each finding report:
- **Category**: A0X – Name
- **Severity**: Critical / High / Medium / Low
- **Location**: file and line (if applicable)
- **Description**: what the issue is
- **Risk**: what an attacker can do
- **Remediation**: corrected code snippet or concrete fix

Group by severity (Critical → High → Medium → Low). End with a summary table: category → finding count → highest severity.
