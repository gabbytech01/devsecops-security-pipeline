# devsecops-security-pipeline

> **Automated, plug-and-play GitHub Actions workflow for secret scanning, SAST, SCA, and shell script auditing.**

`devsecops-security-pipeline` is a centralized, reusable CI/CD security gate designed to shift security left in the Software Development Life Cycle (SDLC). By embedding industry-standard open-source security tools into your workflow, this pipeline automatically catches hardcoded credentials, code vulnerabilities, insecure dependencies, and bad scripting practices on every `push` and `pull_request`.

---

## 🚀 Key Features & Security Gates

This pipeline runs four parallel security checks using top-tier DevSecOps engines:

| Security Gate | Tool Used | Description |
| :--- | :--- | :--- |
| **🔑 Secret Scanning** | [TruffleHog](https://github.com/trufflesecurity/trufflehog) | Scans git history for exposed API keys, database credentials, AWS keys, and private SSH keys. |
| **🔍 SAST (Static Analysis)** | [Semgrep](https://semgrep.dev/) | Scans application source code for security flaws (OWASP Top 10, SQLi, XSS, insecure logic). |
| **📦 SCA (Dependency Audit)** | [Trivy](https://github.com/aquasecurity/trivy) | Analyzes software composition to identify known CVE vulnerabilities in dependencies and filesystems. |
| **🐚 Shell Script Audit** | [ShellCheck](https://www.shellcheck.net/) | Audits Bash/Shell scripts for syntax flaws, unquoted variables, and unsafe operations. |

---

## 🛠️ How to Implement (Quick Start)

Because this repository uses GitHub Actions' **Reusable Workflows** (`workflow_call`), you do not need to copy or paste complex security code into your projects. 

Simply add a tiny 10-line workflow file to any repository you want to protect.

### Step 1: Create the Workflow File
In **your target repository**, create a file at `.github/workflows/security.yml`.

### Step 2: Add the Integration Code
Paste the following YAML into `.github/workflows/security.yml`:

```yaml
name: Security Audit Gate

on:
  push:
    branches: [ "main", "master" ]
  pull_request:
    branches: [ "main", "master" ]

jobs:
  run-devsecops-pipeline:
    # Calls this centralized reusable workflow directly
    uses: gabbytech01/devsecops-security-pipeline/.github/workflows/security_audit.yml@main

📁 Repository Structure
Plaintext

devsecops-security-pipeline/
├── .github/
│   └── workflows/
│       └── security_audit.yml   # The core reusable workflow engine
└── README.md                    # Project documentation


📊 Pipeline Execution Workflow
Code snippet

graph TD
    A[Code Push / Pull Request] --> B[GitHub Actions Runner]
    B --> C[Secret Scan: TruffleHog]
    B --> D[SAST Scan: Semgrep]
    B --> E[SCA Scan: Trivy]
    B --> F[Shell Audit: ShellCheck]
    C --> G{Security Gate Passed?}
    D --> G
    E --> G
    F --> G
    G -->|Yes| H[Build & Deploy Approved]
    G -->|No| I[Pipeline Fails / Alert Triggered]

🤝 Contributing & Customization

Contributions, issues, and feature requests are welcome! Feel free to check the issues page if you want to extend rule sets or add additional security scanners (e.g., container or IaC scanning).


📄 License

This project is licensed under the MIT License - see the LICENSE file for details.

---

<Elicitations message="What would you like to do next?">
  <Elicitation label="Create security_audit.yml file" query="Show me the full security_audit.yml workflow file to commit into the repository."/>
  <Elicitation label="Push repository to GitHub" query="Give me the step-by-step terminal commands to push this project to GitHub."/>
</Elicitations>
