# devSecOps-Security-pipeline Engine.

> **A modular, plug-and-play GitHub Actions DevSecOps suite providing automated Secret Scanning, SAST, SCA, Container Vulnerability Analysis, and IaC Security Auditing.**

`devsecops-security-pipeline` is a centralized security automation suite designed to shift security left across the entire Software Development Life Cycle (SDLC). Built using GitHub Actions **Reusable Workflows** (`workflow_call`), this repository allows developers and security teams to call pre-configured, battle-tested security scanners directly into their own repositories with minimal configuration.

---

## 📁 Repository Overview

| Directory / File | Description |
| :--- | :--- |
| **`.github/workflows/security_audit.yml`** | Core pipeline for Secrets, Code Vulnerabilities, Dependencies, and Shell Scripts. |
| **`.github/workflows/container_scan.yml`** | Container security pipeline for scanning Docker images and OS layers. |
| **`.github/workflows/iac_scan.yml`** | Infrastructure as Code security engine for cloud configurations. |
| **`LICENSE`** | MIT Open Source License. |
| **`README.md`** | Complete documentation and integration guides. |

---

## 🚀 Available Workflows & Security Engines

### 1. Core Security Audit (`security_audit.yml`)
> *Scans application code, embedded credentials, third-party packages, and scripts.*

| Security Gate | Tool Used | Target / Function |
| :--- | :--- | :--- |
| **🔑 Secret Scanning** | [TruffleHog](https://github.com/trufflesecurity/trufflehog) | Detects exposed API keys, private SSH keys, and cloud credentials in git history. |
| **🔍 SAST** | [Semgrep](https://semgrep.dev/) | Scans application code for OWASP Top 10 vulnerabilities (SQLi, XSS, etc.). |
| **📦 SCA** | [Trivy](https://github.com/aquasecurity/trivy) | Audits dependencies and libraries for published CVE vulnerabilities. |
| **🐚 Shell Audit** | [ShellCheck](https://www.shellcheck.net/) | Audits Bash/Shell scripts for syntax flaws and bad practices. |

---

### 2. Specialized Scanning Workflows

| Workflow File | Security Gate | Tool Used | Target / Function |
| :--- | :--- | :--- | :--- |
| **`container_scan.yml`** | **🐳 Container Security** | [Trivy](https://github.com/aquasecurity/trivy) | Analyzes Docker container images and underlying OS layers for High and Critical severity CVEs. |
| **`iac_scan.yml`** | **☁️ IaC Security** | [Checkov](https://github.com/bridgecrewio/checkov) | Scans cloud infrastructure code (Terraform, AWS CloudFormation, Kubernetes manifests, Dockerfiles) for misconfigurations and compliance violations. |

---

## 📊 Pipeline Architecture

graph TD
    A[Code Push / Pull Request] --> B[Target Repository]
    B --> C{DevSecOps Security Gate}
    
    C -->|security_audit.yml| D[Secret Scan: TruffleHog]
    C -->|security_audit.yml| E[SAST: Semgrep]
    C -->|security_audit.yml| F[SCA: Trivy FS]
    C -->|security_audit.yml| G[Shell Audit: ShellCheck]
    
    C -->|iac_scan.yml| H[IaC Misconfig: Checkov]
    C -->|container_scan.yml| I[Container CVEs: Trivy]
    
    D --> J{Pass Security Standards?}
    E --> J
    F --> J
    G --> J
    H --> J
    I --> J
    
    J -->|Pass| K[Approve Build / Merge]
    J -->|Fail| L[Block Pipeline & Alert Developer]




## 🛠️ Integration Guide (How to Use)

To implement these security gates in your own projects, create a workflow file in your target repository at `.github/workflows/security_gate.yml`.

### Option A: Run the Full Security Suite (Recommended)
> *Call all three scanning pipelines in parallel:*

| Workflow Reference | Parameter / Requirement | Description |
| :--- | :--- | :--- |
| **`security_audit.yml`** | None | Scans source code, secrets, dependencies, and shell scripts. |
| **`iac_scan.yml`** | None | Scans Terraform, Kubernetes, Dockerfiles, and CloudFormation. |
| **`container_scan.yml`** | `image-name` *(Required)* | Scans built Docker images (e.g., `'my-app:latest'`). |

```yaml

name: DevSecOps Gate Check

on:
  push:
    branches: [ "main", "master" ]
  pull_request:
    branches: [ "main", "master" ]

jobs:
  # 1. Code, Secret, & Script Audit
  application-security:
    uses: gabbytech01/devsecops-security-pipeline/.github/workflows/security_audit.yml@main

  # 2. Infrastructure as Code (IaC) Scan
  infrastructure-security:
    uses: gabbytech01/devsecops-security-pipeline/.github/workflows/iac_scan.yml@main

  # 3. Docker Container Scan
  container-security:
    uses: gabbytech01/devsecops-security-pipeline/.github/workflows/container_scan.yml@main
    with:
      image-name: 'my-app:latest'


### Option B: Run Modular Workflows Individually

If you only need specific security gates (e.g., only infrastructure auditing), call only the workflow you require:

name: IaC Security Only

on:
  push:
    branches: [ "main" ]

jobs:
  iac-check:
    uses: gabbytech01/devsecops-security-pipeline/.github/workflows/iac_scan.yml@main
