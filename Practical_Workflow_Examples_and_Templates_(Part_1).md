# Practical Workflow Examples and Templates (Part 1)

This document provides a curated list of **practical examples, starter templates, and real-world repository links** for the first sixteen categories of workflows, designed to serve as immediate blueprints for your GitHub Actions implementation. These resources move beyond theoretical concepts, offering concrete YAML files and repository structures that can be adapted for various projects.

---

## Initial Block: Core Technology Workflows (Categories 1-8)

This section focuses on the foundational areas of technology where automated workflows are indispensable, providing links to official starter kits and high-impact community projects.

### 1. Software Development Life Cycle (SDLC) & CI/CD

The core of any modern development process relies on robust Continuous Integration and Continuous Deployment (CI/CD) pipelines. The following examples provide templates for building, testing, and releasing software across different languages and versioning strategies.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Official Starter Workflows** | Official templates for common languages (Node, Python, Java, Go, etc.) to quickly set up basic CI. | [actions/starter-workflows](https://github.com/actions/starter-workflows) |
| **Real-world CI Example** | Examination of complex, high-volume CI/CD pipelines used by major open-source projects. | [facebook/react/.github/workflows](https://github.com/facebook/react/tree/main/.github/workflows) |
| **Automated Versioning** | Implementation of Semantic Release to automate version bumping, changelog generation, and package publishing. | [semantic-release/semantic-release](https://github.com/semantic-release/semantic-release) |

### 2. Web Development

Web workflows focus on frontend and backend testing, deployment to various hosting services, and performance auditing directly within the CI pipeline.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Frontend Deployment** | Official action for deploying static sites to GitHub Pages, a common use case for web projects. | [actions/deploy-pages](https://github.com/actions/deploy-pages) |
| **Cypress E2E Testing** | Templates and actions for running end-to-end (E2E) tests using Cypress in a CI environment. | [cypress-io/github-action](https://github.com/cypress-io/github-action) |
| **Lighthouse Performance Audit** | Integrating Google Lighthouse to perform performance, accessibility, and SEO audits on every commit. | [treosh/lighthouse-ci-action](https://github.com/treosh/lighthouse-ci-action) |

### 3. Mobile Applications

Mobile development workflows are specialized for building, testing, and distributing applications for iOS and Android, often involving platform-specific tools and complex signing processes.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Android Build & Test** | Standard CI setup using `setup-java` and Gradle for building and running unit tests on Android projects. | [actions/setup-java](https://github.com/actions/setup-java) |
| **iOS/Android Distribution** | Utilizing Fastlane, a powerful toolset for automating the release process for both major mobile platforms. | [fastlane/fastlane](https://github.com/fastlane/fastlane) |
| **Unity Game Development** | Specialized actions for automating the building and testing of projects created with the Unity game engine. | [game-ci/unity-actions](https://github.com/game-ci/unity-actions) |

### 4. Cybersecurity (DevSecOps)

DevSecOps workflows embed security checks directly into the development pipeline, ensuring that vulnerabilities are identified and mitigated early in the process.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Static Analysis (SAST)** | Official action for running CodeQL, GitHub's powerful semantic code analysis engine, to find security vulnerabilities. | [github/codeql-action](https://github.com/github/codeql-action) |
| **Dependency Scanning** | Integration with Snyk for scanning dependencies and open-source packages for known vulnerabilities. | [snyk/actions](https://github.com/snyk/actions) |
| **Container & IaC Scanning** | Using Trivy, a comprehensive scanner for vulnerabilities in container images and Infrastructure as Code (IaC) files. | [aquasecurity/trivy-action](https://github.com/aquasecurity/trivy-action) |

### 5. Infrastructure as Code (IaC)

IaC workflows automate the provisioning and management of cloud resources, focusing on validation, planning, and application of infrastructure changes.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Terraform Setup & Plan** | Official action for setting up Terraform and running `plan` or `apply` commands securely. | [hashicorp/setup-terraform](https://github.com/hashicorp/setup-terraform) |
| **Pulumi Cloud Automation** | Workflows for deploying and managing cloud infrastructure using the Pulumi framework. | [pulumi/actions](https://github.com/pulumi/actions) |
| **Ansible Linting & Testing** | Action for linting Ansible playbooks to ensure best practices and syntax correctness before execution. | [ansible/ansible-lint-action](https://github.com/ansible/ansible-lint-action) |

### 6. Network Automation

These workflows apply CI/CD principles to network configurations, enabling automated testing and deployment of network changes, often using specialized tools like ZeroTier for secure connectivity.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Secure Runner Connectivity** | Action to connect GitHub Actions runners to a private ZeroTier network for secure access to internal resources. | [zerotier/github-action](https://github.com/zerotier/github-action) |
| **DNS as Code** | Automation for managing DNS records declaratively using the `dnscontrol` tool. | [stackexchange/dnscontrol](https://github.com/stackexchange/dnscontrol) |

### 7. MLOps (Machine Learning Operations)

MLOps workflows automate the entire machine learning lifecycle, from data preparation and model training to deployment and monitoring.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **AWS SageMaker Pipelines** | Templates for orchestrating MLOps pipelines using AWS SageMaker and GitHub Actions. | [aws-samples/mlops-sagemaker-github-actions](https://github.com/aws-samples/mlops-sagemaker-github-actions) |
| **Continuous Machine Learning (CML)** | Using CML to generate reports, visualize metrics, and automate model retraining based on data changes. | [iterative/cml](https://github.com/iterative/cml) |
| **Experiment Tracking** | Action for integrating Weights & Biases to track and log machine learning experiments directly from the CI pipeline. | [wandb/wandb-action](https://github.com/wandb/wandb-action) |

### 8. Project Management & Automation

These workflows automate common repository maintenance tasks, reducing administrative overhead and keeping projects organized.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Stale Issue Management** | Automatically marking and closing issues or pull requests that have been inactive for a defined period. | [actions/stale](https://github.com/actions/stale) |
| **Automatic Labeling** | Applying labels to pull requests based on the files or directories that have been modified. | [actions/labeler](https://github.com/actions/labeler) |
| **Advanced Project Card Automation** | Automating the movement of project cards across columns based on issue/PR events (e.g., closing, merging). | [alex-page/github-project-automation-plus](https://github.com/alex-page/github-project-automation-plus) |

---

## Expansion Block: Specialized Workflows (Categories 9-16)

This block delves into more specialized domains, offering practical examples for content management, data processing, and niche development environments.

### 9. Docs-as-Code & Content Ops

Workflows for treating documentation as code, automating the building, linting, and deployment of technical content.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Static Site Deployment** | General-purpose action for deploying static site generators (like Hugo or Jekyll) to GitHub Pages. | [peaceiris/actions-gh-pages](https://github.com/peaceiris/actions-gh-pages) |
| **Rust-based Site Generation** | Workflow for building and deploying sites created with the Zola static site generator. | [peaceiris/actions-zola](https://github.com/peaceiris/actions-zola) |
| **Editorial Style Linting** | Using Vale to enforce editorial style guides and check for consistency in technical documentation. | [errata-ai/vale-action](https://github.com/errata-ai/vale-action) |

### 10. Code Quality & Refactoring

Focuses on tools that enforce coding standards, perform deep code analysis, and even automate large-scale code refactoring tasks.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Universal Linter** | The Super-Linter action, which combines multiple linters for various languages into a single, easy-to-use workflow. | [github/super-linter](https://github.com/github/super-linter) |
| **Automated Refactoring** | Toolkit for performing automated, large-scale refactoring and code transformation tasks. | [m-zakeri/CodART](https://github.com/m-zakeri/CodART) |
| **Advanced Quality Metrics** | Integration with SonarCloud to provide deep code quality, security, and maintainability analysis. | [SonarSource/sonarcloud-github-action](https://github.com/SonarSource/sonarcloud-github-action) |

### 11. Game Development

Specialized CI/CD for game engines, focusing on automated builds for different platforms and running in-engine tests.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Godot Game Export** | Workflow for automating the export of Godot projects to various target platforms. | [firebelley/godot-export](https://github.com/firebelley/godot-export) |
| **Unity Test Runner** | Action dedicated to running unit and integration tests within the Unity engine environment. | [game-ci/unity-test-runner](https://github.com/game-ci/unity-test-runner) |

### 12. Blockchain & Web3

Workflows for the decentralized web, including automated testing of smart contracts and security analysis of blockchain code.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Smart Contract Testing** | Templates for using Hardhat, a popular development environment for Ethereum smart contracts, in CI. | [nomiclabs/hardhat](https://github.com/NomicFoundation/hardhat) |
| **Solidity Static Analysis** | Integration of Slither, a static analysis framework for Solidity, to detect common vulnerabilities. | [crytic/slither-action](https://github.com/crytic/slither-action) |

### 13. IoT & Embedded Systems

CI/CD for hardware projects, where the workflow must handle cross-compilation, firmware building, and interaction with embedded platforms.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **PlatformIO CI** | Examples for setting up Continuous Integration for embedded projects managed by PlatformIO. | [platformio/platformio-examples](https://github.com/platformio/platformio-examples) |
| **Arduino Sketch Linting** | Action for linting Arduino sketches to ensure code quality and adherence to best practices. | [arduino/arduino-lint-action](https://github.com/arduino/arduino-lint-action) |

### 14. Data Engineering & ETL

Workflows for managing data transformation pipelines, ensuring data quality, and orchestrating complex data flows.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **dbt (Data Build Tool)** | Templates for running dbt commands (e.g., `dbt run`, `dbt test`) to manage data transformations in a warehouse. | [dbt-labs/dbt-core](https://github.com/dbt-labs/dbt-core) |
| **Airflow Orchestration** | Examples of using GitHub Actions to manage or trigger complex data pipelines orchestrated by Apache Airflow. | [apache/airflow](https://github.com/apache/airflow) |

### 15. FinOps & Cost Optimization

Workflows dedicated to monitoring and optimizing cloud spending, integrating cost analysis directly into the IaC review process.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **IaC Cost Estimation** | Integration of Infracost to provide cloud cost estimates for Terraform changes directly in the pull request. | [infracost/actions](https://github.com/infracost/actions) |

### 16. Advanced CI/CD (Chaos, Performance)

These are high-level workflows that focus on non-functional requirements like system resilience and performance under load.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Load Testing** | Using Artillery to run performance and load tests against applications as part of the deployment pipeline. | [artilleryio/artillery-action](https://github.com/artilleryio/artillery-action) |
| **Chaos Engineering** | Workflows for injecting faults and running chaos experiments in Kubernetes environments using Chaos Mesh. | [chaos-mesh/chaos-mesh](https://github.com/chaos-mesh/chaos-mesh) |

---
---

---

*This document is part of the practical workflow examples collection.*
