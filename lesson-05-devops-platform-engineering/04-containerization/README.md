# 4. Containerization

[<< Previous: Infrastructure as Code](../03-infrastructure-as-code/README.md) | [Back to Lesson 05](../README.md) | [Next: Container Orchestration >>](../05-container-orchestration/README.md)

---

## Overview

Containerization packages applications and their dependencies into portable, isolated units called containers. This section covers Docker fundamentals and container security best practices.

---

## Learning Objectives

- Understand container concepts and Docker architecture
- Write optimized Dockerfiles following best practices
- Implement container security measures

---

## 4.1 Docker Architecture

Docker uses a client-server architecture with the Docker daemon managing containers, images, networks, and volumes. Key concepts include:

- **Images**: Read-only templates for containers
- **Containers**: Running instances of images
- **Volumes**: Persistent data storage
- **Networks**: Container communication

**Best Practices**:
- Use multi-stage builds
- Run as non-root user
- Use specific version tags
- Implement health checks

---

## 4.2 Container Security

Security must be applied at multiple layers:

**Image Security**:
- Use minimal base images (Alpine, Distroless)
- Scan for vulnerabilities (Trivy, Snyk)
- Sign and verify images
- Do not run as root

**Runtime Security**:
- Read-only file systems
- Resource limits (CPU, memory)
- Network policies
- Seccomp profiles

**Registry Security**:
- Private registries
- Access control
- Vulnerability scanning

---

## Diagrams

| Diagram | Description |
|---------|-------------|
| [4.1-docker-architecture.drawio](./4.1-docker-architecture.drawio) | Docker architecture and components |
| [4.2-container-security-layers.drawio](./4.2-container-security-layers.drawio) | Container security layers visualization |

---

## Key Takeaways

1. Multi-stage builds reduce image size and attack surface
2. Always run containers as non-root users
3. Layer caching optimizes build times
4. Container security spans image, runtime, and registry layers

---

[<< Previous: Infrastructure as Code](../03-infrastructure-as-code/README.md) | [Back to Lesson 05](../README.md) | [Next: Container Orchestration >>](../05-container-orchestration/README.md)
