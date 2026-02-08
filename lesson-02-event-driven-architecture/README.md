# ⚡ Lesson 02: Event-Driven Architecture

## 📋 Overview

Event-Driven Architecture (EDA) is a software design paradigm where the flow of the program is determined by events—significant changes in state or occurrences. This architecture style enables loosely coupled, scalable, and responsive systems.

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- 📨 Understand core concepts of event-driven systems
- 📬 Choose appropriate message brokers for different scenarios
- 🔄 Implement common event patterns (Event Sourcing, CQRS, Saga)
- 🌊 Design streaming and async communication solutions

---

## 📚 Sub-lessons

| # | Topic | Diagrams |
|---|-------|----------|
| 01 | [📨 Core Concepts](./01-core-concepts/README.md) | 2 |
| 02 | [📬 Message Brokers](./02-message-brokers/README.md) | 5 |
| 03 | [🔄 Event Patterns](./03-event-patterns/README.md) | 6 |
| 04 | [🌊 Stream Processing](./04-stream-processing/README.md) | 3 |
| 05 | [🔀 Async Communication Patterns](./05-async-communication-patterns/README.md) | 4 |
| 06 | [✅ Best Practices](./06-best-practices/README.md) | 2 |

---

## 💪 Practical Exercises

1. **📊 Kafka Setup**: Deploy a local Kafka cluster and produce/consume messages
2. **📜 Event Sourcing**: Implement a simple bank account with event sourcing
3. **🎭 Saga Implementation**: Design a saga for an e-commerce checkout flow
4. **📈 Stream Processing**: Build a real-time dashboard using Kafka Streams

---

## 📖 Further Reading

- 📕 "Designing Data-Intensive Applications" - Martin Kleppmann
- 📗 "Enterprise Integration Patterns" - Gregor Hohpe
- 📘 "Building Event-Driven Microservices" - Adam Bellemare
- 📙 Apache Kafka Documentation
- 📓 Microsoft Azure Event-Driven Architecture Guide

---

## 🎓 Summary

Event-Driven Architecture enables building scalable, loosely coupled systems that can react to changes in real-time. Understanding message brokers, event patterns like Event Sourcing and CQRS, and stream processing technologies is essential for modern distributed systems.

> 💡 **Key Insight**: The key is choosing the right patterns and tools based on your specific requirements for consistency, latency, and complexity.

---

## 📁 Files in This Lesson

### 📚 Sub-Lessons
| # | Topic | README |
|---|-------|--------|
| 01 | Core Concepts | [README](./01-core-concepts/README.md) |
| 02 | Message Brokers | [README](./02-message-brokers/README.md) |
| 03 | Event Patterns | [README](./03-event-patterns/README.md) |
| 04 | Stream Processing | [README](./04-stream-processing/README.md) |
| 05 | Async Communication Patterns | [README](./05-async-communication-patterns/README.md) |
| 06 | Best Practices | [README](./06-best-practices/README.md) |

### 📊 Diagrams
| File | Preview |
|------|---------|
| [1.1-eda-overview.drawio](./01-core-concepts/1.1-eda-overview.drawio) | [PNG](./01-core-concepts/1.1-eda-overview.png) |
| [1.2-event-types-structure.drawio](./01-core-concepts/1.2-event-types-structure.drawio) | [PNG](./01-core-concepts/1.2-event-types-structure.png) |
| [2.1-kafka-architecture.drawio](./02-message-brokers/2.1-kafka-architecture.drawio) | [PNG](./02-message-brokers/2.1-kafka-architecture.png) |
| [2.2-rabbitmq-architecture.drawio](./02-message-brokers/2.2-rabbitmq-architecture.drawio) | [PNG](./02-message-brokers/2.2-rabbitmq-architecture.png) |
| [2.3-azure-service-bus.drawio](./02-message-brokers/2.3-azure-service-bus.drawio) | [PNG](./02-message-brokers/2.3-azure-service-bus.png) |
| [2.4-aws-sqs-sns.drawio](./02-message-brokers/2.4-aws-sqs-sns.drawio) | [PNG](./02-message-brokers/2.4-aws-sqs-sns.png) |
| [2.5-message-brokers-comparison.drawio](./02-message-brokers/2.5-message-brokers-comparison.drawio) | [PNG](./02-message-brokers/2.5-message-brokers-comparison.png) |
| [3.1-event-sourcing.drawio](./03-event-patterns/3.1-event-sourcing.drawio) | [PNG](./03-event-patterns/3.1-event-sourcing.png) |
| [3.2-cqrs-pattern.drawio](./03-event-patterns/3.2-cqrs-pattern.drawio) | [PNG](./03-event-patterns/3.2-cqrs-pattern.png) |
| [3.3-saga-choreography.drawio](./03-event-patterns/3.3-saga-choreography.drawio) | [PNG](./03-event-patterns/3.3-saga-choreography.png) |
| [3.4-saga-orchestration.drawio](./03-event-patterns/3.4-saga-orchestration.drawio) | [PNG](./03-event-patterns/3.4-saga-orchestration.png) |
| [3.5-compensating-transactions.drawio](./03-event-patterns/3.5-compensating-transactions.drawio) | [PNG](./03-event-patterns/3.5-compensating-transactions.png) |
| [3.6-event-patterns-comparison.drawio](./03-event-patterns/3.6-event-patterns-comparison.drawio) | [PNG](./03-event-patterns/3.6-event-patterns-comparison.png) |
| [4.1-stream-processing-overview.drawio](./04-stream-processing/4.1-stream-processing-overview.drawio) | [PNG](./04-stream-processing/4.1-stream-processing-overview.png) |
| [4.2-flink-kafka-streams.drawio](./04-stream-processing/4.2-flink-kafka-streams.drawio) | [PNG](./04-stream-processing/4.2-flink-kafka-streams.png) |
| [4.3-stream-processing-comparison.drawio](./04-stream-processing/4.3-stream-processing-comparison.drawio) | [PNG](./04-stream-processing/4.3-stream-processing-comparison.png) |
| [5.1-pubsub-pattern.drawio](./05-async-communication-patterns/5.1-pubsub-pattern.drawio) | [PNG](./05-async-communication-patterns/5.1-pubsub-pattern.png) |
| [5.2-message-queue-pattern.drawio](./05-async-communication-patterns/5.2-message-queue-pattern.drawio) | [PNG](./05-async-communication-patterns/5.2-message-queue-pattern.png) |
| [5.3-event-streams-pattern.drawio](./05-async-communication-patterns/5.3-event-streams-pattern.drawio) | [PNG](./05-async-communication-patterns/5.3-event-streams-pattern.png) |
| [5.4-request-reply-pattern.drawio](./05-async-communication-patterns/5.4-request-reply-pattern.drawio) | [PNG](./05-async-communication-patterns/5.4-request-reply-pattern.png) |
| [6.1-error-handling-dlq.drawio](./06-best-practices/6.1-error-handling-dlq.drawio) | [PNG](./06-best-practices/6.1-error-handling-dlq.png) |
| [6.2-eda-best-practices.drawio](./06-best-practices/6.2-eda-best-practices.drawio) | [PNG](./06-best-practices/6.2-eda-best-practices.png) |
