# Cloud Computing Fundamentals

> **Navigation**: [Back to Lesson Overview](../README.md) | [Next: Microsoft Azure](../02-microsoft-azure/README.md)

---

## 1.1 Service Models

```
┌─────────────────────────────────────────────────────────────────┐
│                         SaaS                                    │
│                    (Gmail, Salesforce)                          │
│    ┌───────────────────────────────────────────────────────┐   │
│    │                      PaaS                              │   │
│    │               (App Service, Heroku)                    │   │
│    │    ┌─────────────────────────────────────────────┐    │   │
│    │    │                  IaaS                        │    │   │
│    │    │              (EC2, Azure VMs)                │    │   │
│    │    │    ┌───────────────────────────────────┐    │    │   │
│    │    │    │           On-Premises              │    │    │   │
│    │    │    │        (Your Data Center)          │    │    │   │
│    │    │    └───────────────────────────────────┘    │    │   │
│    │    └─────────────────────────────────────────────┘    │   │
│    └───────────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────────────┘
```

| Model | You Manage | Provider Manages | Examples |
|-------|------------|------------------|----------|
| **IaaS** | Apps, Data, Runtime, Middleware, OS | Virtualization, Servers, Storage, Networking | EC2, Azure VMs, GCE |
| **PaaS** | Apps, Data | Runtime, Middleware, OS, Infrastructure | App Service, Cloud Run, Elastic Beanstalk |
| **SaaS** | Nothing (just use it) | Everything | Office 365, Salesforce, Gmail |

## 1.2 Deployment Models

| Model | Description | Use Case |
|-------|-------------|----------|
| **Public Cloud** | Shared infrastructure, multi-tenant | Most workloads, startups |
| **Private Cloud** | Dedicated infrastructure | Compliance, sensitive data |
| **Hybrid Cloud** | Mix of public and private | Gradual migration, burst capacity |
| **Multi-Cloud** | Multiple public cloud providers | Avoid vendor lock-in, best-of-breed |

---

## Diagrams in This Section

- [1.1-service-models-iaas-paas-saas.drawio](./1.1-service-models-iaas-paas-saas.drawio)
- [1.2-deployment-models.drawio](./1.2-deployment-models.drawio)
