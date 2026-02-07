# 📖 Best Practices for GitHub Actions Workflows

This guide provides comprehensive best practices for creating secure, efficient, and maintainable GitHub Actions workflows.

---

## 📋 Table of Contents

- [🏗️ Workflow Structure](#️-workflow-structure)
- [🔒 Security Best Practices](#-security-best-practices)
- [⚡ Performance Optimization](#-performance-optimization)
- [🧪 Testing Strategies](#-testing-strategies)
- [📦 Dependency Management](#-dependency-management)
- [🔄 Environment Management](#-environment-management)
- [📊 Monitoring and Observability](#-monitoring-and-observability)
- [🐛 Error Handling](#-error-handling)
- [📝 Documentation](#-documentation)
- [🧹 Cleanup and Maintenance](#-cleanup-and-maintenance)

---

## 🏗️ Workflow Structure

### 1. Organize with Descriptive Names

```yaml
# ✅ Good - Clear and descriptive
name: "CI: Node.js Application"

# ❌ Bad - Vague or unclear
name: "build"
```

### 2. Use Appropriate Triggers

```yaml
# ✅ Good - Specific triggers
on:
  push:
    branches: [main, develop]
    paths:
      - 'src/**'
      - 'package.json'
      - '*.yml'
  pull_request:
    branches: [main]
  workflow_dispatch:
    inputs:
      environment:
        description: 'Deployment environment'
        required: true
        default: 'staging'

# ❌ Bad - Too broad
on: [push, pull_request]
```

### 3. Modularize Complex Workflows

```yaml
name: Complete CI/CD

on: [push, pull_request]

jobs:
  lint:
    name: Lint
    uses: ./.github/workflows/lint.yml
  
  test:
    name: Test
    needs: lint
    uses: ./.github/workflows/test.yml
  
  build:
    name: Build
    needs: test
    uses: ./.github/workflows/build.yml
  
  deploy:
    name: Deploy
    needs: build
    uses: ./.github/workflows/deploy.yml
```

### 4. Use Reusable Workflows

**.github/workflows/reusable-test.yml**

```yaml
name: Reusable Test Workflow

on:
  workflow_call:
    inputs:
      node_version:
        required: true
        type: string
        default: '20'
      test_suite:
        required: false
        type: string
        default: 'all'
    secrets:
      npm_token:
        required: true

jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ inputs.node_version }}
      
      - name: Install dependencies
        run: npm ci
        env:
          NODE_AUTH_TOKEN: ${{ secrets.npm_token }}
      
      - name: Run tests
        run: npm test -- --suite ${{ inputs.test_suite }}
```

---

## 🔒 Security Best Practices

### 1. Pin Action Versions to SHA

```yaml
steps:
  # ✅ Good - Pinned to SHA
  - uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683  # v4.2.2
  
  # ❌ Bad - Using tag (vulnerable to hijacking)
  - uses: actions/checkout@v4
```

### 2. Apply Least Privilege

```yaml
# ✅ Good - Minimal permissions
permissions:
  contents: read
  id-token: write

# ❌ Bad - Excessive permissions
permissions:
  contents: write
  packages: write
  pull-requests: write
```

### 3. Use OIDC for Cloud Authentication

```yaml
permissions:
  id-token: write
  contents: read

steps:
  - name: Authenticate to AWS
    uses: aws-actions/configure-aws-credentials@v4
    with:
      role-to-assume: arn:aws:iam::123456789:role/GitHubActionsRole
      aws-region: us-east-1
```

### 4. Mask Sensitive Data

```yaml
steps:
  - name: Deploy
    run: |
      echo "::add-mask::${{ secrets.API_KEY }}"
      deploy --key "${{ secrets.API_KEY }}"
```

---

## ⚡ Performance Optimization

### 1. Use Caching

```yaml
steps:
  - uses: actions/checkout@v4
  
  - name: Cache Node.js modules
    uses: actions/cache@v4
    with:
      path: |
        ~/.npm
        node_modules
      key: ${{ runner.os }}-npm-${{ hashFiles('**/package-lock.json') }}
      restore-keys: |
        ${{ runner.os }}-npm-
  
  - name: Install dependencies
    run: npm ci
```

### 2. Implement Concurrency Control

```yaml
# Cancel in-progress runs when a new one starts
concurrency:
  group: ${{ github.workflow }}-${{ github.ref }}
  cancel-in-progress: true
```

### 3. Use Path Filtering

```yaml
on:
  push:
    paths:
      - 'src/**'
      - '*.json'
      - '*.yml'
```

### 4. Run Jobs in Parallel

```yaml
jobs:
  lint:
    runs-on: ubuntu-latest
    steps:
      - run: npm run lint
  
  test:
    runs-on: ubuntu-latest
    steps:
      - run: npm test
  
  # These jobs run in parallel automatically
```

### 5. Use Matrix Strategies Efficiently

```yaml
jobs:
  test:
    runs-on: ${{ matrix.os }}
    strategy:
      fail-fast: false
      matrix:
        os: [ubuntu-latest, windows-latest, macos-latest]
        node-version: [18, 20, 22]
        exclude:
          - os: windows-latest
            node-version: 22
```

---

## 🧪 Testing Strategies

### 1. Comprehensive Test Suite

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Run unit tests
        run: npm test -- --testPathPattern=unit
      
      - name: Run integration tests
        run: npm test -- --testPathPattern=integration
        env:
          DATABASE_URL: ${{ secrets.DATABASE_URL }}
      
      - name: Run E2E tests
        uses: cypress-io/github-action@v6
        with:
          build: npm run build
          start: npm start
```

### 2. Test Matrix

```yaml
jobs:
  test:
    runs-on: }}
    strategy:
 ${{ matrix.os      matrix:
        os: [ubuntu-latest, windows-latest]
        node: [18, 20, 22]
        database: [postgres, mysql]
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node }}
      
      - name: Setup database
        run: |
          # Setup database based on matrix value
      
      - name: Run tests
        run: npm test
```

### 3. Parallel Test Execution

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    
    strategy:
      matrix:
        test:
          - path: tests/unit
          - path: tests/integration/api
          - path: tests/integration/ui
          - path: tests/e2e
    
    steps:
      - uses: actions/checkout@v4
      
      - name: Run ${{ matrix.test.path }} tests
        run: npm test -- ${{ matrix.test.path }}
```

---

## 📦 Dependency Management

### 1. Cache Package Managers

```yaml
# npm
- name: Cache npm
  uses: actions/cache@v4
  with:
    path: ~/.npm
    key: ${{ runner.os }}-npm-${{ hashFiles('**/package-lock.json') }}

# pip
- name: Cache pip
  uses: actions/cache@v4
  with:
    path: ~/.cache/pip
    key: ${{ runner.os }}-pip-${{ hashFiles('**/requirements*.txt') }}

# Maven
- name: Cache Maven
  uses: actions/cache@v4
  with:
    path: ~/.m2
    key: ${{ runner.os }}-m2-${{ hashFiles('**/pom.xml') }}
```

### 2. Use Dependabot

**.github/dependabot.yml**

```yaml
version: 2
updates:
  - package-ecosystem: 'github-actions'
    directory: '/'
    schedule:
      interval: 'weekly'
  
  - package-ecosystem: 'npm'
    directory: '/'
    schedule:
      interval: 'weekly'
```

---

## 🔄 Environment Management

### 1. Use GitHub Environments

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: production
      url: https://your-app.com
```

### 2. Environment Protection Rules

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment:
      name: production
      url: https://your-app.com
      # Requires review before deployment
```

### 3. Environment-Specific Configuration

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    environment: ${{ github.ref == 'refs/heads/main' && 'production' || 'staging' }}
    
    steps:
      - name: Deploy
        run: ./deploy.sh ${{ github.ref }}
        env:
          ENVIRONMENT: ${{ github.ref == 'refs/heads/main' && 'production' || 'staging' }}
```

---

## 📊 Monitoring and Observability

### 1. Add Workflow Status Badges

```yaml
name: CI

on: [push, pull_request]

jobs:
  build:
    # ...
```

Add to your README:

```markdown
![CI Pipeline](https://github.com/frangelbarrera/awesome-workflows/actions/workflows/ci.yml/badge.svg)
```

### 2. Track Workflow Metrics

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    
    steps:
      - name: Track build time
        run: |
          echo "Build started at $(date)"
      
      - name: Build
        run: npm run build
      
      - name: Track completion
        run: |
          echo "Build completed at $(date)"
```

### 3. Send Notifications

```yaml
- name: Notify on failure
  if: failure()
  uses: slackapi/slack-github-action@v1.25.0
  with:
    payload: |
      {
        "text": "Workflow failed: ${{ github.workflow }}",
        "blocks": [
          {
            "type": "section",
            "text": {
              "type": "mrkdwn",
              "text": "❌ *Workflow Failed*\n${{ github.repository }}"
            }
          }
        ]
      }
  env:
    SLACK_WEBHOOK_URL: ${{ secrets.SLACK_WEBHOOK_URL }}
```

---

## 🐛 Error Handling

### 1. Continue on Error (When Appropriate)

```yaml
steps:
  - name: Run linter
    continue-on-error: true  # Allow workflow to continue
    run: npm run lint
  
  - name: Run tests
    run: npm test
```

### 2. Add Timeout Limits

```yaml
jobs:
  long-running-test:
    runs-on: ubuntu-latest
    timeout-minutes: 30  # Prevent hung jobs
    
    steps:
      - name: Long running task
        run: npm run test:integration
```

### 3. Handle Expected Failures

```yaml
steps:
  - name: Optional deployment
    run: |
      if [[ "${{ github.ref }}" == "refs/heads/main" ]]; then
        ./deploy.sh
      else
        echo "Skipping deployment for non-main branch"
      fi
```

---

## 📝 Documentation

### 1. Comment Your Workflows

```yaml
jobs:
  deploy:
    runs-on: ubuntu-latest
    
    # Only deploy on main branch after tests pass
    if: github.ref == 'refs/heads/main' && github.event_name == 'push'
    
    steps:
      # Step-level comments
      - name: Checkout code
        uses: actions/checkout@v4
      
      - name: Configure AWS credentials
        # Explain WHY this step is necessary
        uses: aws-actions/configure-aws-credentials@v4
        with:
          role-to-assume: arn:aws:iam::123456789:role/deploy-role
          aws-region: us-east-1
```

### 2. Create Workflow Documentation

```markdown
<!-- docs/workflows/deploy.yml -->

# Deploy Workflow

This workflow deploys the application to AWS Elastic Beanstalk.

## Triggers

- Push to `main` branch
- Manual trigger via `workflow_dispatch`

## Jobs

### 1. Build
- Compiles the application
- Runs unit tests
- Creates artifact

### 2. Deploy
- Authenticates to AWS
- Deploys to Elastic Beanstalk
- Sends notification

## Requirements

- AWS credentials configured
- Elastic Beanstalk application created
```

---

## 🧹 Cleanup and Maintenance

### 1. Clean Up Artifacts

```yaml
- name: Upload build artifact
  uses: actions/upload-artifact@v4
  with:
    name: build
    path: dist/
    retention-days: 7  # Automatically cleanup after 7 days
```

### 2. Rotate Secrets Regularly

```yaml
name: Secret Rotation Reminder
on:
  schedule:
    - cron: '0 9 1 */3 *'  # Quarterly on 1st day of month at 9am

jobs:
  check:
    runs-on: ubuntu-latest
    
    steps:
      - name: Check secret age
        run: |
          # Check last rotation date for secrets
          # Send reminder if approaching 90 days
```

### 3. Archive Old Workflows

```yaml
# Move rarely used workflows to a separate directory
.github/workflows/archived/
```

---

## 📋 Quick Reference Checklist

### Before Committing

- [ ] Actions pinned to SHA
- [ ] Minimal permissions set
- [ ] Secrets used instead of hardcoded values
- [ ] Caching configured
- [ ] Concurrency control added
- [ ] Timeout limits set
- [ ] Appropriate triggers configured
- [ ] Comments added for complex logic
- [ ] Tested locally or in PR

### Security Checklist

- [ ] No secrets in logs
- [ ] OIDC for cloud auth
- [ ] Sensitive data masked
- [ ] Dependency scanning included
- [ ] Code scanning configured

### Performance Checklist

- [ ] Dependencies cached
- [ ] Jobs run in parallel
- [ ] Path filtering used
- [ ] Artifacts have retention limits
- [ ] No unnecessary steps

---

## 📚 Additional Resources

| Resource | Link |
|----------|------|
| GitHub Actions Documentation | https://docs.github.com/en/actions |
| Workflow Syntax | https://docs.github.com/en/actions/using-workflows/workflow-syntax-for-github-actions |
| Reusable Workflows | https://docs.github.com/en/actions/using-workflows/reusing-workflows |
| Security Hardening | https://docs.github.com/en/actions/security-guides/security-hardening-for-github-actions |

---

<div align="center">

**🚀 Following these best practices will help you create robust, secure, and efficient workflows.**

</div>
