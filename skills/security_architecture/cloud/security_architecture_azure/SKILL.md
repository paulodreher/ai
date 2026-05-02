---
name: security-architecture-azure
description: Review and analyze Azure security architecture. Use when the user wants to assess Azure infrastructure, RBAC assignments, service configurations, or cloud-native security controls for risks and misconfigurations.
---

Perform a security architecture review of the provided Azure infrastructure, design, or configuration.

Focus on:
1. **Identity & access control**: Overly broad RBAC roles (Owner/Contributor at subscription scope), missing Privileged Identity Management (PIM), service principal credential hygiene, Managed Identity vs client secrets, guest account access, Conditional Access policies
2. **Network exposure**: NSG rules (any-source ingest), public endpoints on PaaS services (Storage, Key Vault, SQL), missing Private Endpoints, Azure Firewall / NVA placement, DDoS protection tiers
3. **Data protection**: Storage account public access, Blob/Queue/Table service encryption (CMK via Key Vault), TLS version enforcement, Azure SQL TDE and TLS, Key Vault soft-delete and purge protection
4. **Secrets management**: Key Vault access policies vs RBAC, secret/certificate rotation, Managed Identity usage vs connection strings, avoiding secrets in App Settings/environment variables
5. **Logging & detection**: Diagnostic settings on resources, Azure Monitor / Log Analytics workspace, Microsoft Defender for Cloud (secure score, recommendations), Sentinel analytics rules, Activity Log retention
6. **Service hardening**: App Service authentication, Function App managed identity and network restrictions, AKS workload identity and Azure Policy, Container Registry vulnerability scanning, API Management policies
7. **Subscription & governance controls**: Azure Policy assignments, Management Group hierarchy, Blueprints/Landing Zone, resource locks (CanNotDelete/ReadOnly), subscription-level Defender plans enabled
8. **Resilience & blast radius**: Single-region deployments, missing geo-redundant backups, no resource locks on critical resources, broad subscription-scope assignments

Map findings to the Microsoft Azure Security Benchmark (ASB) and CIS Microsoft Azure Foundations Benchmark where applicable.

Output a prioritized findings list: Critical → High → Medium → Low. For each finding include: what it is, why it matters, the specific Azure resource/service affected, and a concrete remediation with example ARM/Bicep snippet, az CLI command, or policy definition.
