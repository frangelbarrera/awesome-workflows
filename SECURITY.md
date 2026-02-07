# 🔒 Security Policy

## 🛡️ Security at Awesome GitHub Workflows

This document outlines the security practices, reporting procedures, and guidelines for contributing secure workflows to this repository.

---

## 📋 Table of Contents

- [Reporting Security Vulnerabilities](#-reporting-security-vulnerabilities)
- [Security Best Practices](#-security-best-practices)
- [Required Security Checks](#-required-security-checks)
- [Sensitive Data Handling](#-sensitive-data-handling)
- [Dependency Management](#-dependency-management)
- [Audit and Compliance](#-audit-and-compliance)

---

## 📢 Reporting Security Vulnerabilities

### Responsible Disclosure

We take the security of this repository seriously. If you discover a security vulnerability, please follow responsible disclosure practices:

1. **Do NOT** create a public issue
2. **Do NOT** post about the vulnerability on social media
3. **Do NOT** share the vulnerability with others publicly

### How to Report

| Method | Contact |
|--------|---------|
| Email | frangelrcbarrera@gmail.com |
| GitHub Security Advisory | [Report via GitHub](https://github.com/frangelbarrera/awesome-workflows/security/advisories/new) |
| Private Fork | Create a private fork to demonstrate the issue |

### What to Include

When reporting, please provide:

```
- Description of the vulnerability
- Steps to reproduce
- Potential impact
- Suggested fix (if any)
- Your contact information (optional)
```

### Response Timeline

| Timeline | Action |
|----------|--------|
| 24 hours | Acknowledge receipt |
| 7 days | Initial assessment |
| 30 days | Resolution and release (if applicable) |

---

## 🛡️ Security Best Practices

### 1. Action Pinning

**Always pin actions to their full SHA commit hash.**

```yaml
# ❌ Bad - Using a tag (can be hijacked)
uses: actions/checkout@v4

# ✅ Good - Using full SHA (verifiable)
uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683  # v4.2.2
```

### Why SHA Pinning?

| Risk | Without SHA Pinning | With SHA Pinning |
|------|---------------------|------------------|
| Supply chain attack | ⚠️ Vulnerable | ✅ Protected |
| Tag deletion | ⚠️ Breaks workflow | ✅ Stable |
| Tag hijacking | ⚠️ Possible | ✅ Prevented |

### 2. Least Privilege Permissions

**Grant only the permissions your workflow needs.**

```yaml
# ✅ Good - Minimal permissions
permissions:
  contents: read    # Only read repository contents
  id-token: write    # Only for OIDC authentication

# ❌ Bad - Excessive permissions
permissions:
  contents: write   # Not needed for most workflows
  packages: write    # Not needed for CI
  pull-requests: write
```

### Permission Reference

| Permission | Use Case | Required? |
|------------|----------|-----------|
| `contents: read` | Checkout code | ✅ Usually yes |
| `contents: write` | Create releases | ❌ Only when needed |
| `id-token: write` | OIDC tokens | ❌ Only for cloud auth |
| `packages: write` | Publish packages | ❌ Only for publishing |
| `pull-requests: write` | Comment on PRs | ❌ Only when needed |
| `actions: write` | Manage workflows | ❌ Rarely needed |

### 3. Environment Variables vs Secrets

**Use secrets for sensitive data, environment variables for configuration.**

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    
    # ✅ Good - Secrets for sensitive data
    env:
      API_KEY: ${{ secrets.API_KEY }}
      DATABASE_URL: ${{ secrets.DATABASE_URL }}
    
    # ✅ Good - Plain text for non-sensitive config
    env:
      REGION: us-east-1
      ENVIRONMENT: production
      LOG_LEVEL: info
```

### Secret Types

| Secret Type | Examples | Storage |
|-------------|----------|---------|
| 🔴 Critical | API keys, passwords | GitHub Secrets |
| 🟠 High | Database credentials | GitHub Secrets |
| 🟡 Medium | OAuth tokens | GitHub Secrets |
| 🟢 Low | Non-sensitive config | Environment variables |

### 4. OpenID Connect (OIDC)

**Use OIDC instead of long-lived credentials for cloud authentication.**

```yaml
permissions:
  id-token: write  # Required for OIDC
  contents: read

jobs:
  deploy:
    runs-on: ubuntu-latest
    
    steps:
      - name: Authenticate to AWS
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::123456789:role/GitHubActionsRole
          aws-region: us-east-1
```

### OIDC Benefits

| Benefit | Description |
|---------|-------------|
| 🔒 No secrets | No long-lived credentials stored |
| 🔄 Automatic rotation | Tokens issued per workflow run |
| � Auditable | Clear attribution to GitHub |
| ✅ More secure | Reduced attack surface |

---

## ✅ Required Security Checks

All workflows submitted to this repository must include:

### 1. Dependency Scanning

```yaml
jobs:
  security-scan:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      # ✅ Required - Dependency vulnerability scanning
      - name: Run Trivy vulnerability scanner
        uses: aquasecurity/trivy-action@master
        with:
          scan-type: 'fs'
          scan-ref: '.'
          format: 'sarif'
          output: 'trivy-results.sarif'
      
      # ✅ Required - Upload results to GitHub
      - name: Upload vulnerability results
        uses: github/codeql-action/upload-sarif@v3
        with:
          sarif_file: 'trivy-results.sarif'
```

### 2. Code Scanning

```yaml
jobs:
  codeql:
    runs-on: ubuntu-latest
    permissions:
      security-events: write
      contents: read
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Initialize CodeQL
        uses: github/codeql-action/init@v3
        with:
          languages: javascript, python
      
      - name: Perform CodeQL Analysis
        uses: github/codeql-action/analyze@v3
```

### 3. Secret Scanning

```yaml
jobs:
  secrets-scan:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: TruffleHog Secrets Scan
        uses: trufflesecurity/truffleHog-action@v1.2.1
        with:
          path: ./
          base_depth: 10
```

---

## 🔐 Sensitive Data Handling

### 1. Masking Sensitive Values

**GitHub automatically masks secrets in logs, but you can add extra masking.**

```yaml
steps:
  - name: Deploy
    run: |
      echo "Deploying to ${{ secrets.API_ENDPOINT }}"
      echo "::add-mask::${{ secrets.API_KEY }}"
      deploy --api-key "${{ secrets.API_KEY }}"
```

### 2. Avoiding Log Exposure

```yaml
# ❌ Bad - Sensitive data in logs
- name: Configure
  run: |
    echo "API_KEY=${{ secrets.API_KEY }}" >> .env
    cat .env  # Exposes the key!

# ✅ Good - No log exposure
- name: Configure
  run: |
    echo "API_KEY=${{ secrets.API_KEY }}" >> .env
    # File is used but never logged
```

### 3. Using Intermediate Files Safely

```yaml
steps:
  - name: Create credentials file
    run: |
      cat > ${{ secrets.GCP_CREDENTIALS_FILE }} << EOF
      ${{ secrets.GCP_SERVICE_ACCOUNT_KEY }}
      EOF
  
  - name: Use credentials
    run: |
      gcloud auth activate-service-account --key-file=${{ secrets.GCP_CREDENTIALS_FILE }}
      rm ${{ secrets.GCP_CREDENTIALS_FILE }}  # Clean up!
```

---

## 📦 Dependency Management

### 1. Dependabot Configuration

**.github/dependabot.yml**

```yaml
version: 2
updates:
  - package-ecosystem: 'github-actions'
    directory: '/'
    schedule:
      interval: 'weekly'
    labels:
      - 'dependencies'
      - 'security'
    open-pull-requests-limit: 10
  
  - package-ecosystem: 'npm'
    directory: '/'
    schedule:
      interval: 'weekly'
    labels:
      - 'dependencies'
```

### 2. Automated Dependency Updates

```yaml
name: Dependency Updates
on:
  schedule:
    - cron: '0 5 * * 1'  # Weekly on Monday
  workflow_dispatch:

jobs:
  update:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Set up Python
        uses: actions/setup-python@v5
        with:
          python-version: '3.11'
      
      - name: Install Dependabot
        run: pip install dependabot-cli
      
      - name: Check for updates
        run: |
          dependabot check \
            --directory . \
            --生态系统 github-actions
```

---

## 🔍 Audit and Compliance

### 1. SLSA Compliance

For supply chain security, consider SLSA (Supply-chain Levels for Software Artifacts):

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      id-token: write
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Build
        run: make build
      
      - name: Generate provenance
        uses: slsa-framework/slsa-github-generator/generic@v2.0.0
        with:
          monster_artifact_name: 'binary'
          monster_artifact_path: './binary'
          compile_l1: true
```

### 2. Security Audit Workflow

```yaml
name: Security Audit
on:
  schedule:
    - cron: '0 0 * * 0'  # Weekly audit
  workflow_dispatch:

jobs:
  audit:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Run comprehensive security audit
        run: |
          echo "=== Security Audit Report ===" >> audit.md
          date >> audit.md
          
          # Check for sensitive files
          echo "## Sensitive Files Check" >> audit.md
          find . -name "*.pem" -o -name "*.key" >> audit.md || echo "None found" >> audit.md
          
          # Check for hardcoded secrets
          echo "## Potential Secrets Check" >> audit.md
          grep -r "password\s*=\s*['\"][^'\"]+['\"]" . >> audit.md || echo "None found" >> audit.md
      
      - name: Upload audit report
        uses: actions/upload-artifact@v4
        with:
          name: security-audit
          path: audit.md
```

---

## 📊 Security Checklist

### Before Submitting a Workflow

| Checklist Item | Status |
|----------------|--------|
| ✅ Action versions pinned to SHA | ⬜ |
| ✅ Minimal permissions granted | ⬜ |
| ✅ Secrets stored in GitHub Secrets | ⬜ |
| ✅ OIDC used for cloud auth | ⬜ |
| ✅ Sensitive data masked in logs | ⬜ |
| ✅ No hardcoded credentials | ⬜ |
| ✅ Dependency scanning included | ⬜ |
| ✅ CodeQL analysis included | ⬜ |
| ✅ Follows SLSA guidelines | ⬜ |

---

## 📚 Additional Resources

### Security Tools

| Tool | Purpose | Link |
|------|---------|------|
| CodeQL | Code analysis | https://codeql.github.com/ |
| Trivy | Vulnerability scanner | https://trivy.dev/ |
| TruffleHog | Secret scanning | https://trufflesecurity.com/trufflehog/ |
| Scorecard | Security posture | https://github.com/ossf/scorecard |
| Harden-Runner | Runtime security | https://github.com/step-security/harden-runner |

### References

| Resource | Description |
|----------|-------------|
| [GitHub Security Docs](https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions) | Official security guide |
| [SLSA Framework](https://slsa.dev/) | Supply chain security |
| [OWASP Top 10 CI/CD](https://owasp.org/www-project-top-10-ci-cd-security-risks/) | CI/CD security risks |
| [Awesome DevSecOps](https://github.com/TaptuIT/awesome-devsecops) | DevSecOps resources |

---

## ⚠️ Important Warnings

> **Warning**: Never commit sensitive data to the repository, even in private repos.
>
> **Warning**: Always rotate exposed credentials immediately.
>
> **Warning**: Review all third-party actions before using them.
>
> **Warning**: Use the principle of least privilege for all permissions.

---

<div align="center">

**🔒 Security is everyone's responsibility**

_If you see something, say something._

</div>
