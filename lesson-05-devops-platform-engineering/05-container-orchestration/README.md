# 5. Container Orchestration

[<< Previous: Containerization](../04-containerization/README.md) | [Back to Lesson 05](../README.md) | [Next: GitOps >>](../06-gitops/README.md)

---

## Overview

Container orchestration automates the deployment, scaling, and management of containerized applications. This section covers Kubernetes architecture and managed Kubernetes services.

---

## Learning Objectives

- Understand Kubernetes architecture and components
- Write Kubernetes manifests for deployments, services, and ingress
- Compare managed Kubernetes services across cloud providers

---

## 5.1 Kubernetes Architecture

Kubernetes clusters consist of:

**Control Plane**:
- API Server - Central management point
- Scheduler - Assigns pods to nodes
- Controller Manager - Maintains desired state
- etcd - Cluster state storage

**Worker Nodes**:
- kubelet - Node agent
- Container runtime - Runs containers
- kube-proxy - Network proxy

---

## 5.2 Kubernetes Resources

Key Kubernetes resources:
- **Deployment**: Manages stateless application replicas
- **Service**: Provides stable networking for pods
- **Ingress**: HTTP/HTTPS routing
- **ConfigMap/Secret**: Configuration and sensitive data
- **PersistentVolume**: Persistent storage

---

## 5.3 Managed Kubernetes Services

| Service | Provider | Features |
|---------|----------|----------|
| **AKS** | Azure | Azure integration, Azure CNI, AAD integration |
| **EKS** | AWS | AWS integration, Fargate support |
| **GKE** | GCP | Autopilot mode, best-in-class networking |

---

## Diagrams

| Diagram | Description |
|---------|-------------|
| [5.1-kubernetes-architecture.drawio](./5.1-kubernetes-architecture.drawio) | Kubernetes cluster architecture |
| [5.2-kubernetes-resources.drawio](./5.2-kubernetes-resources.drawio) | Kubernetes resource types and relationships |
| [5.3-managed-k8s-comparison.drawio](./5.3-managed-k8s-comparison.drawio) | Comparison of managed Kubernetes services |

---

## Key Takeaways

1. Kubernetes provides declarative container orchestration
2. Control plane manages cluster state, worker nodes run workloads
3. Resource limits and probes are essential for production deployments
4. Managed services reduce operational burden

---

[<< Previous: Containerization](../04-containerization/README.md) | [Back to Lesson 05](../README.md) | [Next: GitOps >>](../06-gitops/README.md)
