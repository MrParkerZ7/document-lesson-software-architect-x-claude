# Event Patterns

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Message Brokers](../02-message-brokers/README.md) | [Next: Stream Processing](../04-stream-processing/README.md)

---

## 3.1 Event Sourcing

**Description**: Store state as a sequence of events rather than current state.

```
Traditional:              Event Sourcing:
┌─────────────┐          ┌─────────────────────────┐
│ Account     │          │ Event Store             │
│ balance:$100│          │ 1. AccountOpened($0)    │
└─────────────┘          │ 2. Deposited($150)      │
                         │ 3. Withdrawn($50)       │
                         │ Current: $100           │
                         └─────────────────────────┘
```

**Benefits**:
- Complete audit trail
- Temporal queries (state at any point in time)
- Event replay for debugging
- Natural fit for EDA

**Challenges**:
- Event schema evolution
- Eventual consistency
- Storage growth
- Query complexity

**Implementation Pattern**:
```
Command ──▶ Aggregate ──▶ Events ──▶ Event Store
                │                         │
                ▼                         ▼
           Validation              Projections
```

## 3.2 CQRS (Command Query Responsibility Segregation)

**Description**: Separate models for reading and writing data.

```
┌─────────────────────────────────────────────────────┐
│                      CQRS                           │
│                                                     │
│   Commands                      Queries             │
│      │                             │                │
│      ▼                             ▼                │
│  ┌─────────┐                 ┌─────────┐           │
│  │ Write   │                 │  Read   │           │
│  │ Model   │────Events──────▶│  Model  │           │
│  └────┬────┘                 └────┬────┘           │
│       │                           │                │
│       ▼                           ▼                │
│  ┌─────────┐                 ┌─────────┐           │
│  │Write DB │                 │Read DB  │           │
│  │(Events) │                 │(Views)  │           │
│  └─────────┘                 └─────────┘           │
└─────────────────────────────────────────────────────┘
```

**When to Use**:
- Different read/write scaling needs
- Complex domains
- Performance optimization for reads
- Often combined with Event Sourcing

**Read Model Projections**:
| Projection | Purpose |
|------------|---------|
| **Order Summary** | Quick order lookup |
| **Customer Orders** | All orders by customer |
| **Daily Revenue** | Aggregated sales data |

## 3.3 Event Patterns Comparison

These four patterns are often confused but serve **different purposes**:

| Pattern | Category | What It Is | Key Question |
|---------|----------|------------|--------------|
| **Domain Events** | Data Structure | Record of something that happened | "What happened?" |
| **Event Bus** | Infrastructure | Transport mechanism for events | "How do events get delivered?" |
| **Event Sourcing** | Storage Pattern | State as sequence of events | "How do we persist state?" |
| **CQRS** | Architecture | Separate read/write models | "How do we optimize reads vs writes?" |

**Key Distinctions**:

```
┌─────────────────────────────────────────────────────────────────────┐
│                    How They Work Together                           │
│                                                                     │
│  Command → Aggregate → Domain Event → Event Store → Event Bus       │
│                           │              │              │           │
│                           │      Event Sourcing    Transport        │
│                           │              │              │           │
│                     The "WHAT"    The "STORAGE"   The "HOW"         │
│                                                         │           │
│                                              ┌──────────┴───────┐   │
│                                              │                  │   │
│                                         Projection 1     Projection 2│
│                                              │                  │   │
│                                         Read Model 1    Read Model 2 │
│                                              └──────────┬───────┘   │
│                                                    CQRS              │
│                                              The "SEPARATION"        │
└─────────────────────────────────────────────────────────────────────┘
```

**Independence**: These patterns can be used independently:
- Domain Events work without Event Sourcing (in-memory, fire-and-forget)
- Event Sourcing works without CQRS (single model rebuilt from events)
- CQRS works without Event Sourcing (traditional DB sync between models)
- Event Bus works without any of the above (just message transport)

**When to Use Each**:

| Use This Pattern | When You Need |
|-----------------|---------------|
| Domain Events | Loose coupling, audit trail, DDD implementation |
| Event Bus | Distributed systems, async communication, multiple subscribers |
| Event Sourcing | Complete history, time-travel queries, event replay |
| CQRS | High read/write asymmetry, complex queries, independent scaling |

## 3.4 Saga Pattern

**Description**: Manage distributed transactions through a sequence of local transactions.

### Choreography-based Saga

```
┌─────────┐    ┌─────────┐    ┌─────────┐    ┌─────────┐
│ Order   │───▶│ Payment │───▶│Inventory│───▶│Shipping │
│ Service │◀───│ Service │◀───│ Service │◀───│ Service │
└─────────┘    └─────────┘    └─────────┘    └─────────┘
     │              │              │              │
     └──────────────┴──────────────┴──────────────┘
                    Event Bus
```

- Each service listens for events and publishes events
- No central coordinator
- Simpler but harder to track overall flow

### Orchestration-based Saga

```
                    ┌─────────────┐
                    │ Saga        │
                    │ Orchestrator│
                    └──────┬──────┘
           ┌───────────────┼───────────────┐
           ▼               ▼               ▼
      ┌─────────┐    ┌─────────┐    ┌─────────┐
      │ Order   │    │ Payment │    │Inventory│
      │ Service │    │ Service │    │ Service │
      └─────────┘    └─────────┘    └─────────┘
```

- Central orchestrator controls flow
- Easier to understand and monitor
- Single point of coordination

**Compensating Transactions**:
```
Happy Path:
  CreateOrder → ReserveInventory → ProcessPayment → ShipOrder

Compensation (if Payment fails):
  ProcessPayment fails → ReleaseInventory → CancelOrder
```

## 3.5 Choreography vs Orchestration

| Aspect | Choreography | Orchestration |
|--------|--------------|---------------|
| **Coupling** | Loose | Tighter to orchestrator |
| **Complexity** | Distributed | Centralized |
| **Visibility** | Harder to track | Easier to monitor |
| **Single Point of Failure** | No | Yes (orchestrator) |
| **Adding Steps** | Modify multiple services | Modify orchestrator |
| **Best For** | Simple workflows | Complex business processes |

---

## Diagrams in This Section

- [3.1-event-sourcing.drawio](./3.1-event-sourcing.drawio)
- [3.2-cqrs-pattern.drawio](./3.2-cqrs-pattern.drawio)
- [3.3-saga-choreography.drawio](./3.3-saga-choreography.drawio)
- [3.4-saga-orchestration.drawio](./3.4-saga-orchestration.drawio)
- [3.5-compensating-transactions.drawio](./3.5-compensating-transactions.drawio)
- [3.6-event-patterns-comparison.drawio](./3.6-event-patterns-comparison.drawio)
