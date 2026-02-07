# Cloud-Native Design Principles

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Service Comparison](../05-service-comparison/README.md) | [Next: Multi-Cloud & Hybrid Strategies](../07-multi-cloud-hybrid-strategies/README.md)

---

## 6.1 The Twelve-Factor App

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

## 6.2 Cloud-Native Patterns

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

## Diagrams in This Section

- [6.1-twelve-factor-app.drawio](./6.1-twelve-factor-app.drawio)
- [6.2-cloud-native-patterns.drawio](./6.2-cloud-native-patterns.drawio)
