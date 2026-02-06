# 6. GitOps

[<< Previous: Container Orchestration](../05-container-orchestration/README.md) | [Back to Lesson 05](../README.md) | [Next: Artifact Management >>](../07-artifact-management/README.md)

---

## Overview

GitOps is a set of practices that uses Git as the single source of truth for declarative infrastructure and applications. Changes are applied automatically through Git operations (pull requests, merges).

---

## Learning Objectives

- Understand GitOps principles and workflow
- Configure ArgoCD for GitOps deployments
- Implement drift detection and automatic synchronization

---

## 6.1 GitOps Principles

Core GitOps principles:

1. **Declarative**: Describe desired state
2. **Versioned**: Git as single source of truth
3. **Automated**: Changes applied automatically
4. **Auditable**: Complete history in Git

The GitOps flow:
- Git repository stores desired state
- GitOps operator (ArgoCD/Flux) monitors repository
- Operator synchronizes cluster to match Git
- Drift detection ensures consistency

---

## 6.2 ArgoCD Workflow

ArgoCD is a declarative, GitOps continuous delivery tool for Kubernetes.

**Key Features**:
- Application definitions in Git
- Automated synchronization
- Self-healing (prune, selfHeal)
- Multi-cluster support
- Web UI and CLI

---

## Diagrams

| Diagram | Description |
|---------|-------------|
| [6.1-gitops-flow.drawio](./6.1-gitops-flow.drawio) | GitOps workflow and principles |
| [6.2-argocd-workflow.drawio](./6.2-argocd-workflow.drawio) | ArgoCD synchronization workflow |

---

## Key Takeaways

1. Git becomes the single source of truth for both code and infrastructure
2. All changes go through Git (PR reviews, audit trail)
3. Automatic drift detection ensures cluster state matches Git
4. ArgoCD and Flux are the leading GitOps tools

---

[<< Previous: Container Orchestration](../05-container-orchestration/README.md) | [Back to Lesson 05](../README.md) | [Next: Artifact Management >>](../07-artifact-management/README.md)
