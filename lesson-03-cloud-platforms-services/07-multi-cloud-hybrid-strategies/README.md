# Multi-Cloud & Hybrid Strategies

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Cloud-Native Design Principles](../06-cloud-native-design-principles/README.md) | [Next: Cost Optimization & Well-Architected](../08-cost-optimization-well-architected/README.md)

---

## 7.1 Multi-Cloud Architecture

```
┌─────────────────────────────────────────────────────────┐
│                    Multi-Cloud                           │
│                                                         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐    │
│  │    AWS      │  │    Azure    │  │    GCP      │    │
│  │  (Compute)  │  │  (Identity) │  │  (BigQuery) │    │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘    │
│         │                │                │            │
│         └────────────────┼────────────────┘            │
│                          │                             │
│               ┌──────────┴──────────┐                  │
│               │  Orchestration Layer │                  │
│               │  (Terraform, K8s)    │                  │
│               └─────────────────────┘                  │
└─────────────────────────────────────────────────────────┘
```

## 7.2 When to Use Multi-Cloud

| Reason | Description |
|--------|-------------|
| **Avoid Lock-in** | Reduce dependency on single vendor |
| **Best-of-Breed** | Use best service from each provider |
| **Compliance** | Meet data residency requirements |
| **Disaster Recovery** | Cross-cloud DR |
| **Cost Optimization** | Leverage competitive pricing |

## 7.3 Hybrid Cloud Patterns

| Pattern | Description | Use Case |
|---------|-------------|----------|
| **Cloud Bursting** | Extend to cloud during peak | Seasonal workloads |
| **Tiered Hybrid** | Different tiers in different locations | Sensitive data on-prem |
| **Analytics Hybrid** | On-prem data, cloud analytics | Legacy systems + modern analytics |
| **DR Hybrid** | Production on-prem, DR in cloud | Cost-effective DR |

---

## Diagrams in This Section

- [7.1-multi-cloud-architecture.drawio](./7.1-multi-cloud-architecture.drawio)
- [7.2-hybrid-cloud-patterns.drawio](./7.2-hybrid-cloud-patterns.drawio)
