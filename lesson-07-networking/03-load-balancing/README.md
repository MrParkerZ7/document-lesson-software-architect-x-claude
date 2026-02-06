# 3. Load Balancing

[Previous: Cloud Networking](../02-cloud-networking/README.md) | [Next: Service Mesh](../04-service-mesh/README.md)

---

## Overview

Load balancing distributes network traffic across multiple servers to ensure high availability, reliability, and performance. This section covers load balancing types, algorithms, and Azure-specific options.

---

## Learning Objectives

By the end of this section, you will be able to:

- Differentiate between Layer 4 and Layer 7 load balancing
- Choose appropriate load balancing algorithms
- Select the right Azure load balancing service
- Configure Application Gateway with path-based routing

---

## 3.1 Load Balancing Types

**Layer 4 (Transport)**:
- TCP/UDP level
- Fast and simple
- IP + Port routing
- No content inspection

**Layer 7 (Application)**:
- HTTP/HTTPS level
- Content-based routing
- URL/Header routing
- SSL termination

---

## 3.2 Load Balancing Algorithms

| Algorithm | Description | Use Case |
|-----------|-------------|----------|
| Round Robin | Sequential distribution | Equal capacity servers |
| Weighted Round Robin | Based on server capacity | Mixed capacity servers |
| Least Connections | To server with fewest connections | Long-lived connections |
| IP Hash | Based on client IP | Session persistence |
| Least Response Time | To fastest responding server | Performance-critical |

---

## 3.3 Azure Load Balancing Options

| Service | Layer | Scope | Features |
|---------|-------|-------|----------|
| Azure Load Balancer | L4 | Regional | Basic TCP/UDP, fast |
| Application Gateway | L7 | Regional | WAF, SSL, URL routing |
| Front Door | L7 | Global | CDN, WAF, global routing |
| Traffic Manager | DNS | Global | DNS-based routing |

---

## Diagrams

| Diagram | Description |
|---------|-------------|
| [3.1-load-balancing-types.drawio](./3.1-load-balancing-types.drawio) | Layer 4 vs Layer 7 load balancing |
| [3.2-azure-load-balancing.drawio](./3.2-azure-load-balancing.drawio) | Azure load balancing options |

---

## Key Takeaways

1. **Layer 4** operates at transport layer, offering fast traffic distribution
2. **Layer 7** provides content-aware routing with advanced features
3. **Algorithm selection** depends on server capacity and persistence requirements
4. **Azure offers multiple options** based on scope and protocol needs

---

[Previous: Cloud Networking](../02-cloud-networking/README.md) | [Next: Service Mesh](../04-service-mesh/README.md)
