# 📘 Day 09 — CI/CD (GitHub Actions + Azure DevOps)

---

## 🎯 Objective

Automate multi-cloud infrastructure deployment using:
- GitHub Actions
- Azure DevOps (conceptual)

By the end of this lab, you will:
- Build CI/CD pipelines
- Automate Terraform workflows
- Implement DevSecOps practices
- Enforce infrastructure validation before deployment
- Understand enterprise pipeline architecture

---

## 🧠 Concept (Think Like an Architect)

### 🏭 Analogy: Automated Factory

- GitHub = Control system
- Pipeline = Assembly line
- Terraform = Machine building infrastructure
- Engineers = Operators
- Approval gates = Quality control

👉 No manual deployment — everything is automated

---

## 🏗️ Architecture

```mermaid
flowchart TD
    A[Developer Commit] --> B[GitHub Repo]
    B --> C[GitHub Actions Pipeline]
    C --> D[Terraform Plan]
    D --> E[Approval Gate]
    E --> F[Terraform Apply]
    F --> G[Cloud Infrastructure]
🧠 CI/CD Stages
Stage	Purpose
Validate	Check syntax
Plan	Preview changes
Approve	Manual review
Apply	Deploy infrastructure
🧪 Lab Step 1 — Create GitHub Actions Directory
mkdir -p .github/workflows
🧪 Lab Step 2 — Create Pipeline File
nano .github/workflows/terraform.yml
📄 GitHub Actions Pipeline
name: Terraform CI/CD

on:
  push:
    branches:
      - main

jobs:
  terraform:
    name: Terraform Workflow
    runs-on: ubuntu-latest

    steps:
      - name: Checkout Code
        uses: actions/checkout@v3

      - name: Setup Terraform
        uses: hashicorp/setup-terraform@v2

      - name: Terraform Init
        run: terraform init
        working-directory: ./terraform

      - name: Terraform Validate
        run: terraform validate
        working-directory: ./terraform

      - name: Terraform Plan
        run: terraform plan
        working-directory: ./terraform

      # Optional Apply Step (disable for safety)
      # - name: Terraform Apply
      #   run: terraform apply -auto-approve
🔐 Lab Step 3 — Configure Secrets

Go to GitHub:

Settings → Secrets → Actions

Add:

AWS_ACCESS_KEY_ID
AWS_SECRET_ACCESS_KEY
ARM_CLIENT_ID
ARM_CLIENT_SECRET
ARM_SUBSCRIPTION_ID
ARM_TENANT_ID
GCP credentials (if needed)
🧠 Key Concept — Secure Pipelines

👉 NEVER hardcode credentials
👉 ALWAYS use secrets

🧪 Lab Step 4 — Trigger Pipeline
git add .
git commit -m "Add CI/CD pipeline"
git push

👉 This triggers GitHub Actions automatically

🧪 Lab Step 5 — View Pipeline

Go to GitHub:

Click Actions tab
Watch pipeline execution
🧠 Key Concept — Shift Left Security

Security happens:

BEFORE deployment (not after)

Examples:

Terraform validation
Policy checks
Static analysis
🔥 Azure DevOps (Concept)

Pipeline stages:

Build
Test
Deploy
Approvals
🧪 Example Azure DevOps Pipeline
trigger:
- main

pool:
  vmImage: 'ubuntu-latest'

steps:
- script: terraform init
- script: terraform plan
- script: terraform apply
🧠 Advanced Concept — Approval Gates

Enterprise flow:

Dev → Plan → Security Review → Approval → Deploy

👉 Prevents unauthorized changes

🧠 DevSecOps Integration
Area	Tool
IaC	Terraform
CI/CD	GitHub Actions
Security	Policies + Scanning
Monitoring	SIEM
🧪 Lab Step 6 — Add Branch Protection (Concept)
Require PR approval
Require pipeline success
Block direct pushes to main
🚨 Troubleshooting
Pipeline fails at init
Check Terraform path
Check working directory
Auth errors
Verify secrets
Check permissions
Plan fails
Syntax issues
Missing variables
✅ Validation Checklist
 GitHub Actions workflow created
 Pipeline runs successfully
 Terraform init works
 Terraform validate works
 Secrets configured
 Pipeline visible in GitHub
🎯 Key Takeaways
CI/CD = automation backbone
GitHub Actions = powerful pipeline tool
Terraform integrates seamlessly
Security must be embedded early
Pipelines reduce human error
🚀 Next Step

➡️ Day 10 — Operations, Incident Response & Capstone

You will:

Troubleshoot real issues
Build runbooks
Simulate incidents
Present final architecture
