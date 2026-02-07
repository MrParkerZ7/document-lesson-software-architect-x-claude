# Distributed Tracing

[Previous: Metrics](../03-metrics/README.md) | [Next: Alerting](../05-alerting/README.md)

---

## Overview

Distributed tracing tracks requests as they flow through multiple services, providing visibility into the end-to-end journey of a transaction. It is essential for debugging latency issues and understanding service dependencies in microservices architectures.

## Learning Objectives

- Understand distributed tracing concepts and terminology
- Learn trace context propagation mechanisms
- Implement tracing in distributed systems
- Analyze traces to identify performance bottlenecks

## Key Concepts

### Tracing Terminology
- **Trace**: End-to-end journey of a request
- **Span**: Single unit of work within a trace
- **Context**: Metadata propagated between services
- **Baggage**: User-defined key-value pairs

### Context Propagation
- HTTP headers (W3C Trace Context, B3)
- Message queue headers
- gRPC metadata

### Sampling Strategies
- Head-based sampling
- Tail-based sampling
- Adaptive sampling

## Diagrams

| Diagram | Description |
|---------|-------------|
| [4.1-distributed-tracing.drawio](./4.1-distributed-tracing.drawio) | Distributed tracing architecture and flow |

## Summary

Distributed tracing is critical for understanding request flows in microservices architectures. It enables identification of latency bottlenecks and helps visualize service dependencies.

---

[Previous: Metrics](../03-metrics/README.md) | [Next: Alerting](../05-alerting/README.md)
