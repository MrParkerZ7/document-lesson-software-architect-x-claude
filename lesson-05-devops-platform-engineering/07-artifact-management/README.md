# 7. Artifact Management

[<< Previous: GitOps](../06-gitops/README.md) | [Back to Lesson 05](../README.md) | [Next: Platform Engineering >>](../08-platform-engineering/README.md)

---

## Overview

Artifact management involves storing, versioning, and distributing build outputs including container images, packages, and binaries. This section covers container registries and package managers.

---

## Learning Objectives

- Understand artifact management concepts
- Configure and use container registries
- Manage packages across different ecosystems

---

## 7.1 Container Registries

| Registry | Provider | Features |
|----------|----------|----------|
| **Azure Container Registry** | Azure | Geo-replication, Tasks |
| **Amazon ECR** | AWS | Lifecycle policies |
| **Google Artifact Registry** | GCP | Multi-format support |
| **Docker Hub** | Docker | Public registry |
| **Harbor** | CNCF | Self-hosted, scanning |

**Best Practices**:
- Use immutable tags - Never overwrite existing tags
- Implement lifecycle policies - Clean up old images
- Enable vulnerability scanning - Scan images on push
- Geo-replicate for availability - Distribute images globally
- Use private registries - Control access to images

---

## 7.2 Package Managers

| Type | Tools |
|------|-------|
| **npm/Node** | Azure Artifacts, npm registry, Verdaccio |
| **NuGet/.NET** | Azure Artifacts, NuGet.org |
| **Maven/Java** | Nexus, Artifactory |
| **PyPI/Python** | Azure Artifacts, private PyPI |
| **Helm** | Harbor, ChartMuseum |

---

## Diagrams

| Diagram | Description |
|---------|-------------|
| [7.1-artifact-management.drawio](./7.1-artifact-management.drawio) | Artifact management ecosystem |

---

## Key Takeaways

1. Centralized artifact management improves security and reliability
2. Container registries are essential for containerized workflows
3. Private registries provide control over package distribution
4. Lifecycle policies prevent storage bloat

---

[<< Previous: GitOps](../06-gitops/README.md) | [Back to Lesson 05](../README.md) | [Next: Platform Engineering >>](../08-platform-engineering/README.md)
