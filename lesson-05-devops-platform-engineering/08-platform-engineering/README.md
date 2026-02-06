# 8. Platform Engineering

[<< Previous: Artifact Management](../07-artifact-management/README.md) | [Back to Lesson 05](../README.md)

---

## Overview

Platform Engineering extends DevOps by providing self-service capabilities to development teams through internal developer platforms (IDPs). This section covers platform engineering concepts and tools.

---

## Learning Objectives

- Understand Platform Engineering principles
- Design Internal Developer Platforms (IDPs)
- Evaluate platform engineering tools

---

## 8.1 Internal Developer Platform (IDP)

An IDP provides a unified experience for developers:

**Key Components**:
- **Developer Portal** - Central hub for developers
- **Service Catalog** - Discoverable services and APIs
- **Self-Service Infrastructure** - Provision resources on demand
- **Golden Paths** - Paved roads for common workflows
- **Observability** - Integrated monitoring and logging

**Layers**:
1. Developer Portal (Service Catalog, Documentation, Self-Service, Templates)
2. Platform Services (CI/CD Pipeline, IaC Templates, Secrets Management)
3. Observability (Monitoring, Logs, Alerts)
4. Infrastructure (Kubernetes, Cloud Services, Network)

---

## 8.2 Platform Engineering Tools

| Category | Tools |
|----------|-------|
| **Developer Portal** | Backstage, Port, Cortex |
| **Self-Service** | Crossplane, Kratix |
| **Templates** | Cookiecutter, Yeoman, Backstage |
| **Documentation** | Backstage TechDocs, ReadMe |

**Backstage** - Open-source platform for building developer portals (Spotify):
- Software catalog
- Tech docs
- Templates (scaffolding)
- Plugins ecosystem

**Crossplane** - Extends Kubernetes to manage cloud resources:
- Declarative infrastructure
- Self-service for developers
- Consistent API across clouds

---

## Diagrams

| Diagram | Description |
|---------|-------------|
| [8.1-internal-developer-platform.drawio](./8.1-internal-developer-platform.drawio) | IDP architecture and components |
| [8.2-platform-engineering-tools.drawio](./8.2-platform-engineering-tools.drawio) | Platform engineering tooling landscape |

---

## Key Takeaways

1. Platform Engineering reduces cognitive load on developers
2. IDPs provide self-service while maintaining governance
3. Golden paths guide developers toward best practices
4. Backstage has become the standard for developer portals

---

[<< Previous: Artifact Management](../07-artifact-management/README.md) | [Back to Lesson 05](../README.md)
