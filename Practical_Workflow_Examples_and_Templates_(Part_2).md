# Practical Workflow Examples and Templates (Part 2)

This document completes the curated list of **practical examples, starter templates, and real-world repository links** for the remaining sixteen categories of workflows. These resources focus on advanced, specialized, and strategic areas, providing the necessary blueprints to implement complex automation in niche and high-value domains.

---

## Advanced Block: Specialized and Strategic Workflows (Categories 17-32)

This section covers the more specialized and strategic applications of GitHub Actions, from High-Performance Computing to Meta-Automation.

### 17. High-Performance Computing (HPC)

Workflows in HPC focus on automating the build, testing, and deployment of scientific and computationally intensive applications, often involving specialized hardware or clusters.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **HPC Deployment & Release** | Workflows designed for the development, release, and deployment of repositories used in High-Performance Computing environments. | [UN-OCHA/hpc-actions](https://github.com/UN-OCHA/hpc-actions) |
| **Scientific CI** | Examples of automated workflows for scientific applications, such as distributed copy tools used in large-scale data movement. | [hpc/dcp/actions](https://github.com/hpc/dcp/actions) |

### 18. Serverless & Cloud-Native

These workflows automate the deployment and management of serverless functions and cloud-native applications, emphasizing speed and cost-efficiency.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Serverless Framework Deployment** | An action that wraps the Serverless Framework CLI to deploy serverless applications to various cloud providers. | [serverless/github-action](https://github.com/serverless/github-action) |
| **AWS Lambda Deployment** | A dedicated action for deploying code packages directly to existing AWS Lambda functions. | [appleboy/lambda-action](https://github.com/appleboy/lambda-action) |

### 19. Edge Computing & Hybrid Cloud

Workflows for deploying and managing applications on the edge, often involving lightweight Kubernetes distributions or specialized IoT platforms.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Lightweight Kubernetes Deployment** | Examples of using K3s, a highly available, certified Kubernetes distribution, for edge deployments. | [rancher/k3s](https://github.com/rancher/k3s) |
| **Azure IoT Edge Deployment** | Workflows for building and deploying modules to Azure IoT Edge devices. | [azure/iot-edge-github-action](https://github.com/Azure/iot-edge-github-action) |

### 20. Compliance & Governance (Policy-as-Code)

Focuses on enforcing organizational policies, security standards, and regulatory compliance directly through automated checks in the CI pipeline.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Security Posture Analysis** | Using the OpenSSF Scorecard to monitor and track security best practices and compliance metrics for open-source projects. | [ossf/scorecard-action](https://github.com/ossf/scorecard-action) |
| **Compliance Framework** | Examples of using Trellis, an open-source compliance framework, to manage and enforce policies as code. | [trellis-framework/trellis](https://github.com/trellis-framework/trellis) |

### 21. Open Source Governance

Workflows dedicated to automating the administrative and community-facing aspects of maintaining an open-source project.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **CLA Management** | Automating the process of managing Contributor License Agreements (CLA) for incoming pull requests. | [cla-assistant/github-action](https://github.com/cla-assistant/github-action) |
| **Contributor Recognition** | Automating the recognition of all types of contributors (not just code) using the All Contributors bot. | [all-contributors/all-contributors-bot](https://github.com/all-contributors/all-contributors-bot) |

### 22. Monorepo Management

Specialized workflows to optimize CI/CD performance in large monorepos by running tasks only on projects affected by a code change.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Conditional Execution (Path Filtering)** | Using path filtering to trigger workflows only when specific files or directories are modified. | [dorny/paths-filter](https://github.com/dorny/paths-filter) |
| **Nx Monorepo Optimization** | Actions designed to optimize CI for monorepos managed by Nx, leveraging its dependency graph for faster builds. | [nrwl/nx-set-shas](https://github.com/nrwl/nx-set-shas) |

### 23. Supply Chain Security

Workflows focused on securing the software supply chain, from verifying artifact provenance to checking dependencies for vulnerabilities.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **SLSA Provenance Attestation** | Generating cryptographically verifiable build provenance (SLSA Level 3) for artifacts. | [actions/attest-build-provenance](https://github.com/actions/attest-build-provenance) |
| **Dependency Review** | Automatically reviewing and flagging vulnerable dependencies introduced in a pull request. | [actions/dependency-review-action](https://github.com/actions/dependency-review-action) |

### 24. Observability-as-Code

Automating the configuration and validation of monitoring, logging, and alerting systems, treating them as code within the repository.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **AWS Observability Integration** | Action to connect code changes to AWS CloudWatch for end-to-end application observability. | [aws-observability/aws-observability-investigator-action](https://github.com/aws-observability/aws-observability-investigator-action) |
| **Prometheus Rule Validation** | Workflows for validating Prometheus alerting and recording rules in CI before deployment. | [prometheus/prometheus](https://github.com/prometheus/prometheus) |

### 25. Disaster Recovery & Business Continuity

Workflows that automate testing of disaster recovery plans and detect configuration drift in infrastructure.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **AWS DR Tools** | A collection of tools and solutions for automating tasks related to AWS Elastic Disaster Recovery (DRS). | [aws-samples/drs-tools](https://github.com/aws-samples/drs-tools) |
| **Terraform Drift Detection** | Using `driftctl` to detect and report on unmanaged infrastructure changes that could compromise recovery plans. | [cloudskiff/driftctl](https://github.com/cloudskiff/driftctl) |

### 26. FinOps & Cost Management (Advanced)

Advanced workflows for automating cost-saving measures and providing granular financial visibility into cloud usage.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Cost Optimization Automation** | Examples of using GitHub Actions to automate cloud cost management and FinOps best practices. | [bhpuri/github-actions-finops](https://github.com/bhpuri/github-actions-finops) |

### 27. API Management & Testing

Workflows for ensuring the quality and consistency of APIs, including automated testing and specification validation.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Postman/Newman Testing** | Running API collection tests using Newman (Postman's CLI runner) in the CI pipeline. | [postman-labs/newman-action](https://github.com/postman-labs/newman-action) |
| **OpenAPI Specification Validation** | Action for validating API specifications against the OpenAPI (Swagger) standard. | [charlesw007/openapi-validator-action](https://github.com/charlesw007/openapi-validator-action) |

### 28. Virtualization & Emulation

Workflows for building and testing software across multiple architectures and operating systems using virtualization and emulation tools.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Multi-platform Builds (QEMU)** | Setting up QEMU to enable building and testing for different architectures (e.g., ARM) within a standard GitHub runner. | [docker/setup-qemu-action](https://github.com/docker/setup-qemu-action) |
| **VM Image Testing** | Examples of using Vagrant to provision and test virtual machine images in a consistent environment. | [hashicorp/vagrant](https://github.com/hashicorp/vagrant) |

### 29. Desktop Application Development

CI/CD for traditional desktop applications, automating the complex build, signing, and packaging processes for Windows, macOS, and Linux.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Electron App Building** | Using `electron-builder` to create installers and packages for Electron-based desktop applications. | [electron-userland/electron-builder](https://github.com/electron-userland/electron-builder) |
| **Windows Desktop CI** | Microsoft's official guidance and actions for setting up CI/CD for desktop applications like WPF and WinForms. | [microsoft/github-actions-for-desktop-apps](https://github.com/microsoft/github-actions-for-desktop-apps) |

### 30. Database DevOps (GitOps for DB)

Workflows that apply version control and CI/CD practices to database schema changes, ensuring safe and repeatable deployments.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Liquibase Schema Migration** | Action for managing and applying database schema changes using Liquibase. | [liquibase/liquibase-github-action](https://github.com/liquibase/liquibase-github-action) |
| **Flyway Version Control** | Action for running database migrations and managing the database schema version using Flyway. | [flyway/flyway-github-action](https://github.com/flyway/flyway-github-action) |

### 31. AI & LLM Integration

Workflows that integrate Large Language Models (LLMs) and AI tools into the development process, such as automated code review or documentation generation.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **OpenAI API Integration** | Examples of using the OpenAI Node.js client within a workflow to interact with LLMs for various tasks. | [openai/openai-node](https://github.com/openai/openai-node) |
| **LangChain Applications** | Workflows for building and testing applications that use the LangChain framework to orchestrate LLM calls. | [langchain-ai/langchain](https://github.com/langchain-ai/langchain) |

### 32. Meta-Automation (Self-Healing Pipelines)

The ultimate level of automation: workflows that monitor, validate, and secure the CI/CD pipelines themselves.

| Focus Area | Description | Repository / Template Link |
| :--- | :--- | :--- |
| **Workflow Linting** | Using `actionlint` to statically analyze and validate the syntax and security of the workflow YAML files. | [rhysd/actionlint](https://github.com/rhysd/actionlint) |
| **Pipeline Runtime Security** | Implementing `harden-runner` to provide runtime security, network egress filtering, and integrity checks for the GitHub Actions runner environment. | [step-security/harden-runner](https://github.com/step-security/harden-runner) |

---
---

---

*This document is part of the practical workflow examples collection.*
