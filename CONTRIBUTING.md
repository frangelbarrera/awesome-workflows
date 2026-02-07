# 🤝 Contributing to Awesome GitHub Workflows

Thank you for your interest in contributing to this repository! This guide will help you get started with contributing workflows, documentation, and improvements.

---

## 📋 Table of Contents

- [🎯 Types of Contributions](#-types-of-contributions)
- [🚀 Getting Started](#-getting-started)
- [📝 Adding a New Workflow](#-adding-a-new-workflow)
- [📖 Improving Documentation](#-improving-documentation)
- [✅ Workflow Requirements](#-workflow-requirements)
- [🔄 Pull Request Process](#-pull-request-process)
- [📜 Code of Conduct](#-code-of-conduct)

---

## 🎯 Types of Contributions

We welcome various types of contributions:

| Type | Description | Example |
|------|-------------|---------|
| 🆕 **New Workflow** | Add a new workflow template | Deploy to a new cloud provider |
| 🐛 **Bug Fix** | Fix issues in existing workflows | Fix path filtering logic |
| 📈 **Improvement** | Enhance existing workflows | Add caching optimization |
| 📚 **Documentation** | Improve docs or add examples | Add usage examples |
| 🌐 **Translation** | Translate docs to other languages | Spanish, Chinese, etc. |
| 💡 **Idea** | Suggest new categories or tools | Open a discussion |

---

## 🚀 Getting Started

### Prerequisites

- A GitHub account
- Basic understanding of YAML
- Knowledge of GitHub Actions concepts

### First Steps

1. **Fork** the repository by clicking the "Fork" button
2. **Clone** your fork locally:

```bash
git clone https://github.com/YOUR-USERNAME/awesome-workflows.git
cd awesome-workflows
```

3. **Create** a feature branch:

```bash
git checkout -b feature/your-workflow-name
```

4. **Make** your changes
5. **Test** your workflow (if applicable)
6. **Commit** and push your changes
7. **Open** a Pull Request

---

## 📝 Adding a New Workflow

### Directory Structure

Place your workflow in the appropriate location:

```text
.github/workflows/
├── ci-basic.yml              # General CI workflows
├── deploy-terraform.yml      # IaC deployments
├── security-scan.yml         # Security workflows
└── your-workflow.yml         # Your new workflow
```

### Workflow Template

Use this template when adding a new workflow:

```yaml
# .github/workflows/your-workflow.yml

name: "Category: Your Workflow Name"

# When should this workflow run?
on:
  push:
    branches: [main]
    paths:
      - 'your-path/**'
  pull_request:
    branches: [main]

# Who can run this workflow?
permissions:
  contents: read
  id-token: write  # Only if needed for OIDC

# Run jobs in parallel or sequence
jobs:
  validate:
    name: Validate
    runs-on: ubuntu-latest
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      - name: Setup
        uses: actions/setup-tool@v4
        with:
          version: 'latest'
      
      - name: Run
        run: your-command

  deploy:
    name: Deploy
    needs: validate
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'
    
    steps:
      - name: Checkout
        uses: actions/checkout@v4
      
      # Add deployment steps...
    
    environment:
      name: production
      url: https://your-deployment.com

# Optional: Cancel redundant runs
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
```

### Workflow Naming Conventions

| Category | Prefix | Example |
|----------|--------|---------|
| CI/CD | `ci-` | `ci-nodejs.yml` |
| Deployment | `deploy-` | `deploy-aws.yml` |
| Security | `security-` | `security-scan.yml` |
| Testing | `test-` | `test-integration.yml` |
| Release | `release-` | `release-npm.yml` |

---

## 📖 Improving Documentation

### Documentation Structure

```
docs/
├── getting-started.md       # Quick start guide
├── best-practices.md        # Best practices
├── security-guide.md        # Security guidelines
├── examples/                # Detailed examples
│   ├── ci-example.md
│   └── deployment-example.md
└── troubleshooting.md       # Common issues
```

### Documentation Style Guide

- Use **Markdown** for all documentation
- Include **code blocks** with language syntax
- Add **screenshots** when helpful
- Use **clear headings** (H2, H3)
- Keep sentences **concise and clear**
- Include **links** to official documentation

### Example Documentation

```markdown
## Deploying to AWS

This workflow deploys your application to AWS Elastic Beanstalk.

### Prerequisites

- AWS credentials configured
- EB application already created

### Configuration

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Deploy to EB
        uses: einaregilsson/beanstalk-deploy@v21
        with:
          aws_access_key: ${{ secrets.AWS_ACCESS_KEY }}
          aws_secret_key: ${{ secrets.AWS_SECRET_KEY }}
          application_name: my-app
          environment_name: my-app-env
          version_label: ${{ github.sha }}
```

### See Also

- [AWS EB Documentation](https://docs.aws.amazon.com/elastic-beanstalk/)
- [Beanstalk Deploy Action](https://github.com/einaregilsson/beanstalk-deploy)
```

---

## ✅ Workflow Requirements

All submitted workflows must meet these requirements:

### 1. Security Requirements

| Requirement | Description | Checklist |
|------------|-------------|-----------|
| ✅ Least Privilege | Use minimal permissions | `permissions: contents: read` |
| ✅ Secrets | Store credentials in GitHub Secrets | Never hardcode tokens |
| ✅ Pin SHA | Use full commit SHA for actions | `uses: actions/checkout@11bd7190...` |
| ✅ Validate | Add input validation where needed | Check environment variables |

### 2. Quality Requirements

| Requirement | Description | Checklist |
|------------|-------------|-----------|
| ✅ Documentation | Add clear comments | `# Step description` |
| ✅ Error Handling | Add `continue-on-error` when appropriate | Handle failures gracefully |
| ✅ Idempotency | Workflow should be re-runnable | Don't rely on one-time state |
| ✅ Timeout | Set reasonable timeouts | `timeout-minutes: 30` |

### 3. Compatibility Requirements

| Requirement | Description | Checklist |
|------------|-------------|-----------|
| ✅ Latest Actions | Use recent action versions | Update from `@v1` to `@v4` |
| ✅ Matrix Testing | Test on multiple OS when relevant | Use `runs-on: ${{ matrix.os }}` |
| ✅ Caching | Cache dependencies when possible | `actions/cache` |

### 4. Performance Requirements

| Requirement | Description | Checklist |
|------------|-------------|-----------|
| ✅ Concurrency | Cancel redundant runs | `concurrency` group |
| ✅ Path Filtering | Trigger only on relevant files | `on.<push>.paths` |
| ✅ Efficient Caching | Use appropriate cache keys | `hashFiles()` |

---

## 🔄 Pull Request Process

### Before Submitting

1. **Test** your workflow locally (if possible)
2. **Lint** your YAML:
   ```bash
   # Using actionlint (recommended)
   brew install actionlint
   actionlint .github/workflows/*.yml
   ```
3. **Format** your YAML:
   ```bash
   # Using yamlfmt
   pip install yamlfmt
   yamlfmt .github/workflows/*.yml
   ```
4. **Review** your changes for:
   - [ ] Correct syntax
   - [ ] Security best practices
   - [ ] Clear naming
   - [ ] Documentation updates

### PR Description Template

```markdown
## Description

Brief description of the changes.

## Type of Change

- [ ] 🆕 New workflow
- [ ] 🐛 Bug fix
- [ ] 📈 Improvement
- [ ] 📚 Documentation
- [ ] 🌐 Translation

## Checklist

- [ ] My workflow follows the security requirements
- [ ] I have tested the workflow (if applicable)
- [ ] I have added appropriate documentation
- [ ] I have lint/format checked my YAML
- [ ] I have signed off my commits

## Related Issues

Fixes # (issue number)

## Additional Context

Any additional information about the PR.
```

### PR Review Process

1. **Automated Checks**: CI runs linting and validation
2. **Maintainer Review**: A maintainer reviews your PR
3. **Feedback**: You may receive suggestions for improvements
4. **Approval**: Once approved, your PR is merged! 🎉

---

## 📜 Code of Conduct

### Our Pledge

We pledge to make participation in our project a harassment-free experience for everyone, regardless of:
- Age, body size, disability
- Ethnicity, gender identity/expression
- Experience level, education
- Nationality, personal appearance
- Race, religion
- Sexual identity/orientation

### Our Standards

**Encouraged Behavior:**
- Using welcoming and inclusive language
- Being respectful of differing viewpoints
- Accepting constructive criticism gracefully
- Focusing on what is best for the community
- Showing empathy towards community members

**Unacceptable Behavior:**
- Harassment, insults, or discriminatory comments
- Trolling or personal attacks
- Publishing private information without consent
- Unwelcome sexual attention or advances
- Other conduct that is inappropriate in a professional setting

### Enforcement

Instances of unacceptable behavior may be reported to the project team. All complaints will be reviewed and investigated promptly and fairly.

---

## 💬 Getting Help

### Questions?

- **Discussions**: Use [GitHub Discussions](https://github.com/frangelbarrera/awesome-workflows/discussions) for questions
- **Issues**: Use [GitHub Issues](https://github.com/frangelbarrera/awesome-workflows/issues) for bugs
- **Slack**: Join our community Slack channel

### Resources

| Resource | Link |
|----------|------|
| GitHub Actions Docs | https://docs.github.com/en/actions |
| Awesome Actions | https://github.com/sdras/awesome-actions |
| ActionLint | https://github.com/rhysd/actionlint |
| YAML Lint | https://www.yamllint.com/ |

---

<div align="center">

**Thank you for contributing! 🚀**

_Your contributions help make this repository amazing for everyone._

</div>
