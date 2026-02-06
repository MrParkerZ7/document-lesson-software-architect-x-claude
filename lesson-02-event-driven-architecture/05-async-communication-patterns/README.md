# Async Communication Patterns

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Stream Processing](../04-stream-processing/README.md) | [Next: Best Practices](../06-best-practices/README.md)

---

## 5.1 Pub/Sub (Publish/Subscribe)

```
Publisher ──▶ Topic ──▶ Subscriber A
                  └───▶ Subscriber B
                  └───▶ Subscriber C
```

**Characteristics**:
- One-to-many communication
- Publishers don't know subscribers
- Subscribers receive all messages

## 5.2 Message Queues

```
Producer ──▶ Queue ──▶ Consumer A
                  ◀── Consumer B (competing)
```

**Characteristics**:
- Point-to-point communication
- Message consumed once
- Load balancing across consumers

## 5.3 Event Streams

```
Producer ──▶ Stream ──▶ Consumer A (offset: 100)
                   └──▶ Consumer B (offset: 50)
                   └──▶ Consumer C (offset: 100)
```

**Characteristics**:
- Ordered, immutable log
- Multiple consumers at different positions
- Replay capability

## 5.4 Request/Reply over Messaging

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

## Diagrams in This Section

- [5.1-pubsub-pattern.drawio](./5.1-pubsub-pattern.drawio)
- [5.2-message-queue-pattern.drawio](./5.2-message-queue-pattern.drawio)
- [5.3-event-streams-pattern.drawio](./5.3-event-streams-pattern.drawio)
- [5.4-request-reply-pattern.drawio](./5.4-request-reply-pattern.drawio)
