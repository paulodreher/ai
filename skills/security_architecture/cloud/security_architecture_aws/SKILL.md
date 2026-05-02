---
name: security-architecture-aws
description: Review and analyze AWS security architecture. Use when the user wants to assess AWS infrastructure, IAM policies, service configurations, or cloud-native security controls for risks and misconfigurations.
---

Perform a security architecture review of the provided AWS infrastructure, design, or configuration.

Focus on:
1. **IAM & least privilege**: Overly permissive policies, wildcard actions/resources, missing permission boundaries, cross-account trust, role chaining, unused credentials
2. **Network exposure**: Public subnets, security group rules (0.0.0.0/0), NACLs, VPC peering, exposed endpoints, missing PrivateLink
3. **Data protection**: S3 bucket policies (public access, ACLs), encryption at rest (KMS key policies, CMK vs AWS-managed), encryption in transit, unencrypted EBS/RDS/DynamoDB
4. **Secrets management**: Hardcoded credentials, Secrets Manager vs SSM Parameter Store usage, secret rotation, EC2 instance profile vs long-term access keys
5. **Logging & detection**: CloudTrail (multi-region, log file validation, S3 access logging), VPC Flow Logs, GuardDuty, Security Hub, Config rules enabled
6. **Service hardening**: Lambda function permissions and VPC config, ECS/EKS task roles, API Gateway authentication, Cognito configuration, SNS/SQS access policies
7. **Account & boundary controls**: SCPs, AWS Organizations structure, account separation (prod/dev), resource policies vs identity policies
8. **Resilience & blast radius**: Single-AZ deployments, missing backups, overly broad resource tagging, lack of resource-based deny policies

Map findings to the AWS Well-Architected Framework Security Pillar and CIS AWS Foundations Benchmark where applicable.

Output a prioritized findings list: Critical → High → Medium → Low. For each finding include: what it is, why it matters, the specific AWS resource/service affected, and a concrete remediation with example policy or CLI fix.
