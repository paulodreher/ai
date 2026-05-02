---
name: owasp-top10-mobile
description: Review mobile applications against the OWASP Mobile Top 10 (2024). Use when the user wants to audit Android or iOS apps for insecure data storage, weak authentication, insecure communication, and mobile-specific attack surfaces.
---

Perform a security review mapped to the OWASP Mobile Top 10 (2024).

Assess each category in turn, then report only categories where findings exist:

1. **M1 – Improper Credential Usage**: Hardcoded credentials in source or config, API keys/tokens in APK/IPA resources, credentials logged or stored in SharedPreferences/UserDefaults in plaintext
2. **M2 – Inadequate Supply Chain Security**: Unverified third-party SDKs, malicious or tampered dependencies, no dependency integrity checks, outdated libraries with known CVEs
3. **M3 – Insecure Authentication/Authorization**: Weak PIN/password policies, missing biometric re-authentication for sensitive actions, insecure token storage, JWT misuse, missing server-side authorization (trusting client-side checks)
4. **M4 – Insufficient Input/Output Validation**: Missing input sanitization leading to injection via deep links or IPC, unvalidated intent data, XSS in WebViews via `addJavascriptInterface`, unsafe deserialization
5. **M5 – Insecure Communication**: Missing certificate pinning, accepting invalid/self-signed TLS certificates, cleartext HTTP traffic, sensitive data in URLs or query strings, insecure WebSocket connections
6. **M6 – Inadequate Privacy Controls**: Excessive permissions requested, PII in logs (logcat/os_log), sensitive data exposed via screenshots, clipboard leakage, third-party analytics SDKs collecting PII
7. **M7 – Insufficient Binary Protections**: Missing code obfuscation, debuggable flag set in production (`android:debuggable=true`), missing root/jailbreak detection, no anti-tampering or integrity checks
8. **M8 – Security Misconfiguration**: Overly permissive exported components (Activities, Services, Receivers with no permission), ADB backup enabled, insecure WebView settings (`setAllowFileAccess`, `setJavaScriptEnabled` broadly)
9. **M9 – Insecure Data Storage**: Sensitive data in external storage, unencrypted SQLite databases, data in app logs, sensitive content in `onSaveInstanceState`, iOS NSUserDefaults without encryption
10. **M10 – Insufficient Cryptography**: Weak algorithms (MD5, SHA-1, DES, ECB mode), hardcoded or short keys, predictable IVs, using `Math.random()` for security-sensitive purposes, rolling custom crypto

For each finding report:
- **Category**: MX – Name
- **Severity**: Critical / High / Medium / Low
- **Platform**: Android / iOS / Both
- **Location**: file and line (if applicable)
- **Description**: what the issue is
- **Risk**: what an attacker can do
- **Remediation**: corrected code snippet or concrete fix

Group by severity (Critical → High → Medium → Low). End with a summary table: category → finding count → highest severity.
