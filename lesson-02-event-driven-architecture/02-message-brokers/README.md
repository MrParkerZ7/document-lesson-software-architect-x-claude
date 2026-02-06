# Message Brokers

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Core Concepts](../01-core-concepts/README.md) | [Next: Event Patterns](../03-event-patterns/README.md)

---

## 2.1 Apache Kafka

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

## 2.2 RabbitMQ

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

## 2.3 Azure Service Bus

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

## 2.4 AWS SQS/SNS

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

## Broker Comparison

| Feature | Kafka | RabbitMQ | Azure SB | AWS SQS/SNS |
|---------|-------|----------|----------|-------------|
| **Throughput** | Very High | Medium | Medium | Medium |
| **Ordering** | Per partition | Per queue | Per session | FIFO option |
| **Retention** | Configurable | Until consumed | 14 days max | 14 days max |
| **Replay** | Yes | No | No | No |
| **Protocol** | Custom | AMQP | AMQP | HTTP |
| **Best For** | Streaming | Task queues | Enterprise | Serverless |

---

## Diagrams in This Section

- [2.1-kafka-architecture.drawio](./2.1-kafka-architecture.drawio)
- [2.2-rabbitmq-architecture.drawio](./2.2-rabbitmq-architecture.drawio)
- [2.3-azure-service-bus.drawio](./2.3-azure-service-bus.drawio)
- [2.4-aws-sqs-sns.drawio](./2.4-aws-sqs-sns.drawio)
- [2.5-message-brokers-comparison.drawio](./2.5-message-brokers-comparison.drawio)
