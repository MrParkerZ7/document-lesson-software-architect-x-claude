# 2. Cloud Networking

[Previous: Networking Fundamentals](../01-networking-fundamentals/README.md) | [Next: Load Balancing](../03-load-balancing/README.md)

---

## Overview

Cloud networking extends traditional networking concepts to the cloud environment. This section covers virtual networks, network security groups, VNet peering, private endpoints, and dedicated connectivity options like ExpressRoute.

---

## Learning Objectives

By the end of this section, you will be able to:

- Design virtual network architectures with proper subnet segmentation
- Configure network security groups for traffic filtering
- Implement VNet peering for network interconnectivity
- Set up private endpoints for secure service access
- Understand ExpressRoute/Direct Connect for hybrid connectivity

---

## 2.1 Virtual Networks

Virtual Networks (VNet/VPC) provide isolated network environments with:
- Customizable address spaces (e.g., 10.0.0.0/16)
- Multiple subnets for different tiers (web, app, data)
- Network Security Groups (NSGs) for traffic filtering

---

## 2.2 Network Security Groups (NSGs)

NSGs act as virtual firewalls with rules for:
- Inbound and outbound traffic control
- Priority-based rule evaluation
- Source/destination filtering by IP, port, protocol

---

## 2.3 VNet Peering

Features:
- Low latency, high bandwidth
- Traffic stays on cloud provider backbone
- Cross-region peering supported
- Cross-subscription peering supported

---

## 2.4 Private Endpoints

Private endpoints enable:
- Secure access to PaaS services via private IP
- No exposure to public internet
- DNS integration for seamless resolution

---

## 2.5 ExpressRoute / Direct Connect

Benefits:
- Private connectivity (not over internet)
- Predictable performance
- Higher bandwidth (up to 100 Gbps)
- Lower latency

---

## Diagrams

| Diagram | Description |
|---------|-------------|
| [2.1-virtual-network.drawio](./2.1-virtual-network.drawio) | Virtual network architecture with subnets |
| [2.2-private-connectivity.drawio](./2.2-private-connectivity.drawio) | Private endpoints and ExpressRoute |

---

## Key Takeaways

1. **Virtual Networks** provide isolated network environments with customizable address spaces
2. **NSGs** control inbound and outbound traffic at subnet and NIC level
3. **VNet Peering** enables low-latency connectivity between virtual networks
4. **Private Endpoints** allow secure access to services through private IP addresses
5. **ExpressRoute** provides dedicated, high-bandwidth private connectivity

---

[Previous: Networking Fundamentals](../01-networking-fundamentals/README.md) | [Next: Load Balancing](../03-load-balancing/README.md)
