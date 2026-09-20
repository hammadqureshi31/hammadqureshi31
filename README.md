<div align="center">

# Muhammad Hammad Qureshi

### DevOps / Cloud Engineer

Software Engineer with production experience, focused on cloud infrastructure, Kubernetes, CI/CD, DevSecOps, and observability.

[![Email](https://img.shields.io/badge/Email-muhammadhammadq882%40gmail.com-D14836?style=flat-square&logo=gmail&logoColor=white)](mailto:muhammadhammadq882@gmail.com)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-muhammad--hammad--qureshi-0A66C2?style=flat-square&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/muhammad-hammad-qureshi-mhq)
[![GitHub](https://img.shields.io/badge/GitHub-hammadqureshi31-181717?style=flat-square&logo=github&logoColor=white)](https://github.com/hammadqureshi31)
[![Portfolio](https://img.shields.io/badge/Portfolio-View_Site-000000?style=flat-square&logo=googlechrome&logoColor=white)](https://hammadqureshi31.github.io/muhammadhammadqureshi/)

</div>

---

## About

I have a software-engineering background — building and maintaining backend and web applications — and my work has since moved into DevOps and cloud engineering. At **Innova360**, my responsibilities as a Software Engineer extended into AWS infrastructure, CI/CD, blue-green deployment automation, database operations, and production observability.

Alongside that production experience, I build and document independent projects that go deeper into specific parts of the DevOps stack: provisioning cloud infrastructure with Terraform, operating Kubernetes on Amazon EKS, securing CI/CD pipelines, and automating multi-OS server configuration. A consistent thread across all of it is **deliberately breaking things and recovering them** — I'd rather understand why a system fails than only show that it can succeed once.

---

## Technical Focus

**Cloud**
`AWS` `EC2` `EKS` `ECR` `RDS` `VPC` `IAM` `S3` `ALB`

**Containers & Orchestration**
`Docker` `Kubernetes` `Docker Compose` `Kind`

**Infrastructure & Automation**
`Terraform` `Ansible` `Linux` `Bash`

**CI/CD**
`GitHub Actions` `Jenkins`

**DevSecOps**
`Trivy` `Gitleaks` `Semgrep` `SonarQube` `OWASP Dependency-Check` `Syft (SBOM)` `Cosign`

**Observability**
`Prometheus` `Grafana` `Loki`

**Development**
`TypeScript` `Node.js` `NestJS` `Python` `PostgreSQL` `Prisma`

---

## Selected DevOps / Cloud Projects

### [OpenAI Chatbot on Amazon EKS](https://github.com/hammadqureshi31/openai-chatbot-eks)
A production-style deployment of a Next.js chatbot onto Amazon EKS — infrastructure provisioned with Terraform and delivered through a Jenkins CI/CD pipeline with integrated security gates.

`Terraform` `AWS EKS` `Jenkins` `Trivy` `SonarQube` `OWASP Dependency-Check` `Kubernetes RBAC`

- Multi-AZ EKS cluster with private worker nodes, provisioned entirely through Terraform (VPC + EKS modules)
- Jenkins pipeline enforcing quality gates and vulnerability scanning before any image reaches EKS, reducing findings from 36 to 0 HIGH/CRITICAL by fixing root causes rather than suppressing them
- Jenkins' cluster access redesigned from `cluster-admin` down to a namespace-scoped, least-privilege Kubernetes Role
- Automated rollback validated against a real, deliberately triggered failed rollout (`ImagePullBackOff`), with the pipeline still correctly reporting the build as failed after recovery

### [Cloud-Native Monitoring on Kubernetes & EKS](https://github.com/hammadqureshi31/cloud-native-monitoring)
A monitored Flask workload taken from a local Kind cluster to Amazon EKS, with Prometheus/Grafana observability, HPA autoscaling, an ALB ingress path, and Kubernetes security controls.

`Kubernetes` `AWS EKS` `Prometheus` `Grafana` `HPA` `RBAC` `NetworkPolicy` `EKS Pod Identity`

- Full observability pipeline (Prometheus, ServiceMonitor, Grafana) separate from the Metrics Server → HPA autoscaling loop (2 → 5 replicas under load)
- AWS Load Balancer Controller authenticated via EKS Pod Identity rather than static AWS credentials
- RBAC, NetworkPolicy, and PodDisruptionBudget applied and actively tested, not just configured
- 17 deliberately injected failures diagnosed and recovered — including a node-drain-vs-PDB scheduling incident and a NetworkPolicy that was applied but silently unenforced

### [Supply Chain Security Lab](https://github.com/hammadqureshi31/supply-chain-security)
A DevSecOps pipeline that secures a Node.js container from source to signed artifact — secret scanning, SCA, SAST, container scanning, SBOM generation, and keyless signing, fully automated in GitHub Actions.

`GitHub Actions` `Trivy` `Gitleaks` `Semgrep` `Syft` `Cosign` `GHCR`

- Sequential security gates (Gitleaks → npm audit → Semgrep → Trivy) block publication on any failure
- Investigated a discrepancy between a clean `npm audit` and 4 HIGH Trivy findings, traced them to unused base-image tooling, and removed it — reaching 0 HIGH/CRITICAL rather than suppressing the scan
- SPDX SBOM generated with Syft and published as a build artifact for every release
- Images signed with Cosign using GitHub OIDC identity — no long-lived signing key stored in CI

### [Cross-OS Infrastructure Automation](https://github.com/hammadqureshi31/cross-os-automation)
Reusable AWS infrastructure provisioning with Terraform paired with cross-platform server configuration in Ansible, targeting both Ubuntu and Amazon Linux.

`Terraform` `Ansible` `AWS` `Ubuntu` `Amazon Linux`

- Modular Terraform (VPC, EC2, Security Group modules) across separate dev/staging environments with encrypted, locked remote state in S3
- Ansible's AWS EC2 dynamic inventory discovers infrastructure by tag — no hand-maintained host lists
- One Ansible role configures both Ubuntu (`apt`) and Amazon Linux (`dnf`) hosts to the same desired state
- Idempotency and failure/recovery explicitly validated, including a state-safe Terraform refactor using `moved` blocks

---

## Production Engineering — Innova360

Separate from the projects above, this reflects production experience as a **Software Engineer at Innova360**, where responsibilities extended into infrastructure, deployment automation, and operations for a live multi-tenant SaaS backend.

- **CI/CD & deployment:** GitHub Actions pipeline building and versioning Docker images (timestamp + Git SHA) to Amazon ECR, deployed to EC2 through a blue-green workflow with health-gated traffic switching via Nginx and automated rollback on failure
- **Database operations:** PostgreSQL with a database-per-tenant model, each tenant on a dedicated role; Prisma migrations run through a validate → backup → migrate → verify sequence, with migration troubleshooting and per-tenant diagnostics
- **Observability:** contributed to a Prometheus/Grafana/Loki stack used for real incident investigation — metrics to spot abnormal behavior, logs to trace root cause
- **Automation:** Bash tooling for deployment orchestration, database backup/recovery, tenant provisioning, and diagnostics, designed around explicit validation and failure handling rather than ad-hoc scripts

---

## Engineering Principles

| Principle | In Practice |
|---|---|
| **Security is part of delivery** | Scanning and gating happen before an artifact reaches a registry or cluster, not after |
| **Least privilege by default** | CI/CD and workload permissions are scoped to what's actually required, iterated down from broad defaults |
| **Infrastructure should be reproducible** | Terraform and Ansible define infrastructure and configuration instead of manual changes |
| **Deployments need verification, not assumption** | Rollout status and health checks are treated as the real signal of success, not `apply` succeeding |
| **Failures should be tested, not just handled in theory** | Recovery paths (rollback, backup restore) are validated against real, deliberately triggered failures |
| **Systems should be observable** | Metrics and logs are built in from the start, not added after an incident |
