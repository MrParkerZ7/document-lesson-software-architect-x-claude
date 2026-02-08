# Lesson 07: Networking

## Overview

Networking knowledge is fundamental for Software Architects designing distributed systems and cloud-native applications. This lesson covers networking fundamentals, cloud networking concepts, and modern networking patterns like service mesh.

---

## Learning Objectives

By the end of this lesson, you will be able to:

- Understand core networking concepts (TCP/IP, DNS, HTTP)
- Design cloud network architectures
- Implement load balancing strategies
- Configure service mesh for microservices
- Design and manage API gateways

---

## 📁 Files in This Lesson

### 📚 Sub-Lessons
| # | Topic | README |
|---|-------|--------|
| 01 | Networking Fundamentals | [README](./01-networking-fundamentals/README.md) |
| 02 | Cloud Networking | [README](./02-cloud-networking/README.md) |
| 03 | Load Balancing | [README](./03-load-balancing/README.md) |
| 04 | Service Mesh | [README](./04-service-mesh/README.md) |
| 05 | API Gateway | [README](./05-api-gateway/README.md) |

### 📊 Diagrams & Images
| Diagram | Image |
|---------|-------|
| [1.1-osi-model.drawio](./01-networking-fundamentals/1.1-osi-model.drawio) | [PNG](./01-networking-fundamentals/1.1-osi-model.png) |
| [1.2-tcp-udp-comparison.drawio](./01-networking-fundamentals/1.2-tcp-udp-comparison.drawio) | [PNG](./01-networking-fundamentals/1.2-tcp-udp-comparison.png) |
| [1.3-dns-resolution.drawio](./01-networking-fundamentals/1.3-dns-resolution.drawio) | [PNG](./01-networking-fundamentals/1.3-dns-resolution.png) |
| [1.4-http-overview.drawio](./01-networking-fundamentals/1.4-http-overview.drawio) | [PNG](./01-networking-fundamentals/1.4-http-overview.png) |
| [2.1-virtual-network.drawio](./02-cloud-networking/2.1-virtual-network.drawio) | [PNG](./02-cloud-networking/2.1-virtual-network.png) |
| [2.2-private-connectivity.drawio](./02-cloud-networking/2.2-private-connectivity.drawio) | [PNG](./02-cloud-networking/2.2-private-connectivity.png) |
| [3.1-load-balancing-types.drawio](./03-load-balancing/3.1-load-balancing-types.drawio) | [PNG](./03-load-balancing/3.1-load-balancing-types.png) |
| [3.2-azure-load-balancing.drawio](./03-load-balancing/3.2-azure-load-balancing.drawio) | [PNG](./03-load-balancing/3.2-azure-load-balancing.png) |
| [4.1-service-mesh.drawio](./04-service-mesh/4.1-service-mesh.drawio) | [PNG](./04-service-mesh/4.1-service-mesh.png) |
| [5.1-api-gateway.drawio](./05-api-gateway/5.1-api-gateway.drawio) | [PNG](./05-api-gateway/5.1-api-gateway.png) |

---

## 1. Networking Fundamentals

### 1.1 OSI Model

| Layer | Name | Protocols/Examples |
|-------|------|-------------------|
| 7 | Application | HTTP, HTTPS, DNS, SMTP |
| 6 | Presentation | SSL/TLS, Encryption |
| 5 | Session | Session management |
| 4 | Transport | TCP, UDP |
| 3 | Network | IP, ICMP, Routing |
| 2 | Data Link | Ethernet, MAC addresses |
| 1 | Physical | Cables, signals |

### 1.2 TCP/IP

**TCP (Transmission Control Protocol)**:
- Connection-oriented
- Reliable delivery
- Ordered packets
- Use cases: HTTP, FTP, SMTP, databases

**UDP (User Datagram Protocol)**:
- Connectionless
- No guaranteed delivery
- Lower latency
- Use cases: DNS, video streaming, gaming, VoIP

### 1.3 DNS (Domain Name System)

| Type | Purpose | Example |
|------|---------|---------|
| A | IPv4 address | example.com to 93.184.216.34 |
| AAAA | IPv6 address | example.com to 2606:2800:220:1:... |
| CNAME | Canonical name | www to example.com |
| MX | Mail server | example.com to mail.example.com |
| TXT | Text record | SPF, DKIM, verification |

### 1.4 HTTP/HTTPS

| Method | Purpose | Idempotent | Safe |
|--------|---------|------------|------|
| GET | Retrieve resource | Yes | Yes |
| POST | Create resource | No | No |
| PUT | Replace resource | Yes | No |
| PATCH | Partial update | No | No |
| DELETE | Remove resource | Yes | No |

---

## 2. Cloud Networking

### 2.1 Virtual Networks

- Isolated network environments
- Customizable address spaces
- Subnet segmentation for tiers

### 2.2 Network Security Groups

- Virtual firewall rules
- Priority-based evaluation
- Inbound/outbound filtering

### 2.3 VNet Peering

- Low latency connectivity
- Cross-region support
- Stays on provider backbone

### 2.4 Private Endpoints

- Secure PaaS access via private IP
- No public internet exposure

### 2.5 ExpressRoute / Direct Connect

- Private dedicated connectivity
- Predictable performance
- Up to 100 Gbps bandwidth

---

## 3. Load Balancing

### 3.1 Load Balancing Types

| Type | Layer | Features |
|------|-------|----------|
| Layer 4 | Transport | TCP/UDP, fast, simple |
| Layer 7 | Application | Content-based, SSL termination |

### 3.2 Load Balancing Algorithms

| Algorithm | Use Case |
|-----------|----------|
| Round Robin | Equal capacity servers |
| Weighted Round Robin | Mixed capacity servers |
| Least Connections | Long-lived connections |
| IP Hash | Session persistence |

### 3.3 Azure Load Balancing Options

| Service | Layer | Scope |
|---------|-------|-------|
| Azure Load Balancer | L4 | Regional |
| Application Gateway | L7 | Regional |
| Front Door | L7 | Global |
| Traffic Manager | DNS | Global |

---

## 4. Service Mesh

### 4.1 Service Mesh Features

| Feature | Description |
|---------|-------------|
| Traffic Management | Load balancing, routing, retries |
| Security | mTLS, authorization policies |
| Observability | Metrics, tracing, logging |
| Resilience | Circuit breaking, fault injection |

### 4.2 Service Mesh Comparison

| Feature | Istio | Linkerd | Consul Connect |
|---------|-------|---------|----------------|
| Proxy | Envoy | linkerd2-proxy | Envoy |
| Complexity | High | Low | Medium |
| Performance | Good | Excellent | Good |

---

## 5. API Gateway

### 5.1 API Gateway Functions

- Request Routing
- Authentication / Authorization
- Rate Limiting / Throttling
- Request/Response Transformation
- Caching
- SSL Termination
- Monitoring / Analytics

### 5.2 API Gateway Comparison

| Feature | Azure APIM | AWS API Gateway | Kong |
|---------|------------|-----------------|------|
| Type | Managed | Managed | Self-hosted/Cloud |
| Protocols | REST, SOAP, WebSocket | REST, WebSocket | REST, gRPC, GraphQL |

---

## Practical Exercises

1. **VNet Design**: Design a hub-spoke network topology for a multi-tier application
2. **Load Balancer**: Configure Application Gateway with path-based routing
3. **Service Mesh**: Deploy Istio and configure traffic splitting for canary deployment
4. **API Gateway**: Set up Azure APIM with rate limiting and JWT validation

---

## Further Reading

- Computer Networking: A Top-Down Approach - Kurose and Ross
- Network Programmability and Automation - Jason Edelman
- Istio Documentation
- Azure Networking Documentation
- Zero Trust Networks - Evan Gilman

---

## Summary

Networking is the backbone of distributed systems and cloud architectures. Understanding fundamentals like TCP/IP, DNS, and HTTP, combined with cloud networking concepts like VNets, load balancing, and private connectivity, enables architects to design secure, performant, and resilient network architectures. Modern patterns like service mesh and API gateways provide additional capabilities for managing complex microservices environments.
