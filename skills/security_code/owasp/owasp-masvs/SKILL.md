---
name: owasp-masvs
description: Verify mobile application security against the OWASP Mobile Application Security Verification Standard (MASVS) v2.x. Use when the user wants a structured, requirement-level audit of Android or iOS apps covering storage, cryptography, authentication, network, platform interaction, code quality, and resilience.
---

Perform a security verification mapped to the OWASP MASVS v2.x and its companion MASTG test cases.

Evaluate each control group against the code, configuration, or architecture provided. Report only groups where gaps are found:

1. **MASVS-STORAGE – Secure Data Storage**
   - No sensitive data (credentials, PII, session tokens, keys) in SharedPreferences / NSUserDefaults without encryption
   - No sensitive data on external storage (SD card, public directories)
   - Keystore (Android) / Keychain (iOS) used for cryptographic keys and secrets
   - No sensitive data in app logs, backups, or SQLite databases without encryption
   - Clipboard access restricted for sensitive fields

2. **MASVS-CRYPTO – Cryptography**
   - No custom or deprecated algorithms (MD5, SHA-1, DES, ECB mode, RC4)
   - Cryptographically strong random number generation (SecureRandom / SecRandomCopyBytes)
   - Keys generated with sufficient length (AES-256, RSA-2048+), stored in hardware-backed Keystore/Secure Enclave where available
   - IVs/nonces unique per encryption operation, not hardcoded
   - No hardcoded symmetric keys or private keys in source or resources

3. **MASVS-AUTH – Authentication and Authorization**
   - Biometric authentication uses platform APIs correctly (Android BiometricPrompt / iOS LocalAuthentication)
   - Biometric re-enrollment invalidates existing keys
   - Server-side session tokens validated on every request; authorization not purely client-side
   - Sensitive operations require step-up authentication
   - Session timeout enforced; tokens revocable server-side

4. **MASVS-NETWORK – Network Communication**
   - All network traffic uses TLS 1.2+ with valid certificates
   - Certificate pinning implemented for high-value connections; pins rotation path exists
   - No cleartext HTTP traffic permitted (Network Security Config / ATS enforced)
   - No sensitive data in URLs, query parameters, or HTTP headers visible in logs
   - TLS certificate validation not suppressed or bypassed in code

5. **MASVS-PLATFORM – Platform Interaction**
   - No sensitive data exposed via IPC (exported Activities/Services/ContentProviders/BroadcastReceivers without permissions)
   - Deep links and URI schemes validated before use
   - WebViews: `setJavaScriptEnabled` justified, `addJavascriptInterface` minimized and annotated, file access disabled, TLS enforced
   - Permissions requested are minimal and justified; dangerous permissions explained at point of use
   - Pasteboard / intent data validated before use

6. **MASVS-CODE – Code Quality and Build Settings**
   - No hardcoded credentials, API keys, or secrets in source, assets, or build configs
   - Third-party libraries up to date and without known CVEs; dependency provenance verified
   - Production build: debuggable flag disabled (`android:debuggable=false`), ADB backup disabled, compiler security flags enabled (PIE, stack canaries, ASLR)
   - No unnecessary permissions in manifest
   - Free of memory safety vulnerabilities in native code (use-after-free, buffer overflow) where applicable

7. **MASVS-RESILIENCE – Tampering and Reverse Engineering Resistance** *(applies to high-risk apps)*
   - Root/jailbreak detection implemented and enforced
   - Anti-tampering controls (code signing validation, integrity checks)
   - Anti-debugging measures (detection of attached debugger)
   - Code obfuscation applied to business-critical logic
   - Runtime instrumentation (Frida, Xposed) detection where required

For each finding report:
- **Control Group**: MASVS-XXXX
- **Requirement ID**: MASVS reference (e.g., MASVS-STORAGE-1)
- **Platform**: Android / iOS / Both
- **Severity**: Critical / High / Medium / Low
- **Location**: file and line (if applicable)
- **Description**: what is missing or non-compliant
- **Risk**: what an attacker can do
- **Remediation**: corrected code snippet or concrete platform-specific fix

Group by severity (Critical → High → Medium → Low). End with a control group compliance summary table: group → requirements checked → gaps found → compliance %.
