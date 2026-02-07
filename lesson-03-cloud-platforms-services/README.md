# Lesson 03: Cloud Platforms & Services ☁️

## 📋 Overview

Cloud computing has transformed how organizations build, deploy, and scale applications. As a Software Architect, understanding major cloud platforms and their services is essential for designing cost-effective, scalable, and resilient solutions.

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Understand cloud computing models (IaaS, PaaS, SaaS)
- Navigate services across AWS, Azure, and GCP
- Design cloud-native architectures
- Make informed decisions about cloud service selection
- Implement multi-cloud and hybrid cloud strategies

---

## Sub-lessons

| # | Topic | Diagrams |
|---|-------|----------|
| 01 | [Cloud Computing Fundamentals](./01-cloud-computing-fundamentals/README.md) | 2 |
| 02 | [Microsoft Azure](./02-microsoft-azure/README.md) | 3 |
| 03 | [Amazon Web Services](./03-amazon-web-services/README.md) | 3 |
| 04 | [Google Cloud Platform](./04-google-cloud-platform/README.md) | 2 |
| 05 | [Service Comparison](./05-service-comparison/README.md) | 3 |
| 06 | [Cloud-Native Design Principles](./06-cloud-native-design-principles/README.md) | 2 |
| 07 | [Multi-Cloud & Hybrid Strategies](./07-multi-cloud-hybrid-strategies/README.md) | 2 |
| 08 | [Cost Optimization & Well-Architected](./08-cost-optimization-well-architected/README.md) | 2 |

---

## 1. 💡 Cloud Computing Fundamentals

### 1.1 Service Models

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

### 1.2 Deployment Models

| Model | Description | Use Case |
|-------|-------------|----------|
| **Public Cloud** | Shared infrastructure, multi-tenant | Most workloads, startups |
| **Private Cloud** | Dedicated infrastructure | Compliance, sensitive data |
| **Hybrid Cloud** | Mix of public and private | Gradual migration, burst capacity |
| **Multi-Cloud** | Multiple public cloud providers | Avoid vendor lock-in, best-of-breed |

---

## 2. 🔷 Microsoft Azure

### 2.1 Compute Services

| Service | Description | Use Case |
|---------|-------------|----------|
| **Azure VMs** | Virtual machines (IaaS) | Lift-and-shift, custom workloads |
| **App Service** | Managed web hosting (PaaS) | Web apps, APIs, mobile backends |
| **Azure Functions** | Serverless compute | Event-driven, microservices |
| **AKS** | Managed Kubernetes | Container orchestration |
| **Container Apps** | Serverless containers | Microservices without K8s complexity |
| **Azure Batch** | Large-scale parallel computing | HPC, rendering, simulations |

### 2.2 Data Services

| Service | Type | Use Case |
|---------|------|----------|
| **Azure SQL** | Relational (SQL Server) | OLTP, traditional apps |
| **Cosmos DB** | Multi-model NoSQL | Global distribution, low latency |
| **Azure Database for PostgreSQL/MySQL** | Managed open-source | Open-source workloads |
| **Azure Synapse** | Analytics | Data warehousing, big data |
| **Azure Cache for Redis** | In-memory cache | Session store, caching |
| **Azure Blob Storage** | Object storage | Files, backups, data lakes |

### 2.3 Integration Services

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

### 2.4 Azure Architecture Example

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

## 3. 🟠 Amazon Web Services (AWS)

### 3.1 Compute Services

| Service | Description | Use Case |
|---------|-------------|----------|
| **EC2** | Virtual machines | General compute, custom workloads |
| **Lambda** | Serverless functions | Event-driven, APIs |
| **ECS** | Container orchestration | Docker workloads |
| **EKS** | Managed Kubernetes | K8s workloads |
| **Fargate** | Serverless containers | Containers without servers |
| **Elastic Beanstalk** | PaaS for web apps | Simple deployments |

### 3.2 Data Services

| Service | Type | Use Case |
|---------|------|----------|
| **RDS** | Managed relational DB | MySQL, PostgreSQL, SQL Server |
| **Aurora** | Cloud-native relational | High performance MySQL/PostgreSQL |
| **DynamoDB** | NoSQL key-value | High scale, low latency |
| **Redshift** | Data warehouse | Analytics, BI |
| **ElastiCache** | In-memory cache | Redis, Memcached |
| **S3** | Object storage | Files, static content, data lakes |

### 3.3 Integration Services

| Service | Purpose |
|---------|---------|
| **SQS** | Message queuing |
| **SNS** | Pub/sub notifications |
| **EventBridge** | Event bus |
| **Step Functions** | Workflow orchestration |
| **API Gateway** | API management |
| **Kinesis** | Real-time streaming |

### 3.4 AWS Architecture Example

```
                    ┌─────────────────┐
                    │   CloudFront    │
                    │      (CDN)      │
                    └────────┬────────┘
                             │
                    ┌────────┴────────┐
                    │   API Gateway   │
                    └────────┬────────┘
                             │
              ┌──────────────┼──────────────┐
              ▼              ▼              ▼
        ┌──────────┐  ┌──────────┐  ┌──────────┐
        │  Lambda  │  │  Lambda  │  │   ECS    │
        │(Function)│  │(Function)│  │(Service) │
        └────┬─────┘  └────┬─────┘  └────┬─────┘
             │             │             │
             └─────────────┼─────────────┘
                           │
              ┌────────────┼────────────┐
              ▼            ▼            ▼
        ┌──────────┐ ┌──────────┐ ┌──────────┐
        │  Aurora  │ │ DynamoDB │ │   S3     │
        └──────────┘ └──────────┘ └──────────┘
```

---

## 4. 🔵 Google Cloud Platform (GCP)

### 4.1 Compute Services

| Service | Description | Use Case |
|---------|-------------|----------|
| **Compute Engine** | Virtual machines | General compute |
| **Cloud Functions** | Serverless functions | Event-driven |
| **Cloud Run** | Serverless containers | Stateless containers |
| **GKE** | Managed Kubernetes | Container orchestration |
| **App Engine** | PaaS | Web applications |

### 4.2 Data Services

| Service | Type | Use Case |
|---------|------|----------|
| **Cloud SQL** | Managed relational | MySQL, PostgreSQL, SQL Server |
| **Cloud Spanner** | Global relational | Global, strongly consistent |
| **Firestore** | Document NoSQL | Mobile, web apps |
| **Bigtable** | Wide-column NoSQL | IoT, time-series, analytics |
| **BigQuery** | Data warehouse | Analytics, ML |
| **Cloud Storage** | Object storage | Files, backups |

### 4.3 GCP Differentiators

| Feature | Description |
|---------|-------------|
| **BigQuery** | Serverless, petabyte-scale analytics |
| **Cloud Spanner** | Globally distributed, strongly consistent |
| **Anthos** | Hybrid/multi-cloud Kubernetes |
| **Vertex AI** | Unified ML platform |
| **Cloud Run** | Fully managed serverless containers |

---

## 5. ⚖️ Service Comparison

### 5.1 Compute Comparison

| Capability | Azure | AWS | GCP |
|------------|-------|-----|-----|
| **VMs** | Azure VMs | EC2 | Compute Engine |
| **Serverless Functions** | Azure Functions | Lambda | Cloud Functions |
| **Containers (Managed)** | Container Apps | Fargate | Cloud Run |
| **Kubernetes** | AKS | EKS | GKE |
| **PaaS** | App Service | Elastic Beanstalk | App Engine |

### 5.2 Database Comparison

| Capability | Azure | AWS | GCP |
|------------|-------|-----|-----|
| **Managed SQL** | Azure SQL | RDS/Aurora | Cloud SQL |
| **NoSQL Document** | Cosmos DB | DynamoDB | Firestore |
| **NoSQL Wide-column** | Cosmos DB | DynamoDB | Bigtable |
| **Data Warehouse** | Synapse | Redshift | BigQuery |
| **Cache** | Azure Cache | ElastiCache | Memorystore |

### 5.3 Storage Comparison

| Capability | Azure | AWS | GCP |
|------------|-------|-----|-----|
| **Object Storage** | Blob Storage | S3 | Cloud Storage |
| **File Storage** | Azure Files | EFS | Filestore |
| **Block Storage** | Managed Disks | EBS | Persistent Disk |
| **Archive** | Archive Storage | Glacier | Archive Storage |

---

## 6. 🏗️ Cloud-Native Design Principles

### 6.1 The Twelve-Factor App

| Factor | Description |
|--------|-------------|
| 1. **Codebase** | One codebase tracked in version control |
| 2. **Dependencies** | Explicitly declare and isolate dependencies |
| 3. **Config** | Store config in the environment |
| 4. **Backing Services** | Treat backing services as attached resources |
| 5. **Build, Release, Run** | Strictly separate build and run stages |
| 6. **Processes** | Execute the app as stateless processes |
| 7. **Port Binding** | Export services via port binding |
| 8. **Concurrency** | Scale out via the process model |
| 9. **Disposability** | Maximize robustness with fast startup/shutdown |
| 10. **Dev/Prod Parity** | Keep dev, staging, and prod similar |
| 11. **Logs** | Treat logs as event streams |
| 12. **Admin Processes** | Run admin tasks as one-off processes |

### 6.2 Cloud-Native Patterns

```
┌─────────────────────────────────────────────────────────┐
│              Cloud-Native Application                    │
│                                                         │
│  ┌──────────┐  ┌──────────┐  ┌──────────┐             │
│  │Container │  │Container │  │Container │              │
│  │(Service) │  │(Service) │  │(Service) │              │
│  └────┬─────┘  └────┬─────┘  └────┬─────┘             │
│       │             │             │                     │
│  ┌────┴─────────────┴─────────────┴────┐              │
│  │         Service Mesh (Istio)         │              │
│  └──────────────────┬──────────────────┘              │
│                     │                                   │
│  ┌──────────────────┴──────────────────┐              │
│  │    Kubernetes / Container Platform   │              │
│  └─────────────────────────────────────┘              │
│                                                         │
│  ┌─────────┐ ┌─────────┐ ┌─────────┐ ┌─────────┐     │
│  │Observ.  │ │Security │ │CI/CD    │ │Config   │     │
│  │(Prom)   │ │(Vault)  │ │(ArgoCD) │ │(Consul) │     │
│  └─────────┘ └─────────┘ └─────────┘ └─────────┘     │
└─────────────────────────────────────────────────────────┘
```

---

## 7. 🌐 Multi-Cloud & Hybrid Strategies

### 7.1 Multi-Cloud Architecture

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

### 7.2 When to Use Multi-Cloud

| Reason | Description |
|--------|-------------|
| **Avoid Lock-in** | Reduce dependency on single vendor |
| **Best-of-Breed** | Use best service from each provider |
| **Compliance** | Meet data residency requirements |
| **Disaster Recovery** | Cross-cloud DR |
| **Cost Optimization** | Leverage competitive pricing |

### 7.3 Hybrid Cloud Patterns

| Pattern | Description | Use Case |
|---------|-------------|----------|
| **Cloud Bursting** | Extend to cloud during peak | Seasonal workloads |
| **Tiered Hybrid** | Different tiers in different locations | Sensitive data on-prem |
| **Analytics Hybrid** | On-prem data, cloud analytics | Legacy systems + modern analytics |
| **DR Hybrid** | Production on-prem, DR in cloud | Cost-effective DR |

---

## 8. 💰 Cost Optimization

### 8.1 Cost Management Strategies

| Strategy | Description |
|----------|-------------|
| **Right-sizing** | Match resources to actual needs |
| **Reserved Instances** | Commit for 1-3 years for discounts |
| **Spot/Preemptible** | Use excess capacity at discount |
| **Auto-scaling** | Scale resources based on demand |
| **Storage Tiering** | Move cold data to cheaper storage |
| **Serverless** | Pay only for actual usage |

### 8.2 Cost Comparison Tools

| Provider | Tool |
|----------|------|
| **Azure** | Azure Cost Management, Pricing Calculator |
| **AWS** | AWS Cost Explorer, Trusted Advisor |
| **GCP** | Cloud Billing, Pricing Calculator |
| **Third-party** | CloudHealth, Spot.io, Kubecost |

---

## 9. 🏆 Well-Architected Frameworks

### 9.1 Common Pillars

| Pillar | Description |
|--------|-------------|
| **Security** | Protect data and systems |
| **Reliability** | Recover from failures, meet availability |
| **Performance** | Use resources efficiently |
| **Cost Optimization** | Avoid unnecessary costs |
| **Operational Excellence** | Run and monitor systems effectively |
| **Sustainability** | Minimize environmental impact (GCP/AWS) |

### 9.2 Framework Resources

- **Azure**: [Well-Architected Framework](https://docs.microsoft.com/azure/architecture/framework/)
- **AWS**: [Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- **GCP**: [Architecture Framework](https://cloud.google.com/architecture/framework)

---

## 🏋️ Practical Exercises

1. **Service Mapping**: Map an on-premises architecture to equivalent cloud services
2. **Cost Estimation**: Use pricing calculators to estimate monthly costs for a workload
3. **Multi-Region Design**: Design a globally distributed application on your cloud of choice
4. **Well-Architected Review**: Conduct a review using the well-architected framework

---

## 📖 Further Reading

- "Cloud Native Patterns" - Cornelia Davis
- "Architecting the Cloud" - Michael Kavis
- "Cloud Strategy" - Gregor Hohpe
- Provider documentation and whitepapers
- Cloud provider certification paths

---

## 📝 Summary

Cloud platforms provide a vast array of services that enable organizations to build scalable, resilient applications without managing physical infrastructure. Understanding the service landscape across major providers, cloud-native design principles, and cost optimization strategies is essential for modern software architects. The key is matching services to requirements while maintaining flexibility and avoiding unnecessary complexity.
