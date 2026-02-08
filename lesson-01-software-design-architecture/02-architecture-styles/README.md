# 🏛️ Architecture Styles

> **Navigation**: [⬅️ Back to Lesson Overview](../README.md) | [Previous: Design Patterns](../01-design-patterns/README.md) | [Next: Architecture Principles ➡️](../03-architecture-principles/README.md)

---

## 🏛️ 2.1 Monolithic Architecture

**📋 Description**: Single deployable unit containing all application functionality.

```
┌────────────────────────────────────────┐
│            🏛️ Monolithic App           │
│  ┌──────────┬──────────┬────────────┐ │
│  │  🖥️ UI   │ ⚙️ Biz   │   💾 Data  │ │
│  │   Layer  │  Logic   │   Layer    │ │
│  └──────────┴──────────┴────────────┘ │
│                  │                     │
│            ┌─────┴─────┐               │
│            │🗄️ Database│               │
│            └───────────┘               │
└────────────────────────────────────────┘
```

| ✅ Pros | ⚠️ Cons |
|------|------|
| Simple to develop and deploy | Difficult to scale individual components |
| Easy debugging and testing | Technology lock-in |
| Low operational complexity | Large codebase becomes unwieldy |
| Good for small teams | Long deployment cycles |

**💡 When to Use**: Small applications, startups, MVPs, small teams

## 🔲 2.2 Microservices Architecture

**📋 Description**: Application as suite of small, independently deployable services.

```
┌─────────┐  ┌─────────┐  ┌─────────┐
│🔲 Svc A │  │🔲 Svc B │  │🔲 Svc C │
│   API   │  │   API   │  │   API   │
│   DB    │  │   DB    │  │   DB    │
└────┬────┘  └────┬────┘  └────┬────┘
     │            │            │
     └────────────┼────────────┘
                  │
            ┌─────┴─────┐
            │🚪 API GW  │
            └───────────┘
```

| ✅ Pros | ⚠️ Cons |
|------|------|
| Independent scaling | Distributed system complexity |
| Technology flexibility | Network latency |
| Fault isolation | Data consistency challenges |
| Team autonomy | Operational overhead |

**💡 When to Use**: Large applications, multiple teams, need for independent scaling

## 🔌 2.3 Service-Oriented Architecture (SOA)

**📋 Description**: Services communicate over a network using an Enterprise Service Bus (ESB).

```
┌─────────┐  ┌─────────┐  ┌─────────┐
│🔌 Svc 1 │  │🔌 Svc 2 │  │🔌 Svc 3 │
└────┬────┘  └────┬────┘  └────┬────┘
     │            │            │
┌────┴────────────┴────────────┴────┐
│       🚌 Enterprise Service Bus    │
└───────────────────────────────────┘
```

**🔑 Key Concepts**:
- 📋 Service contracts
- ♻️ Service reusability
- 🧩 Service composability
- 🚌 Enterprise Service Bus (ESB)

## ☁️ 2.4 Serverless Architecture

**📋 Description**: Application logic runs in stateless compute containers, event-triggered and fully managed.

```
┌──────────┐    ┌──────────┐    ┌──────────┐
│⚡ Event  │───▶│λ Function│───▶│🗄️ DynamoDB│
│  Source  │    │ (Lambda) │    │          │
└──────────┘    └──────────┘    └──────────┘
     │
     ▼
┌──────────┐    ┌──────────┐
│🚪 API    │───▶│λ Function│
│ Gateway  │    │          │
└──────────┘    └──────────┘
```

| ✅ Pros | ⚠️ Cons |
|------|------|
| No server management | Cold start latency |
| Auto-scaling | Vendor lock-in |
| Pay-per-use | Limited execution time |
| Reduced operational cost | Debugging complexity |

**💡 When to Use**: Event-driven workloads, variable traffic, rapid prototyping

## ⚡ 2.5 Event-Driven Architecture (EDA)

**📋 Description**: Communication through events; producers emit events, consumers react to them.

```
┌──────────┐    ┌─────────────┐    ┌──────────┐
│📤 Producer│───▶│📬 Event     │───▶│📥 Consumer│
└──────────┘    │   Broker    │    └──────────┘
                │(Kafka/RMQ)  │
                └──────┬──────┘
                       │
                ┌──────┴──────┐
                │📥 Consumer  │
                └─────────────┘
```

*(🔗 Covered in detail in Lesson 02)*

---

## 📊 Diagrams in This Section

- [2.1-monolithic-architecture.drawio](./2.1-monolithic-architecture.drawio)
- [2.2-microservices-architecture.drawio](./2.2-microservices-architecture.drawio)
- [2.3-soa-esb-architecture.drawio](./2.3-soa-esb-architecture.drawio)
- [2.4-serverless-architecture.drawio](./2.4-serverless-architecture.drawio)
- [2.5-event-driven-architecture.drawio](./2.5-event-driven-architecture.drawio)
- [2.6-architecture-styles-comparison.drawio](./2.6-architecture-styles-comparison.drawio)
