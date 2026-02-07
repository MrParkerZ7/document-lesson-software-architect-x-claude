# CAP Theorem & Consistency

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: NoSQL Databases](../03-nosql-databases/README.md) | [Next: Caching](../05-caching/README.md)

---

## CAP Theorem

```
+-------------------------------------------------------------+
|                    CAP Theorem                               |
|                                                             |
|              Consistency (C)                                |
|                    /\                                       |
|                   /  \                                      |
|                  /    \                                     |
|                 / CA   \                                    |
|                /        \                                   |
|               /          \                                  |
|              /     CP     \                                 |
|             /______________\                                |
|  Availability (A)          Partition                       |
|                            Tolerance (P)                   |
|                                                             |
|  Pick 2 (in presence of network partition):                |
|  - CP: Consistent + Partition Tolerant                     |
|    (MongoDB, HBase, Redis Cluster)                         |
|  - AP: Available + Partition Tolerant                      |
|    (Cassandra, DynamoDB, CouchDB)                          |
|  - CA: Consistent + Available                              |
|    (Traditional RDBMS - single node)                       |
+-------------------------------------------------------------+
```

## Consistency Models

| Model | Description | Example |
|-------|-------------|---------|
| **Strong** | All reads see latest write | PostgreSQL, Spanner |
| **Eventual** | Reads eventually see latest write | Cassandra, DynamoDB |
| **Causal** | Causally related ops seen in order | MongoDB sessions |
| **Read-your-writes** | Client sees own writes | Session consistency |
| **Monotonic reads** | Won't see older data after newer | Client sessions |

## ACID vs BASE

```
+-------------------------------------------------------------+
|              ACID vs BASE                                    |
|                                                             |
|  ACID (Relational)         BASE (NoSQL)                    |
|  -----------------         -----------                      |
|  Atomicity                 Basically Available              |
|  Consistency               Soft state                       |
|  Isolation                 Eventually consistent            |
|  Durability                                                 |
|                                                             |
|  Use ACID when:            Use BASE when:                  |
|  - Data integrity critical - High availability needed      |
|  - Complex transactions    - Global distribution           |
|  - Financial systems       - Scale is priority             |
|  - Regulatory compliance   - Eventual consistency OK       |
+-------------------------------------------------------------+
```

## Key Concepts

### Consistency (C)
Every read receives the most recent write or an error. All nodes see the same data at the same time.

### Availability (A)
Every request receives a response (success or failure). The system remains operational even if some nodes fail.

### Partition Tolerance (P)
The system continues to operate despite network partitions (communication breakdowns between nodes).

## Practical Implications

In distributed systems, network partitions are inevitable. Therefore, you must choose between:

- **CP Systems**: Sacrifice availability for consistency during partitions
  - Use case: Financial transactions, inventory management

- **AP Systems**: Sacrifice consistency for availability during partitions
  - Use case: Social media feeds, shopping carts, DNS

---

## Diagrams in This Section

- [4.1-cap-theorem.drawio](./4.1-cap-theorem.drawio)
