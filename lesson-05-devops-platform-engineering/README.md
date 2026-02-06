# Lesson 05: DevOps and Platform Engineering

## Overview

DevOps is a set of practices that combines software development (Dev) and IT operations (Ops) to shorten the development lifecycle while delivering features, fixes, and updates frequently. Platform Engineering extends this by providing self-service capabilities to development teams through internal developer platforms.

---

## Sub-Lessons Index

| No. | Topic | Description | Diagrams |
|-----|-------|-------------|----------|
| 1 | [DevOps Fundamentals](./01-devops-fundamentals/README.md) | DevOps lifecycle, principles, and DORA metrics | 3 |
| 2 | [CI/CD Tools](./02-cicd-tools/README.md) | Azure DevOps, GitHub Actions, Jenkins | 4 |
| 3 | [Infrastructure as Code](./03-infrastructure-as-code/README.md) | Terraform workflow and IaC tools comparison | 2 |
| 4 | [Containerization](./04-containerization/README.md) | Docker architecture and container security | 2 |
| 5 | [Container Orchestration](./05-container-orchestration/README.md) | Kubernetes architecture and managed services | 3 |
| 6 | [GitOps](./06-gitops/README.md) | GitOps flow and ArgoCD workflow | 2 |
| 7 | [Artifact Management](./07-artifact-management/README.md) | Container registries and package managers | 1 |
| 8 | [Platform Engineering](./08-platform-engineering/README.md) | Internal developer platforms and tooling | 2 |

---

## Learning Objectives

By the end of this lesson, you will be able to:

- Understand DevOps principles and practices
- Design and implement CI/CD pipelines
- Apply Infrastructure as Code (IaC) practices
- Work with containers and orchestration platforms
- Implement GitOps workflows
- Build internal developer platforms

---

## 1. DevOps Fundamentals

### 1.1 DevOps Lifecycle

The DevOps lifecycle represents a continuous loop of planning, building, testing, deploying, and monitoring software.

### 1.2 DevOps Principles

| Principle | Description |
|-----------|-------------|
| **Automation** | Automate repetitive tasks |
| **Continuous Integration** | Merge code frequently, test automatically |
| **Continuous Delivery** | Always keep code deployable |
| **Infrastructure as Code** | Manage infrastructure through code |
| **Monitoring and Logging** | Observe system behavior |
| **Communication** | Break down silos between teams |
| **Continuous Improvement** | Learn from failures, iterate |

### 1.3 DevOps Metrics (DORA)

| Metric | Description | Elite Performance |
|--------|-------------|-------------------|
| **Deployment Frequency** | How often you deploy | Multiple times/day |
| **Lead Time for Changes** | Code commit to production | Less than 1 hour |
| **Change Failure Rate** | % of deployments causing failure | 0-15% |
| **Time to Restore** | Time to recover from failure | Less than 1 hour |

---

## 2. CI/CD Tools

### 2.1 Azure DevOps

**Components**:
- **Azure Repos**: Git repositories
- **Azure Pipelines**: CI/CD pipelines
- **Azure Boards**: Work tracking
- **Azure Artifacts**: Package management
- **Azure Test Plans**: Testing tools

### 2.2 GitHub Actions

GitHub Actions provides native CI/CD capabilities integrated with GitHub repositories.

### 2.3 GitLab CI

GitLab CI provides built-in CI/CD with GitLab repositories.

### 2.4 Jenkins

Jenkins is an open-source automation server supporting any project.

### CI/CD Tool Comparison

| Feature | Azure DevOps | GitHub Actions | GitLab CI | Jenkins |
|---------|--------------|----------------|-----------|---------|
| **Hosting** | Cloud/Server | Cloud | Cloud/Self | Self-hosted |
| **Config** | YAML | YAML | YAML | Groovy |
| **Integration** | Azure | GitHub | GitLab | Any |
| **Marketplace** | Extensions | Actions | Templates | Plugins |
| **Free Tier** | 1800 min/mo | 2000 min/mo | 400 min/mo | Free |

---

## 3. Infrastructure as Code (IaC)

### 3.1 Terraform

Cloud-agnostic IaC tool using HCL (HashiCorp Configuration Language).

### 3.2 IaC Tool Comparison

| Feature | Terraform | Pulumi | ARM/Bicep | CloudFormation |
|---------|-----------|--------|-----------|----------------|
| **Language** | HCL | TypeScript, Python, Go | JSON/Bicep | YAML/JSON |
| **Cloud Support** | Multi-cloud | Multi-cloud | Azure only | AWS only |
| **State Management** | Required | Required | Azure-managed | AWS-managed |
| **Learning Curve** | Medium | Low (if you know lang) | Medium | Medium |

---

## 4. Containerization

### 4.1 Docker

Docker enables containerization with multi-stage builds, non-root users, and health checks.

### 4.2 Container Security

Security layers include image security, runtime security, and registry security.

---

## 5. Container Orchestration

### 5.1 Kubernetes

Kubernetes provides declarative container orchestration with control plane and worker nodes.

### 5.2 Managed Kubernetes Services

| Service | Provider | Features |
|---------|----------|----------|
| **AKS** | Azure | Azure integration, Azure CNI, AAD integration |
| **EKS** | AWS | AWS integration, Fargate support |
| **GKE** | GCP | Autopilot mode, best-in-class networking |

---

## 6. GitOps

### 6.1 GitOps Principles

1. Declarative: Describe desired state
2. Versioned: Git as single source of truth
3. Automated: Changes applied automatically
4. Auditable: Complete history in Git

### 6.2 ArgoCD

Declarative GitOps continuous delivery tool for Kubernetes.

---

## 7. Artifact Management

### 7.1 Container Registries

| Registry | Provider | Features |
|----------|----------|----------|
| **Azure Container Registry** | Azure | Geo-replication, Tasks |
| **Amazon ECR** | AWS | Lifecycle policies |
| **Google Artifact Registry** | GCP | Multi-format support |
| **Docker Hub** | Docker | Public registry |
| **Harbor** | CNCF | Self-hosted, scanning |

### 7.2 Package Managers

| Type | Tools |
|------|-------|
| **npm/Node** | Azure Artifacts, npm registry, Verdaccio |
| **NuGet/.NET** | Azure Artifacts, NuGet.org |
| **Maven/Java** | Nexus, Artifactory |
| **PyPI/Python** | Azure Artifacts, private PyPI |
| **Helm** | Harbor, ChartMuseum |

---

## 8. Platform Engineering

### 8.1 Internal Developer Platform (IDP)

An IDP provides self-service capabilities including:
- Developer Portal
- Service Catalog
- Self-Service Infrastructure
- Golden Paths
- Observability

### 8.2 Platform Engineering Tools

| Category | Tools |
|----------|-------|
| **Developer Portal** | Backstage, Port, Cortex |
| **Self-Service** | Crossplane, Kratix |
| **Templates** | Cookiecutter, Yeoman, Backstage |
| **Documentation** | Backstage TechDocs, ReadMe |

---

## Practical Exercises

1. **CI/CD Pipeline**: Create a complete pipeline with build, test, and deploy stages
2. **Terraform Module**: Build a reusable Terraform module for a web application
3. **Kubernetes Deployment**: Deploy a multi-tier application to Kubernetes
4. **GitOps Setup**: Implement GitOps with ArgoCD for a sample application

---

## Further Reading

- The DevOps Handbook - Gene Kim, et al.
- Infrastructure as Code - Kief Morris
- Kubernetes in Action - Marko Luksa
- Team Topologies - Matthew Skelton and Manuel Pais
- Platform Engineering on Kubernetes - Mauricio Salatino

---

## Summary

DevOps and Platform Engineering are essential disciplines for modern software delivery. By automating CI/CD pipelines, managing infrastructure as code, leveraging containers and orchestration, and implementing GitOps practices, organizations can achieve faster, more reliable software delivery. Platform Engineering extends these capabilities by providing self-service platforms that empower developers while maintaining governance and standards.
