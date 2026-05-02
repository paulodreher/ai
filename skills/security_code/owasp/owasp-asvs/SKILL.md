---
name: owasp-asvs
description: Verify web application security against the OWASP Application Security Verification Standard (ASVS) v4.0. Use when the user wants a structured, requirement-level audit of authentication, session management, access control, cryptography, API security, and other ASVS chapters.
---

Perform a security verification mapped to the OWASP Application Security Verification Standard (ASVS) v4.0.

Unless the user specifies otherwise, target **Level 2** (standard for most applications). Flag requirements that apply only at L3 (high-assurance / critical systems).

Evaluate each chapter against the code, architecture, or design provided. Report only chapters where gaps are found:

1. **V1 – Architecture, Design and Threat Modeling**: Secure SDLC practices, component trust boundaries documented, threat model exists and is current, dependency inventory, security controls not reliant on client-side enforcement
2. **V2 – Authentication**: Password strength and storage (bcrypt/Argon2/scrypt), MFA availability and bypass resistance, credential recovery security (no hints, secure out-of-band reset), anti-automation on auth endpoints, account lockout, lookup secret strength
3. **V3 – Session Management**: Cryptographically random session IDs (≥128-bit entropy), session invalidation on logout and timeout, no session tokens in URLs, re-authentication for sensitive operations, concurrent session controls, cookie attributes (Secure, HttpOnly, SameSite)
4. **V4 – Access Control**: Enforce server-side, deny-by-default, principle of least privilege, no IDOR (object-level ownership checks), directory traversal prevention, attribute-based or role-based access control consistent across all paths
5. **V5 – Validation, Sanitization and Encoding**: Input validation on all untrusted sources, output encoding context-appropriate (HTML, JS, CSS, URL), injection prevention (SQL, OS, LDAP, XPath, template), safe file upload handling, deserialization controls
6. **V6 – Stored Cryptography**: Approved algorithms only (AES-256, RSA-2048+, ECDSA P-256+), no MD5/SHA-1 for security, no DES/3DES/RC4, keys stored separately from encrypted data, key rotation capability, no hardcoded cryptographic keys or IVs
7. **V7 – Error Handling and Logging**: No stack traces or sensitive data in responses, centralized logging, audit log for authentication and access-control events, logs tamper-resistant and retained appropriately, no credentials or PII in logs
8. **V8 – Data Protection**: Sensitive data classified and protected at rest and in transit, no sensitive data in browser cache or local storage without necessity, data minimization, HTTP caching headers prevent sensitive data caching
9. **V9 – Communication**: TLS 1.2+ enforced everywhere, strong cipher suites only, valid certificates, certificate pinning for high-value apps, no mixed content, HSTS with adequate max-age
10. **V10 – Malicious Code**: No backdoors or hidden functionality, no hardcoded credentials, no time bombs or undisclosed network calls, dependency integrity verification
11. **V11 – Business Logic**: Rate limiting on high-value flows, workflow steps cannot be skipped, protection against automated abuse (mass account creation, credential stuffing), values within expected ranges
12. **V12 – Files and Resources**: File type validation by content (not extension), storage outside web root or with no-execute permissions, file size limits enforced, no path traversal in filenames, malware scanning for user uploads where applicable
13. **V13 – API and Web Service**: REST/GraphQL/SOAP authentication required, input validated on every parameter, GraphQL depth/complexity limits, HTTP method restrictions, API versioning, no sensitive data in GET parameters
14. **V14 – Configuration**: No default credentials, unnecessary features/endpoints disabled, security headers present (CSP, X-Frame-Options, X-Content-Type-Options, Referrer-Policy), dependency versions pinned and up to date, secrets not in source control

For each finding report:
- **Chapter**: VX – Name
- **Requirement ID**: ASVS reference (e.g., V2.1.1)
- **Level**: L1 / L2 / L3
- **Severity**: Critical / High / Medium / Low
- **Location**: file and line (if applicable)
- **Description**: what is missing or non-compliant
- **Risk**: what an attacker can do
- **Remediation**: corrected code snippet or concrete fix

Group by severity (Critical → High → Medium → Low). End with a chapter compliance summary table: chapter → requirements checked → gaps found → compliance %.
