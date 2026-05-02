---
name: nist-rmf-ai
description: Assess an AI/ML system against the NIST AI Risk Management Framework (AI RMF 1.0). Use when the user wants to evaluate trustworthy AI properties, identify risks across the AI lifecycle, or map controls to the four AI RMF core functions.
---

Perform a risk management assessment mapped to the NIST AI Risk Management Framework (AI RMF 1.0) and its Playbook.

Evaluate the AI system across the four AI RMF Core Functions. For each, identify gaps relative to the system description, model cards, architecture, or artifacts provided:

1. **GOVERN**: AI risk management policies and accountability structures, roles defined (AI Risk Owner, developer, deployer, end-user), risk tolerance documented, AI governance integrated into enterprise risk management, third-party AI component oversight, team diversity and bias awareness processes
2. **MAP**: AI use case context documented (intended use, users, environment), risk identification across the AI lifecycle (data, model, deployment), impact assessment (safety, fairness, privacy, security, explainability), stakeholder and affected-party identification, failure mode analysis, legal and regulatory mapping
3. **MEASURE**: Trustworthy AI properties evaluated:
   - **Accuracy & Reliability**: Performance metrics, edge case testing, distributional shift monitoring
   - **Fairness & Bias**: Demographic parity, disparate impact testing, dataset representation analysis
   - **Explainability & Transparency**: Model interpretability, decision audit trails, documentation completeness
   - **Privacy**: Training data minimization, inference-time PII exposure, differential privacy controls
   - **Security & Robustness**: Adversarial robustness testing, input validation, poisoning resistance
   - **Safety**: Harm scenario analysis, output filtering, human oversight mechanisms
4. **MANAGE**: Risk response plans (accept/transfer/mitigate/avoid), incident response for AI failures, model monitoring and drift detection, feedback loops for continuous improvement, decommissioning and rollback procedures, residual risk tracking and POA&M

For each gap or finding report:
- **Function**: GOVERN / MAP / MEASURE / MANAGE
- **Trustworthy AI Property** (if applicable): Accuracy / Fairness / Explainability / Privacy / Security / Safety / Reliability
- **Severity**: Critical / High / Medium / Low
- **Description**: what is missing, inadequate, or not addressed
- **Impact**: risk to trustworthiness, users, or affected communities
- **Remediation**: concrete action, evaluation method, control, or documentation to implement

Group findings by Function. End with a trustworthy AI property scorecard: property → current maturity (Not Started / Developing / Defined / Managed) and overall AI RMF alignment rating.
