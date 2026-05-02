---
name: security-architecture
description: Review and analyze security architecture. Use when the user wants to assess system design for security risks, threat modeling, trust boundaries, or architectural vulnerabilities.
---

Perform a security architecture review of the provided system, design, or code.

Focus on:
1. **Threat modeling**: Identify attack surfaces, entry points, and trust boundaries
2. **Authentication & authorization**: Verify identity and access control patterns
3. **Data flow security**: Trace sensitive data paths and encryption requirements
4. **Network segmentation**: Check exposure, isolation, and lateral movement risk
5. **Secrets management**: Identify hardcoded credentials, key storage, rotation
6. **Dependency trust**: Third-party components, supply chain, and update mechanisms
7. **Resilience**: Failure modes, single points of failure, and fallback behavior

Output a prioritized findings list: Critical → High → Medium → Low. For each finding include: what it is, why it matters, and a concrete remediation.
