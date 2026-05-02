---
name: owasp-top10-llm
description: Review LLM-integrated applications against the OWASP Top 10 for LLM Applications (2025 v2.0). Use when the user wants to audit prompt handling, RAG pipelines, agent architectures, plugin systems, or LLM output processing for AI-specific risks.
---

Perform a security review mapped to the OWASP Top 10 for LLM Applications (2025 v2.0).

Assess each category in turn, then report only categories where findings exist:

1. **LLM01 – Prompt Injection**: Direct injection (user input overrides system prompt), indirect injection (malicious content in retrieved documents/tools/emails), missing input/output sanitization, no separation between data and instruction planes
2. **LLM02 – Sensitive Information Disclosure**: PII or confidential data leaking through model outputs, system prompt extraction via adversarial queries, training data memorization exposure, no output filtering for sensitive patterns
3. **LLM03 – Supply Chain Vulnerabilities**: Untrusted pre-trained or fine-tuned models, unverified plugins/extensions, third-party dataset provenance, missing integrity checks on model artifacts and embeddings
4. **LLM04 – Data and Model Poisoning**: Unvalidated content entering RAG datastores, adversarial documents that influence retrieval, fine-tuning on untrusted data, no data quality gates or anomaly detection
5. **LLM05 – Improper Output Handling**: LLM output passed directly to SQL/shell/HTML/code interpreters without sanitization, XSS via rendered markdown, SSRF via LLM-generated URLs, second-order injection
6. **LLM06 – Excessive Agency**: Over-permissioned tools/plugins (write access when read is sufficient), autonomous actions without human approval, missing scope constraints on function calls, agents that can modify their own instructions
7. **LLM07 – System Prompt Leakage**: System prompt exposed via direct extraction, verbose error messages, or indirect inference; missing confidentiality controls; no prompt hardening against leakage
8. **LLM08 – Vector and Embedding Weaknesses**: Poisoned embeddings in vector stores, cross-tenant data leakage in shared RAG, no access control on retrieved context, adversarial documents crafted to manipulate similarity search
9. **LLM09 – Misinformation**: LLM outputs used in decisions without grounding or citation, no hallucination detection, absent human-in-the-loop for high-stakes outputs, over-reliance on LLM for factual claims
10. **LLM10 – Unbounded Consumption**: No rate limiting on inference endpoints, recursive or chained agent calls without depth/iteration caps, missing token budget enforcement, cost-exhaustion via prompt flooding

For each finding report:
- **Category**: LLMxx – Name
- **Severity**: Critical / High / Medium / Low
- **Location**: component, file, or pipeline stage (if applicable)
- **Description**: what the issue is
- **Risk**: what an attacker or adversary can do
- **Remediation**: concrete fix, guardrail, or architectural change

Group by severity (Critical → High → Medium → Low). End with a summary table: category → finding count → highest severity.
