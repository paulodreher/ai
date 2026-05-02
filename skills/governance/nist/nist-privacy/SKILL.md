---
name: nist-privacy
description: Assess a system against the NIST Privacy Framework 1.0. Use when the user wants to evaluate privacy risk management practices, data processing activities, or alignment with privacy-by-design principles across the five Privacy Framework functions.
---

Perform a privacy risk assessment mapped to the NIST Privacy Framework 1.0.

Evaluate the system across the five Privacy Framework Core Functions. For each, identify gaps relative to the system description, data flows, architecture, policies, or code provided:

1. **Identify-P (ID-P)**: Data inventory and mapping (what personal data is collected, processed, stored, shared), privacy roles defined (DPO, data stewards), legal basis for processing documented, privacy risk assessment performed, third-party data processor inventory maintained
2. **Govern-P (GV-P)**: Privacy policies established and enforced, privacy program accountability defined, risk tolerance for privacy established, privacy requirements integrated into development lifecycle, legal and regulatory compliance tracked (GDPR, CCPA, HIPAA, etc.), privacy training for personnel
3. **Control-P (CT-P)**: Data subject rights supported (access, correction, deletion, portability, objection), consent management mechanisms, data minimization enforced, purpose limitation controls, retention and deletion schedules implemented, privacy preferences respected
4. **Communicate-P (CM-P)**: Privacy notices accurate and understandable (plain language), data processing activities disclosed, breach notification processes defined, transparency reports, communication of data sharing with third parties
5. **Protect-P (PR-P)**: Technical and organizational safeguards for personal data (encryption, pseudonymization, access control, anonymization), privacy-by-design in system architecture, data loss prevention controls, security incident response covering privacy breaches, third-party risk management for processors

For each gap or finding report:
- **Function**: ID-P / GV-P / CT-P / CM-P / PR-P
- **Category** (Privacy Framework reference, e.g., CT-P.DP-P3)
- **Severity**: Critical / High / Medium / Low
- **Description**: what is missing, inadequate, or non-compliant
- **Privacy Risk**: potential harm to individuals (dignity, financial, physical, reputational)
- **Remediation**: concrete action, control, design change, or policy update

Group findings by Function. End with a privacy posture scorecard: Function → current maturity (Initial / Repeatable / Defined / Managed / Optimizing) and a list of the top three priority remediation actions.
