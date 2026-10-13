# devsecops-security-pipeline engine

> **A modular, plug-and-play GitHub Actions DevSecOps suite providing automated Secret Scanning, SAST, SCA, Container Vulnerability Analysis, and IaC Security Auditing.**

`devsecops-security-pipeline` is a centralized security automation suite designed to shift security left across the entire Software Development Life Cycle (SDLC). 

Built using GitHub Actions **Reusable Workflows** (`workflow_call`), this repository allows developers and security teams to call pre-configured, battle-tested security scanners directly into their own repositories with minimal configuration.

---

## 📁 Repository Structure

```text
devsecops-security-pipeline/
├── .github/
│   └── workflows/
│       ├── security_audit.yml   # Core SAST, Secret, SCA, & Shell Audit
│       ├── container_scan.yml   # Docker Container Image Vulnerability Scanner
│       └── iac_scan.yml         # Terraform, CloudFormation, & K8s Security Scan
├── LICENSE                      # MIT Open Source License
└── README.md                    # Engine Documentation
