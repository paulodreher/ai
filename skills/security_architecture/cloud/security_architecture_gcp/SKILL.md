---
name: security-architecture-gcp
description: Review and analyze Google Cloud (GCP) security architecture. Use when the user wants to assess GCP infrastructure, IAM bindings, service configurations, or cloud-native security controls for risks and misconfigurations.
---

Perform a security architecture review of the provided GCP infrastructure, design, or configuration.

Focus on:
1. **IAM & least privilege**: Basic roles (Owner/Editor) vs predefined vs custom roles, service account key files, Workload Identity Federation, cross-project bindings, domain-wide delegation, unused accounts
2. **Network exposure**: Firewall rules (0.0.0.0/0 ingress), default network usage, external IPs on VMs, Private Google Access, VPC Service Controls perimeters, Cloud Armor policies
3. **Data protection**: GCS bucket IAM (allUsers/allAuthenticatedUsers), CMEK vs Google-managed encryption, Cloud KMS key rotation, object versioning and retention policies, BigQuery dataset access
4. **Secrets management**: Secret Manager usage vs environment variables/metadata, secret versions and rotation, service account key rotation, avoiding JSON key files in favor of Workload Identity
5. **Logging & detection**: Cloud Audit Logs (Admin Activity, Data Access, System Event), log sinks to Cloud Storage/BigQuery, Security Command Center findings, anomaly detection
6. **Service hardening**: Cloud Run/Cloud Functions service account permissions, GKE Workload Identity, Binary Authorization, Artifact Registry vulnerability scanning, Pub/Sub access controls
7. **Organization & boundary controls**: Organization Policy constraints, resource hierarchy (org → folder → project), VPC Service Controls, assured workloads, Shared VPC design
8. **Resilience & blast radius**: Single-region deployments, missing PITR/backups on Cloud SQL, overly broad project-level bindings, lack of resource-level conditions

Map findings to the Google Cloud Security Foundations Blueprint and CIS Google Cloud Platform Foundations Benchmark where applicable.

Output a prioritized findings list: Critical → High → Medium → Low. For each finding include: what it is, why it matters, the specific GCP resource/service affected, and a concrete remediation with example IAM binding, gcloud command, or policy snippet.
