# Microsoft Azure

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Cloud Computing Fundamentals](../01-cloud-computing-fundamentals/README.md) | [Next: Amazon Web Services](../03-amazon-web-services/README.md)

---

## 2.1 Compute Services

| Service | Description | Use Case |
|---------|-------------|----------|
| **Azure VMs** | Virtual machines (IaaS) | Lift-and-shift, custom workloads |
| **App Service** | Managed web hosting (PaaS) | Web apps, APIs, mobile backends |
| **Azure Functions** | Serverless compute | Event-driven, microservices |
| **AKS** | Managed Kubernetes | Container orchestration |
| **Container Apps** | Serverless containers | Microservices without K8s complexity |
| **Azure Batch** | Large-scale parallel computing | HPC, rendering, simulations |

## 2.2 Data Services

| Service | Type | Use Case |
|---------|------|----------|
| **Azure SQL** | Relational (SQL Server) | OLTP, traditional apps |
| **Cosmos DB** | Multi-model NoSQL | Global distribution, low latency |
| **Azure Database for PostgreSQL/MySQL** | Managed open-source | Open-source workloads |
| **Azure Synapse** | Analytics | Data warehousing, big data |
| **Azure Cache for Redis** | In-memory cache | Session store, caching |
| **Azure Blob Storage** | Object storage | Files, backups, data lakes |

## 2.3 Integration Services

```
┌──────────────────────────────────────────────────────────┐
│                Azure Integration Services                 │
│                                                          │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │ Service Bus │  │ Event Grid  │  │ Event Hubs  │     │
│  │  (Queues)   │  │  (Events)   │  │ (Streaming) │     │
│  └─────────────┘  └─────────────┘  └─────────────┘     │
│                                                          │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐     │
│  │Logic Apps   │  │API Mgmt     │  │ Data Factory│     │
│  │(Workflows)  │  │(Gateway)    │  │(ETL/ELT)    │     │
│  └─────────────┘  └─────────────┘  └─────────────┘     │
└──────────────────────────────────────────────────────────┘
```

## 2.4 Azure Architecture Example

```
                    ┌─────────────────┐
                    │   Azure Front   │
                    │      Door       │
                    └────────┬────────┘
                             │
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
        ┌──────────┐  ┌──────────┐  ┌──────────┐
        │ App Svc  │  │ App Svc  │  │ App Svc  │
        │ (Web)    │  │ (API)    │  │ (Worker) │
        └────┬─────┘  └────┬─────┘  └────┬─────┘
             │             │             │
             └─────────────┼─────────────┘
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
        ┌──────────┐ ┌──────────┐ ┌──────────┐
        │Azure SQL │ │Cosmos DB │ │Redis     │
        └──────────┘ └──────────┘ └──────────┘
```

---

## Diagrams in This Section

- [2.1-azure-compute-services.drawio](./2.1-azure-compute-services.drawio)
- [2.2-azure-data-services.drawio](./2.2-azure-data-services.drawio)
- [2.3-azure-architecture-example.drawio](./2.3-azure-architecture-example.drawio)
