# AI Skills

A collection of Claude Code skills focused on security, architecture review, and governance frameworks. These skills extend Claude Code with specialized capabilities that can be invoked via slash commands during development and security workflows.

## Structure

```
skills/
├── governance/
│   └── nist/
│       ├── nist-csf/          # NIST Cybersecurity Framework 2.0 assessment
│       ├── nist-privacy/      # NIST Privacy Framework 1.0 assessment
│       ├── nist-rmf/          # NIST Risk Management Framework (SP 800-37)
│       └── nist-rmf-ai/       # NIST AI Risk Management Framework (AI RMF 1.0)
├── security_architecture/
│   ├── SKILL.md               # General security architecture review
│   └── cloud/
│       ├── security_architecture_aws/    # AWS security architecture review
│       ├── security_architecture_azure/  # Azure security architecture review
│       └── security_architecture_gcp/   # GCP security architecture review
└── security_code/
    ├── SKILL.md               # General security code review
    ├── container_scanning/    # Dockerfile and container runtime security
    ├── iac_scanning/          # Terraform, CloudFormation, CDK, Kubernetes security
    ├── unit_test/             # Unit test generation (10+ languages)
    └── owasp/
        ├── owasp-aisvs/       # OWASP AI Security Verification Standard
        ├── owasp-asvs/        # OWASP Application Security Verification Standard
        ├── owasp-masvs/       # OWASP Mobile Application Security Verification Standard
        ├── owasp-top10/       # OWASP Top 10 (web applications)
        ├── owasp-top10-ai/    # OWASP ML Security Top 10
        ├── owasp-top10-api/   # OWASP API Security Top 10
        ├── owasp-top10-llm/   # OWASP Top 10 for LLM Applications
        └── owasp-top10-mobile # OWASP Mobile Top 10
```

## Skill Categories

### Governance
Structured assessments against NIST frameworks — covering cybersecurity risk management, privacy risk, RMF authorization readiness, and AI-specific risk evaluation.

### Security Architecture
Review system designs, cloud infrastructure (AWS, Azure, GCP), IAM policies, trust boundaries, and architectural vulnerabilities through threat modeling.

### Security Code
Audit source code, containers, and infrastructure-as-code against OWASP standards and security best practices. Includes dedicated skills for web, mobile, API, LLM, and AI/ML applications.

### Unit Testing
Generate and improve unit tests across C++, C#, Go, Java, JavaScript, PHP, Python, Ruby, Rust, and TypeScript.

## Usage

Skills are invoked as slash commands inside Claude Code:

```
/security_architecture
/security_code
/owasp-top10
/nist-csf
```

## Setup

To use these skills, symlink the `skills/` directory to `~/.claude/skills/`:

```bash
ln -s /path/to/ai/skills ~/.claude/skills
```
