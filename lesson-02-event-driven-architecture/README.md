# Lesson 02: Event-Driven Architecture 📨

## 📋 Overview

Event-Driven Architecture (EDA) is a software design paradigm where the flow of the program is determined by events—significant changes in state or occurrences. This architecture style enables loosely coupled, scalable, and responsive systems.

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Understand core concepts of event-driven systems
- Choose appropriate message brokers for different scenarios
- Implement common event patterns (Event Sourcing, CQRS, Saga)
- Design streaming and async communication solutions

---

## 1. 💡 Core Concepts

### 1.1 What is an Event?

An **event** is a record of something that happened in the system. Events are:
- **Immutable**: Once created, cannot be changed
- **Past tense**: Represent something that already occurred
- **Self-contained**: Include all necessary information

```json
{
  "eventId": "evt_123456",
  "eventType": "OrderPlaced",
  "timestamp": "2026-01-31T10:30:00Z",
  "data": {
    "orderId": "ord_789",
    "customerId": "cust_456",
    "totalAmount": 150.00,
    "items": [...]
  },
  "metadata": {
    "correlationId": "corr_abc",
    "causationId": "evt_123455"
  }
}
```

### 1.2 Event Types

| Type | Description | Example |
|------|-------------|---------|
| **Domain Event** | Business-significant occurrence | `OrderPlaced`, `PaymentReceived` |
| **Integration Event** | Cross-boundary communication | `CustomerCreated` (shared) |
| **System Event** | Technical/infrastructure event | `ServiceStarted`, `HealthCheckPassed` |

### 1.3 Event-Driven Components

```
┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│   Producer   │────────▶│    Broker    │────────▶│   Consumer   │
│  (Publisher) │         │   (Channel)  │         │ (Subscriber) │
└──────────────┘         └──────────────┘         └──────────────┘
```

- **Producer/Publisher**: Emits events when something happens
- **Broker/Channel**: Routes events to interested consumers
- **Consumer/Subscriber**: Reacts to events

---

## 2. 📬 Message Brokers

### 2.1 Apache Kafka

**Description**: Distributed event streaming platform for high-throughput, fault-tolerant messaging.

```
┌─────────────────────────────────────────────────────┐
│                    Kafka Cluster                    │
│  ┌─────────┐  ┌─────────┐  ┌─────────┐            │
│  │Broker 1 │  │Broker 2 │  │Broker 3 │            │
│  └────┬────┘  └────┬────┘  └────┬────┘            │
│       │            │            │                  │
│  ┌────┴────────────┴────────────┴────┐            │
│  │              Topic                 │            │
│  │  ┌─────┐  ┌─────┐  ┌─────┐       │            │
│  │  │ P0  │  │ P1  │  │ P2  │       │            │
│  │  └─────┘  └─────┘  └─────┘       │            │
│  └───────────────────────────────────┘            │
└─────────────────────────────────────────────────────┘
```

**Key Concepts**:
| Concept | Description |
|---------|-------------|
| **Topic** | Named feed of messages |
| **Partition** | Ordered, immutable sequence within topic |
| **Consumer Group** | Group of consumers sharing workload |
| **Offset** | Position of message in partition |
| **Replication** | Data redundancy across brokers |

**Use Cases**:
- Log aggregation
- Stream processing
- Event sourcing
- Metrics collection
- Real-time analytics

**Sample Configuration**:
```properties
# Producer
bootstrap.servers=kafka1:9092,kafka2:9092
acks=all
retries=3
batch.size=16384

# Consumer
group.id=order-service
auto.offset.reset=earliest
enable.auto.commit=false
```

### 2.2 RabbitMQ

**Description**: Traditional message broker implementing AMQP protocol.

```
┌─────────────────────────────────────────────────────┐
│                    RabbitMQ                         │
│                                                     │
│  Producer ──▶ Exchange ──▶ Queue ──▶ Consumer      │
│                  │                                  │
│            ┌─────┴─────┐                           │
│            │  Binding  │                           │
│            └───────────┘                           │
└─────────────────────────────────────────────────────┘
```

**Exchange Types**:
| Type | Routing Logic |
|------|---------------|
| **Direct** | Exact routing key match |
| **Fanout** | Broadcast to all bound queues |
| **Topic** | Pattern matching on routing key |
| **Headers** | Match on message headers |

**Use Cases**:
- Task queues
- Request/reply patterns
- Pub/sub messaging
- Complex routing scenarios

### 2.3 Azure Service Bus

**Description**: Enterprise message broker with queues and pub/sub topics.

**Features**:
- Message sessions
- Dead-letter queue
- Scheduled delivery
- Transactions
- Duplicate detection

```
┌────────────────────────────────────────────────────┐
│              Azure Service Bus                      │
│                                                     │
│  ┌─────────┐    ┌─────────────────────────────┐   │
│  │  Queue  │    │          Topic              │   │
│  │         │    │  ┌─────┐  ┌─────┐  ┌─────┐ │   │
│  │         │    │  │Sub 1│  │Sub 2│  │Sub 3│ │   │
│  └─────────┘    │  └─────┘  └─────┘  └─────┘ │   │
│                 └─────────────────────────────┘   │
└────────────────────────────────────────────────────┘
```

### 2.4 AWS SQS/SNS

**SQS (Simple Queue Service)**:
- Standard queues: Best-effort ordering, at-least-once delivery
- FIFO queues: Exactly-once processing, strict ordering

**SNS (Simple Notification Service)**:
- Pub/sub messaging
- Push-based delivery
- Fan-out to multiple endpoints

```
┌──────────┐    ┌──────────┐    ┌──────────┐
│ Producer │───▶│   SNS    │───▶│   SQS    │───▶ Consumer
└──────────┘    │  Topic   │    │  Queue   │
                └────┬─────┘    └──────────┘
                     │
                     ├─────────▶ Lambda
                     │
                     └─────────▶ HTTP Endpoint
```

### Broker Comparison

| Feature | Kafka | RabbitMQ | Azure SB | AWS SQS/SNS |
|---------|-------|----------|----------|-------------|
| **Throughput** | Very High | Medium | Medium | Medium |
| **Ordering** | Per partition | Per queue | Per session | FIFO option |
| **Retention** | Configurable | Until consumed | 14 days max | 14 days max |
| **Replay** | Yes | No | No | No |
| **Protocol** | Custom | AMQP | AMQP | HTTP |
| **Best For** | Streaming | Task queues | Enterprise | Serverless |

---

## 3. 🔄 Event Patterns

### 3.1 Event Sourcing

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

### 3.2 CQRS (Command Query Responsibility Segregation)

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

### 3.3 Event Patterns Comparison

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

> **📊 Diagram**: See `diagrams/3.6-event-patterns-comparison.drawio` for a visual comparison

### 3.4 Saga Pattern

**Description**: Manage distributed transactions through a sequence of local transactions.

#### Choreography-based Saga

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

#### Orchestration-based Saga

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

### 3.5 Choreography vs Orchestration

| Aspect | Choreography | Orchestration |
|--------|--------------|---------------|
| **Coupling** | Loose | Tighter to orchestrator |
| **Complexity** | Distributed | Centralized |
| **Visibility** | Harder to track | Easier to monitor |
| **Single Point of Failure** | No | Yes (orchestrator) |
| **Adding Steps** | Modify multiple services | Modify orchestrator |
| **Best For** | Simple workflows | Complex business processes |

---

## 4. 🌊 Stream Processing

### 4.1 Apache Flink

**Description**: Distributed stream processing framework for stateful computations.

**Key Features**:
- Exactly-once semantics
- Event time processing
- Stateful computations
- Low latency

```java
// Flink Example
DataStream<Order> orders = env.addSource(kafkaConsumer);

orders
    .keyBy(order -> order.getCustomerId())
    .window(TumblingEventTimeWindows.of(Time.hours(1)))
    .aggregate(new OrderAggregator())
    .addSink(resultSink);
```

### 4.2 Kafka Streams

**Description**: Client library for building stream processing applications on Kafka.

```java
// Kafka Streams Example
StreamsBuilder builder = new StreamsBuilder();

KStream<String, Order> orders = builder.stream("orders");

orders
    .filter((key, order) -> order.getAmount() > 100)
    .groupByKey()
    .count()
    .toStream()
    .to("high-value-orders");
```

### 4.3 Azure Stream Analytics

**Description**: Real-time analytics service on Azure.

```sql
-- Stream Analytics Query
SELECT
    CustomerId,
    COUNT(*) as OrderCount,
    SUM(Amount) as TotalAmount
FROM OrdersInput TIMESTAMP BY EventTime
GROUP BY
    CustomerId,
    TumblingWindow(minute, 5)
HAVING COUNT(*) > 10
```

### Stream Processing Comparison

| Feature | Flink | Kafka Streams | Azure SA |
|---------|-------|---------------|----------|
| **Deployment** | Cluster | Embedded | Managed |
| **State** | Distributed | Local | Managed |
| **Exactly-once** | Yes | Yes | Yes |
| **Latency** | Very Low | Low | Low |
| **Learning Curve** | Steep | Medium | Easy |

---

## 5. ⚡ Async Communication Patterns

### 5.1 Pub/Sub (Publish/Subscribe)

```
Publisher ──▶ Topic ──▶ Subscriber A
                  └───▶ Subscriber B
                  └───▶ Subscriber C
```

**Characteristics**:
- One-to-many communication
- Publishers don't know subscribers
- Subscribers receive all messages

### 5.2 Message Queues

```
Producer ──▶ Queue ──▶ Consumer A
                  ◀── Consumer B (competing)
```

**Characteristics**:
- Point-to-point communication
- Message consumed once
- Load balancing across consumers

### 5.3 Event Streams

```
Producer ──▶ Stream ──▶ Consumer A (offset: 100)
                   └──▶ Consumer B (offset: 50)
                   └──▶ Consumer C (offset: 100)
```

**Characteristics**:
- Ordered, immutable log
- Multiple consumers at different positions
- Replay capability

### 5.4 Request/Reply over Messaging

```
┌──────────┐    Request    ┌──────────┐
│  Client  │──────────────▶│  Server  │
│          │◀──────────────│          │
└──────────┘    Reply      └──────────┘
      │                          │
      └──────────────────────────┘
           Correlation ID
```

---

## 6. ✅ Best Practices

### 6.1 Event Design

- Use past tense for event names (`OrderCreated`, not `CreateOrder`)
- Include all necessary data (fat events vs. thin events)
- Version your events for schema evolution
- Use correlation IDs for tracing

### 6.2 Idempotency

Ensure consumers can handle duplicate events:
```
// Idempotent handler
if (processedEvents.contains(event.id)) {
    return; // Already processed
}
processEvent(event);
processedEvents.add(event.id);
```

### 6.3 Error Handling

```
┌─────────┐    ┌─────────┐    ┌─────────────┐
│  Queue  │───▶│Consumer │───▶│ Dead Letter │
└─────────┘    └─────────┘    │    Queue    │
                    │         └─────────────┘
                    │                │
                    └─── Retry ──────┘
```

### 6.4 Monitoring

Key metrics to track:
- Event lag (consumer behind producer)
- Processing time
- Error rates
- Queue depth

---

## 🏋️ Practical Exercises

1. **Kafka Setup**: Deploy a local Kafka cluster and produce/consume messages
2. **Event Sourcing**: Implement a simple bank account with event sourcing
3. **Saga Implementation**: Design a saga for an e-commerce checkout flow
4. **Stream Processing**: Build a real-time dashboard using Kafka Streams

---

## 📖 Further Reading

- "Designing Data-Intensive Applications" - Martin Kleppmann
- "Enterprise Integration Patterns" - Gregor Hohpe
- "Building Event-Driven Microservices" - Adam Bellemare
- Apache Kafka Documentation
- Microsoft Azure Event-Driven Architecture Guide

---

## 📝 Summary

Event-Driven Architecture enables building scalable, loosely coupled systems that can react to changes in real-time. Understanding message brokers, event patterns like Event Sourcing and CQRS, and stream processing technologies is essential for modern distributed systems. The key is choosing the right patterns and tools based on your specific requirements for consistency, latency, and complexity.
