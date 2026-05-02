---
name: nist-rmf
description: Assess a system against the NIST Risk Management Framework (SP 800-37 Rev. 2). Use when the user wants to evaluate RMF step compliance, authorization readiness, or SP 800-53 control implementation for federal or regulated systems.
---

Perform a risk management assessment mapped to the NIST RMF (SP 800-37 Rev. 2) and SP 800-53 Rev. 5 controls.

Evaluate each RMF step for completeness and correctness based on the system description, architecture, or artifacts provided:

1. **Prepare**: Organizational roles defined (AO, ISSO, ISSM), risk management strategy documented, common control identification, security and privacy requirements established, system registration
2. **Categorize (FIPS 199 / SP 800-60)**: Information types identified, impact levels assigned (Confidentiality / Integrity / Availability: Low / Moderate / High), overall system categorization documented and justified
3. **Select (SP 800-53)**: Baseline controls selected per categorization, tailoring documented (additions, removals, compensating controls), organization-defined parameters filled, overlays applied where applicable
4. **Implement**: Controls implemented as documented, configuration management baseline established, implementation descriptions match design, code/config aligns with control requirements
5. **Assess (SP 800-53A)**: Security Assessment Plan (SAP) developed, controls tested (examine/interview/test methods), Security Assessment Report (SAR) produced, POA&M entries created for findings
6. **Authorize**: Authorization package complete (SSP, SAR, POA&M), residual risk accepted by Authorizing Official, Authorization to Operate (ATO) or Denial (DATO) decision documented, continuous monitoring plan in place
7. **Monitor**: Ongoing control assessments, configuration and vulnerability management active, POA&M remediation tracked, significant change assessment process defined, annual ISSO review, reauthorization triggers identified

For each gap or finding report:
- **RMF Step**: step name
- **Control Family / ID** (SP 800-53 reference, e.g., AC-2, SI-3)
- **Severity**: Critical / High / Moderate / Low
- **Description**: what is missing, incomplete, or non-compliant
- **Impact**: risk to system authorization or security posture
- **Remediation**: specific action, documentation update, or control implementation needed

Group findings by RMF step. End with an authorization readiness summary: step → status (Complete / Partially Complete / Not Started) and overall ATO readiness assessment.
