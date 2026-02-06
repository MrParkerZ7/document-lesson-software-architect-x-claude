# Best Practices

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Async Communication Patterns](../05-async-communication-patterns/README.md)

---

## 6.1 Event Design

- Use past tense for event names (`OrderCreated`, not `CreateOrder`)
- Include all necessary data (fat events vs. thin events)
- Version your events for schema evolution
- Use correlation IDs for tracing

## 6.2 Idempotency

Ensure consumers can handle duplicate events:
```
// Idempotent handler
if (processedEvents.contains(event.id)) {
    return; // Already processed
}
processEvent(event);
processedEvents.add(event.id);
```

## 6.3 Error Handling

```
┌─────────┐    ┌─────────┐    ┌─────────────┐
│  Queue  │───▶│Consumer │───▶│ Dead Letter │
└─────────┘    └─────────┘    │    Queue    │
                    │         └─────────────┘
                    │                │
                    └─── Retry ──────┘
```

## 6.4 Monitoring

Key metrics to track:
- Event lag (consumer behind producer)
- Processing time
- Error rates
- Queue depth

---

## Diagrams in This Section

- [6.1-error-handling-dlq.drawio](./6.1-error-handling-dlq.drawio)
- [6.2-eda-best-practices.drawio](./6.2-eda-best-practices.drawio)
