---
name: container-scanning
description: Scan container configurations for security vulnerabilities. Use when the user wants a security audit of Dockerfiles, docker-compose files, container runtime configs, or wants to assess container image hygiene.
---

Perform a security scan of the provided container configuration files and image definitions.

Focus on:
1. **Running as root**: Missing `USER` directive in Dockerfile, `runAsNonRoot: false` or absent in K8s securityContext
2. **Base image hygiene**: Use of `:latest` or mutable tags, bloated base images with large attack surface, images without known-good provenance
3. **Secrets baked into images**: `ENV`, `ARG`, or `RUN` steps that embed credentials, tokens, or private keys into image layers
4. **Excessive capabilities and privileges**: `--privileged` flag, unnecessary `CAP_ADD` entries, missing `CAP_DROP: ALL`, `allowPrivilegeEscalation: true`
5. **Exposed attack surface**: Unnecessary ports exposed (`EXPOSE`), bind mounts to sensitive host paths (`/var/run/docker.sock`, `/etc`, `/proc`)
6. **Missing resource limits**: No CPU/memory limits (enables noisy-neighbor and DoS), no `pids_limit`
7. **Read-only filesystem**: Writable root filesystem where not required; missing `readOnlyRootFilesystem: true`
8. **Network isolation**: Containers sharing host network (`network_mode: host`), missing network segmentation between services
9. **Image provenance and integrity**: Unsigned images, missing digest pinning, no image scanning step in CI
10. **Multi-stage build misuse**: Secrets or build artifacts accidentally copied from build stage to final image

**Supported formats**: Dockerfile, docker-compose (v2/v3), Kubernetes Pod/Deployment specs, Helm chart templates, container runtime configs (containerd, CRI-O).

For each finding report:
- **Severity**: Critical / High / Medium / Low
- **File and line** (if applicable)
- **Description**: what the misconfiguration is
- **Risk**: what an attacker could do if exploited
- **Remediation**: a corrected snippet or concrete fix

Group findings by severity (Critical → High → Medium → Low) and end with a summary count per severity.
