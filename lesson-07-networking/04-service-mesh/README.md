# 4. Service Mesh

[Previous: Load Balancing](../03-load-balancing/README.md) | [Next: API Gateway](../05-api-gateway/README.md)

---

## Overview

A service mesh is a dedicated infrastructure layer for managing service-to-service communication in microservices architectures. This section covers service mesh concepts, Istio configuration, and comparison of popular solutions.

---

## Learning Objectives

By the end of this section, you will be able to:

- Understand the purpose and architecture of service mesh
- Configure traffic management with Istio
- Implement security features like mTLS
- Compare different service mesh solutions

---

## 4.1 What is a Service Mesh?

A service mesh uses sidecar proxies (like Envoy) alongside each service to handle:
- Traffic management
- Security (mTLS)
- Observability
- Resilience

---

## 4.2 Service Mesh Features

| Feature | Description |
|---------|-------------|
| Traffic Management | Load balancing, routing, retries, timeouts |
| Security | mTLS, authorization policies |
| Observability | Metrics, tracing, logging |
| Resilience | Circuit breaking, fault injection |

---

## 4.3 Istio Configuration

Key resources:
- **VirtualService**: Defines routing rules
- **DestinationRule**: Defines traffic policies and subsets
- **PeerAuthentication**: Configures mTLS
- **AuthorizationPolicy**: Defines access control

---

## 4.4 Service Mesh Comparison

| Feature | Istio | Linkerd | Consul Connect |
|---------|-------|---------|----------------|
| Proxy | Envoy | linkerd2-proxy | Envoy |
| Complexity | High | Low | Medium |
| Performance | Good | Excellent | Good |
| Features | Comprehensive | Core | Core + Service Discovery |

---

## Diagrams

| Diagram | Description |
|---------|-------------|
| [4.1-service-mesh.drawio](./4.1-service-mesh.drawio) | Service mesh architecture with sidecar proxies |

---

## Key Takeaways

1. **Service mesh** decouples networking from application code
2. **Traffic management** enables sophisticated routing and resilience
3. **mTLS** provides automatic encryption between services
4. **Choose the right mesh** based on complexity and feature needs

---

[Previous: Load Balancing](../03-load-balancing/README.md) | [Next: API Gateway](../05-api-gateway/README.md)
