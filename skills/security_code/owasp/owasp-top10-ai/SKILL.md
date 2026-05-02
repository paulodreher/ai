---
name: owasp-top10-ai
description: Review ML/AI systems against the OWASP Machine Learning Security Top 10. Use when the user wants to assess ML pipelines, model serving, training data, or AI-integrated applications for adversarial and supply-chain risks.
---

Perform a security review mapped to the OWASP Machine Learning Security Top 10.

Assess each category in turn, then report only categories where findings exist:

1. **ML01 – Input Manipulation (Adversarial Examples)**: Missing input validation and sanitization before inference, no adversarial robustness testing, lack of anomaly detection on model inputs
2. **ML02 – Data Poisoning**: Untrusted training data sources, no data provenance tracking, missing integrity checks on datasets, no monitoring for distribution shift in production data
3. **ML03 – Model Inversion Attack**: Model outputs expose enough information to reconstruct training data (PII leakage), overly verbose prediction APIs, no output perturbation or differential privacy
4. **ML04 – Membership Inference Attack**: No rate limiting or query throttling on inference endpoints, high-confidence outputs enable membership inference, missing audit logging of repeated queries
5. **ML05 – Model Theft**: Unauthenticated or unrestricted inference endpoints, excessive API response detail enabling model extraction, no watermarking or extraction detection
6. **ML06 – AI Supply Chain Attack**: Untrusted pre-trained models downloaded from unverified sources, missing checksum/signature verification on model artifacts, unpinned model versions
7. **ML07 – Transfer Learning Attack**: Fine-tuning on unvalidated datasets, no verification of base model provenance, hidden backdoor triggers in transferred weights
8. **ML08 – Model Skewing**: Real-time feedback loops that allow adversarial influence of future training, no data quality gates before retraining, missing human-in-the-loop review
9. **ML09 – Output Integrity Attack**: Model outputs used in high-stakes decisions without human oversight, no consistency/sanity checks on predictions, missing output signing or audit trail
10. **ML10 – Model Poisoning**: Compromised training pipeline, untrusted contributors to federated learning, no model diff review before deployment, absent rollback capability

For each finding report:
- **Category**: ML0X – Name
- **Severity**: Critical / High / Medium / Low
- **Location**: file, pipeline stage, or component (if applicable)
- **Description**: what the issue is
- **Risk**: what an attacker or adversary can do
- **Remediation**: concrete fix, control, or architectural change

Group by severity (Critical → High → Medium → Low). End with a summary table: category → finding count → highest severity.
