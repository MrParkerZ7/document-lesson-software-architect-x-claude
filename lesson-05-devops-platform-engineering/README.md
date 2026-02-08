# Lesson 05: DevOps and Platform Engineering

## Overview

DevOps is a set of practices that combines software development (Dev) and IT operations (Ops) to shorten the development lifecycle while delivering features, fixes, and updates frequently. Platform Engineering extends this by providing self-service capabilities to development teams through internal developer platforms.

---

## 📁 Files in This Lesson

### 📚 Sub-Lessons
| # | Topic | README |
|---|-------|--------|
| 01 | DevOps Fundamentals | [README](./01-devops-fundamentals/README.md) |
| 02 | CI/CD Tools | [README](./02-cicd-tools/README.md) |
| 03 | Infrastructure as Code | [README](./03-infrastructure-as-code/README.md) |
| 04 | Containerization | [README](./04-containerization/README.md) |
| 05 | Container Orchestration | [README](./05-container-orchestration/README.md) |
| 06 | GitOps | [README](./06-gitops/README.md) |
| 07 | Artifact Management | [README](./07-artifact-management/README.md) |
| 08 | Platform Engineering | [README](./08-platform-engineering/README.md) |

### 📊 Diagrams & Images
| Diagram | Image |
|---------|-------|
| [1.1-devops-lifecycle.drawio](./01-devops-fundamentals/1.1-devops-lifecycle.drawio) | [PNG](./01-devops-fundamentals/1.1-devops-lifecycle.png) |
| [1.2-devops-principles.drawio](./01-devops-fundamentals/1.2-devops-principles.drawio) | [PNG](./01-devops-fundamentals/1.2-devops-principles.png) |
| [1.3-dora-metrics.drawio](./01-devops-fundamentals/1.3-dora-metrics.drawio) | [PNG](./01-devops-fundamentals/1.3-dora-metrics.png) |
| [2.1-azure-devops-architecture.drawio](./02-cicd-tools/2.1-azure-devops-architecture.drawio) | [PNG](./02-cicd-tools/2.1-azure-devops-architecture.png) |
| [2.2-github-actions-workflow.drawio](./02-cicd-tools/2.2-github-actions-workflow.drawio) | [PNG](./02-cicd-tools/2.2-github-actions-workflow.png) |
| [2.3-cicd-tools-comparison.drawio](./02-cicd-tools/2.3-cicd-tools-comparison.drawio) | [PNG](./02-cicd-tools/2.3-cicd-tools-comparison.png) |
| [2.4-jenkins-architecture.drawio](./02-cicd-tools/2.4-jenkins-architecture.drawio) | [PNG](./02-cicd-tools/2.4-jenkins-architecture.png) |
| [3.1-terraform-workflow.drawio](./03-infrastructure-as-code/3.1-terraform-workflow.drawio) | [PNG](./03-infrastructure-as-code/3.1-terraform-workflow.png) |
| [3.2-iac-tools-comparison.drawio](./03-infrastructure-as-code/3.2-iac-tools-comparison.drawio) | [PNG](./03-infrastructure-as-code/3.2-iac-tools-comparison.png) |
| [4.1-docker-architecture.drawio](./04-containerization/4.1-docker-architecture.drawio) | [PNG](./04-containerization/4.1-docker-architecture.png) |
| [4.2-container-security-layers.drawio](./04-containerization/4.2-container-security-layers.drawio) | [PNG](./04-containerization/4.2-container-security-layers.png) |
| [5.1-kubernetes-architecture.drawio](./05-container-orchestration/5.1-kubernetes-architecture.drawio) | [PNG](./05-container-orchestration/5.1-kubernetes-architecture.png) |
| [5.2-kubernetes-resources.drawio](./05-container-orchestration/5.2-kubernetes-resources.drawio) | [PNG](./05-container-orchestration/5.2-kubernetes-resources.png) |
| [5.3-managed-k8s-comparison.drawio](./05-container-orchestration/5.3-managed-k8s-comparison.drawio) | [PNG](./05-container-orchestration/5.3-managed-k8s-comparison.png) |
| [6.1-gitops-flow.drawio](./06-gitops/6.1-gitops-flow.drawio) | [PNG](./06-gitops/6.1-gitops-flow.png) |
| [6.2-argocd-workflow.drawio](./06-gitops/6.2-argocd-workflow.drawio) | [PNG](./06-gitops/6.2-argocd-workflow.png) |
| [7.1-artifact-management.drawio](./07-artifact-management/7.1-artifact-management.drawio) | [PNG](./07-artifact-management/7.1-artifact-management.png) |
| [8.1-internal-developer-platform.drawio](./08-platform-engineering/8.1-internal-developer-platform.drawio) | [PNG](./08-platform-engineering/8.1-internal-developer-platform.png) |
| [8.2-platform-engineering-tools.drawio](./08-platform-engineering/8.2-platform-engineering-tools.drawio) | [PNG](./08-platform-engineering/8.2-platform-engineering-tools.png) |

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
