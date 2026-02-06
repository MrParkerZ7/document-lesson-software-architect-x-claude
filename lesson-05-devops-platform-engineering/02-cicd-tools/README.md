# 2. CI/CD Tools

[<< Previous: DevOps Fundamentals](../01-devops-fundamentals/README.md) | [Back to Lesson 05](../README.md) | [Next: Infrastructure as Code >>](../03-infrastructure-as-code/README.md)

---

## Overview

CI/CD (Continuous Integration/Continuous Delivery) tools automate the software delivery process, from code commit to production deployment. This section covers the major CI/CD platforms and their configurations.

---

## Learning Objectives

- Understand different CI/CD platforms and their strengths
- Write pipeline configurations for Azure DevOps, GitHub Actions, and Jenkins
- Compare CI/CD tools to make informed technology decisions

---

## 2.1 Azure DevOps

**Components**:
- **Azure Repos**: Git repositories
- **Azure Pipelines**: CI/CD pipelines
- **Azure Boards**: Work tracking
- **Azure Artifacts**: Package management
- **Azure Test Plans**: Testing tools

---

## 2.2 GitHub Actions

GitHub Actions provides native CI/CD capabilities integrated with GitHub repositories, using workflow files defined in YAML format.

---

## 2.3 Jenkins

Jenkins is an open-source automation server that supports building, deploying, and automating any project using declarative or scripted pipelines.

---

## 2.4 CI/CD Tool Comparison

| Feature | Azure DevOps | GitHub Actions | GitLab CI | Jenkins |
|---------|--------------|----------------|-----------|---------|
| **Hosting** | Cloud/Server | Cloud | Cloud/Self | Self-hosted |
| **Config** | YAML | YAML | YAML | Groovy |
| **Integration** | Azure | GitHub | GitLab | Any |
| **Marketplace** | Extensions | Actions | Templates | Plugins |
| **Free Tier** | 1800 min/mo | 2000 min/mo | 400 min/mo | Free |

---

## Diagrams

| Diagram | Description |
|---------|-------------|
| [2.1-azure-devops-architecture.drawio](./2.1-azure-devops-architecture.drawio) | Azure DevOps platform architecture |
| [2.2-github-actions-workflow.drawio](./2.2-github-actions-workflow.drawio) | GitHub Actions workflow visualization |
| [2.3-cicd-tools-comparison.drawio](./2.3-cicd-tools-comparison.drawio) | Comparison of CI/CD tools |
| [2.4-jenkins-architecture.drawio](./2.4-jenkins-architecture.drawio) | Jenkins architecture and pipeline flow |

---

## Key Takeaways

1. Choose CI/CD tools based on your ecosystem (Azure, GitHub, GitLab)
2. YAML-based configurations provide version control for pipelines
3. All major platforms support similar concepts with different syntax
4. Consider self-hosted options for compliance and cost optimization

---

[<< Previous: DevOps Fundamentals](../01-devops-fundamentals/README.md) | [Back to Lesson 05](../README.md) | [Next: Infrastructure as Code >>](../03-infrastructure-as-code/README.md)
