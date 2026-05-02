---
name: security-code
description: Review code for security vulnerabilities. Use when the user wants a security audit of source code, looking for OWASP issues, injection flaws, insecure patterns, or CVE-relevant weaknesses.
---

Perform a security code review of the provided code.

Focus on:
1. **Injection flaws**: SQL, command, LDAP, XPath, template injection
2. **Authentication issues**: Weak passwords, broken session management, missing MFA
3. **Authorization flaws**: Missing access checks, privilege escalation, IDOR
4. **Sensitive data exposure**: Logging secrets, unencrypted storage, weak crypto
5. **Input validation**: Missing sanitization, type confusion, path traversal
6. **Dependency vulnerabilities**: Known CVEs in imported packages
7. **Cryptographic weaknesses**: Deprecated algorithms, hardcoded keys, weak RNG
8. **Error handling**: Stack traces leaked, verbose errors, swallowed exceptions

For each finding report: severity (Critical/High/Medium/Low), file and line, description, exploit scenario, and a corrected code snippet.
