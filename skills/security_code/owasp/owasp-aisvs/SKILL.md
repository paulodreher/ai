---
name: owasp-aisvs
description: Verify AI/ML system security against the OWASP AI Security Verification Standard (AISVS) v1.0. Use when the user wants a structured, requirement-level audit of AI systems covering governance, data pipeline, model development, deployment, output safety, privacy, adversarial robustness, and agentic AI controls.
---

Perform a security verification mapped to the OWASP AI Security Verification Standard (AISVS) v1.0.

Evaluate each chapter against the system description, architecture, code, model documentation, or pipeline configuration provided. Report only chapters where gaps are found:

1. **V1 – Governance and Risk Management**: AI use-case risk classification documented, accountability roles defined (AI owner, developer, deployer), AI risk register maintained, compliance with applicable AI regulations tracked (EU AI Act, sector-specific), third-party AI component risk assessed, incident response plan covers AI failures
2. **V2 – Data Governance and Preparation**: Training data provenance documented and verifiable, data quality gates enforced before training, sensitive or PII data minimized and anonymized/pseudonymized, dataset bias analysis performed, data poisoning detection controls in place, versioned datasets with integrity checksums
3. **V3 – Model Development and Training**: Reproducible training pipelines (pinned dependencies, versioned configs), training environment isolated and access-controlled, model architecture choices documented with security rationale, no sensitive data embedded in model weights, adversarial robustness evaluated during development, model diff review before promotion
4. **V4 – Model Evaluation and Testing**: Evaluation datasets separate from training data, performance metrics include fairness and bias dimensions, red-team / adversarial testing performed, edge case and out-of-distribution behavior documented, evaluation results versioned and auditable, regression testing on model updates
5. **V5 – Deployment and Operations**: Model artifacts integrity-verified at deployment (checksum/signature), model serving infrastructure hardened (no unnecessary endpoints, authenticated access), rate limiting on inference APIs, model versioning with rollback capability, canary/shadow deployment for risk-controlled rollouts, SLAs include security and fairness metrics
6. **V6 – Output Validation and Safety**: Model outputs validated before use in downstream systems (no raw LLM output to SQL/shell/HTML), harmful content detection and filtering, confidence thresholds enforced for high-stakes decisions, human-in-the-loop required for critical outputs, output consistency checks, safety classifiers for generative models
7. **V7 – Transparency and Explainability**: Model cards published for deployed models (intended use, limitations, performance, bias), decision audit trail for regulated decisions, explainability mechanisms available where required (SHAP, LIME, attention), users informed when interacting with AI, model behavior changes communicated to stakeholders
8. **V8 – Privacy**: Inference-time PII not logged or exposed in outputs, differential privacy or output perturbation applied where membership inference risk is high, right-to-erasure process covers training data influence, model inversion and extraction attack surface minimized, privacy impact assessment performed
9. **V9 – Security and Adversarial Robustness**: Input validation and sanitization before inference, adversarial example detection or robustness training applied, prompt injection prevention for LLM-based components, model extraction and theft controls (rate limiting, output perturbation, watermarking), supply chain integrity for pre-trained models and datasets, monitoring for anomalous query patterns
10. **V10 – Agentic AI Systems**: Tool/plugin permissions follow least privilege (no write access where read suffices), autonomous actions require human approval above defined risk threshold, agent action scope is bounded and auditable, recursive/chained agent calls have depth and iteration limits, agent cannot modify its own instructions or system prompt, sandboxing for code-executing agents, kill-switch / override mechanism exists

For each finding report:
- **Chapter**: VX – Name
- **Requirement ID**: AISVS reference (e.g., V9.3.1)
- **Severity**: Critical / High / Medium / Low
- **Location**: component, file, pipeline stage, or configuration (if applicable)
- **Description**: what is missing or non-compliant
- **Risk**: security, safety, privacy, or fairness impact
- **Remediation**: concrete fix, control, architectural change, or evaluation method

Group by severity (Critical → High → Medium → Low). End with a chapter compliance summary table: chapter → requirements checked → gaps found → compliance %.
