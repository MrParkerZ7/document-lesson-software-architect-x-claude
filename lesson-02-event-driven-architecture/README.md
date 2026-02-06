# Lesson 02: Event-Driven Architecture

## Overview

Event-Driven Architecture (EDA) is a software design paradigm where the flow of the program is determined by events—significant changes in state or occurrences. This architecture style enables loosely coupled, scalable, and responsive systems.

---

## Learning Objectives

By the end of this lesson, you will be able to:

- Understand core concepts of event-driven systems
- Choose appropriate message brokers for different scenarios
- Implement common event patterns (Event Sourcing, CQRS, Saga)
- Design streaming and async communication solutions

---

## Sub-lessons

| # | Topic | Diagrams |
|---|-------|----------|
| 01 | [Core Concepts](./01-core-concepts/README.md) | 2 |
| 02 | [Message Brokers](./02-message-brokers/README.md) | 5 |
| 03 | [Event Patterns](./03-event-patterns/README.md) | 6 |
| 04 | [Stream Processing](./04-stream-processing/README.md) | 3 |
| 05 | [Async Communication Patterns](./05-async-communication-patterns/README.md) | 4 |
| 06 | [Best Practices](./06-best-practices/README.md) | 2 |

---

## Practical Exercises

1. **Kafka Setup**: Deploy a local Kafka cluster and produce/consume messages
2. **Event Sourcing**: Implement a simple bank account with event sourcing
3. **Saga Implementation**: Design a saga for an e-commerce checkout flow
4. **Stream Processing**: Build a real-time dashboard using Kafka Streams

---

## Further Reading

- "Designing Data-Intensive Applications" - Martin Kleppmann
- "Enterprise Integration Patterns" - Gregor Hohpe
- "Building Event-Driven Microservices" - Adam Bellemare
- Apache Kafka Documentation
- Microsoft Azure Event-Driven Architecture Guide

---

## Summary

Event-Driven Architecture enables building scalable, loosely coupled systems that can react to changes in real-time. Understanding message brokers, event patterns like Event Sourcing and CQRS, and stream processing technologies is essential for modern distributed systems. The key is choosing the right patterns and tools based on your specific requirements for consistency, latency, and complexity.
