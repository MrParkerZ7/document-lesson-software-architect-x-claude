# Amazon Web Services (AWS)

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Microsoft Azure](../02-microsoft-azure/README.md) | [Next: Google Cloud Platform](../04-google-cloud-platform/README.md)

---

## 3.1 Compute Services

| Service | Description | Use Case |
|---------|-------------|----------|
| **EC2** | Virtual machines | General compute, custom workloads |
| **Lambda** | Serverless functions | Event-driven, APIs |
| **ECS** | Container orchestration | Docker workloads |
| **EKS** | Managed Kubernetes | K8s workloads |
| **Fargate** | Serverless containers | Containers without servers |
| **Elastic Beanstalk** | PaaS for web apps | Simple deployments |

## 3.2 Data Services

| Service | Type | Use Case |
|---------|------|----------|
| **RDS** | Managed relational DB | MySQL, PostgreSQL, SQL Server |
| **Aurora** | Cloud-native relational | High performance MySQL/PostgreSQL |
| **DynamoDB** | NoSQL key-value | High scale, low latency |
| **Redshift** | Data warehouse | Analytics, BI |
| **ElastiCache** | In-memory cache | Redis, Memcached |
| **S3** | Object storage | Files, static content, data lakes |

## 3.3 Integration Services

| Service | Purpose |
|---------|---------|
| **SQS** | Message queuing |
| **SNS** | Pub/sub notifications |
| **EventBridge** | Event bus |
| **Step Functions** | Workflow orchestration |
| **API Gateway** | API management |
| **Kinesis** | Real-time streaming |

## 3.4 AWS Architecture Example

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

## Diagrams in This Section

- [3.1-aws-compute-services.drawio](./3.1-aws-compute-services.drawio)
- [3.2-aws-data-services.drawio](./3.2-aws-data-services.drawio)
- [3.3-aws-architecture-example.drawio](./3.3-aws-architecture-example.drawio)
