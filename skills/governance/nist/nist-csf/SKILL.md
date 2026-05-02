---
name: nist-csf
description: Assess a system or organization against the NIST Cybersecurity Framework 2.0. Use when the user wants a structured gap analysis across the six CSF functions (Govern, Identify, Protect, Detect, Respond, Recover).
---

Perform a security assessment mapped to the NIST Cybersecurity Framework (CSF) 2.0.

Evaluate each of the six CSF Functions. For each, identify gaps, weaknesses, and missing controls relative to the system, code, architecture, or policy provided:

1. **GV – Govern**: Cybersecurity roles and responsibilities defined, risk appetite established, policies documented and enforced, supply chain risk management, cybersecurity integrated into enterprise risk management
2. **ID – Identify**: Asset inventory (hardware, software, data, users), business environment context, risk assessment processes, vulnerability identification, third-party dependency tracking
3. **PR – Protect**: Identity management and access control (MFA, least privilege), data security (encryption, DLP, classification), platform security (hardening, patching, secure config), resilience (backups, redundancy), training and awareness
4. **DE – Detect**: Continuous monitoring, anomaly and event detection, security logging (SIEM), threat intelligence integration, alerting thresholds and tuning
5. **RS – Respond**: Incident response plan existence and testing, communications (internal and external notification), analysis (forensics, root cause), containment and eradication procedures, post-incident review
6. **RC – Recover**: Recovery plan existence and testing, disaster recovery and BCP alignment, backup restoration procedures, post-incident improvements, communications during recovery

For each gap or finding report:
- **Function**: GV / ID / PR / DE / RS / RC
- **Category/Subcategory**: CSF reference (e.g., PR.AA-01)
- **Severity**: Critical / High / Medium / Low
- **Description**: what is missing or inadequate
- **Impact**: business or security risk if unaddressed
- **Remediation**: concrete action, control, or policy to implement

Group findings by Function, then by severity within each Function. End with an overall maturity heat map: Function → current posture (Initial / Developing / Defined / Managed / Optimizing).
