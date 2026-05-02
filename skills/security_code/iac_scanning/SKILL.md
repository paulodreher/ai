---
name: iac-scanning
description: Scan Infrastructure as Code for security misconfigurations. Use when the user wants a security audit of Terraform, CloudFormation, CDK, Kubernetes manifests, Helm charts, or Ansible playbooks.
---

Perform a security scan of the provided Infrastructure as Code (IaC) files.

Focus on:
1. **Overly permissive IAM/RBAC**: Wildcard actions or resources, admin roles granted broadly, missing least-privilege
2. **Network exposure**: Open ingress rules (0.0.0.0/0), public-facing resources that should be private, missing egress restrictions
3. **Encryption at rest and in transit**: Unencrypted storage volumes/buckets/databases, missing TLS enforcement, weak cipher configurations
4. **Secrets in plaintext**: Hardcoded passwords, API keys, tokens in variables, environment blocks, or parameter defaults
5. **Privileged workloads**: Containers running as root, `privileged: true`, hostPID/hostNetwork/hostIPC enabled, excessive Linux capabilities
6. **Missing security controls**: Absent resource limits, no pod security policies/admission controllers, logging/auditing disabled
7. **Public resource exposure**: S3 buckets with public ACLs, publicly accessible databases, unrestricted blob storage
8. **Supply chain risk**: Unpinned module versions, mutable image tags (`:latest`), missing image digest pinning
9. **State and secret management**: Unencrypted Terraform state backends, secrets stored outside a secrets manager

**Supported formats**: Terraform (.tf), CloudFormation / SAM (YAML/JSON), AWS CDK, Kubernetes manifests, Helm charts, Ansible playbooks, Bicep, Pulumi.

For each finding report:
- **Severity**: Critical / High / Medium / Low
- **File and line** (if applicable)
- **Description**: what the misconfiguration is
- **Risk**: what an attacker could do if exploited
- **Remediation**: a corrected code snippet or concrete fix

Group findings by severity (Critical → High → Medium → Low) and end with a summary count per severity.
