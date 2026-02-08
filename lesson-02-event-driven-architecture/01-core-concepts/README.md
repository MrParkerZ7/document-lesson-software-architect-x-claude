# ⚡ Core Concepts

> **Navigation**: [⬅️ Back to Lesson Overview](../README.md) | [Next: Message Brokers ➡️](../02-message-brokers/README.md)

---

## 📋 1.1 What is an Event?

An **event** is a record of something that happened in the system. Events are:
- **📌 Immutable**: Once created, cannot be changed
- **⏮️ Past tense**: Represent something that already occurred
- **📦 Self-contained**: Include all necessary information

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

## 🏷️ 1.2 Event Types

| Type | Description | Example |
|------|-------------|---------|
| **🎯 Domain Event** | Business-significant occurrence | `OrderPlaced`, `PaymentReceived` |
| **🔗 Integration Event** | Cross-boundary communication | `CustomerCreated` (shared) |
| **⚙️ System Event** | Technical/infrastructure event | `ServiceStarted`, `HealthCheckPassed` |

## 🧩 1.3 Event-Driven Components

```
┌──────────────┐         ┌──────────────┐         ┌──────────────┐
│   Producer   │────────▶│    Broker    │────────▶│   Consumer   │
│  (Publisher) │         │   (Channel)  │         │ (Subscriber) │
└──────────────┘         └──────────────┘         └──────────────┘
```

- **📤 Producer/Publisher**: Emits events when something happens
- **📬 Broker/Channel**: Routes events to interested consumers
- **📥 Consumer/Subscriber**: Reacts to events

---

## 📊 Diagrams in This Section

- [1.1-eda-overview.drawio](./1.1-eda-overview.drawio)
- [1.2-event-types-structure.drawio](./1.2-event-types-structure.drawio)
