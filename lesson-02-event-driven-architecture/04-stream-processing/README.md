# Stream Processing

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Event Patterns](../03-event-patterns/README.md) | [Next: Async Communication Patterns](../05-async-communication-patterns/README.md)

---

## 4.1 Apache Flink

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

## 4.2 Kafka Streams

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

## 4.3 Azure Stream Analytics

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

## Stream Processing Comparison

| Feature | Flink | Kafka Streams | Azure SA |
|---------|-------|---------------|----------|
| **Deployment** | Cluster | Embedded | Managed |
| **State** | Distributed | Local | Managed |
| **Exactly-once** | Yes | Yes | Yes |
| **Latency** | Very Low | Low | Low |
| **Learning Curve** | Steep | Medium | Easy |

---

## Diagrams in This Section

- [4.1-stream-processing-overview.drawio](./4.1-stream-processing-overview.drawio)
- [4.2-flink-kafka-streams.drawio](./4.2-flink-kafka-streams.drawio)
- [4.3-stream-processing-comparison.drawio](./4.3-stream-processing-comparison.drawio)
