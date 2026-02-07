# Database Selection Guide

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Storage Solutions](../06-storage-solutions/README.md)

---

## Database Selection Flowchart

```
+-------------------------------------------------------------+
|              Database Selection Flowchart                    |
|                                                             |
|                    Start                                    |
|                      |                                      |
|              Need ACID transactions?                        |
|                 /          \                                |
|               Yes           No                              |
|                |             |                              |
|          Relational     Data structure?                     |
|          (PostgreSQL,      /    |    \                      |
|           MySQL)       Key-   Doc   Graph                  |
|                       Value                                 |
|                         |      |      |                     |
|                      Redis  MongoDB  Neo4j                 |
|                                                             |
|  Consider also:                                            |
|  - Scale requirements -> Cassandra, DynamoDB               |
|  - Time-series -> InfluxDB, TimescaleDB                   |
|  - Search -> Elasticsearch                                  |
|  - Analytics -> ClickHouse, BigQuery                        |
+-------------------------------------------------------------+
```

## Quick Reference Guide

| Use Case | Recommended Database | Why |
|----------|---------------------|-----|
| Financial transactions | PostgreSQL, SQL Server | ACID compliance, strong consistency |
| E-commerce catalog | MongoDB, PostgreSQL | Flexible schema, rich queries |
| Session storage | Redis | Fast, in-memory, TTL support |
| Social network | Neo4j, Neptune | Relationship traversal |
| IoT/Metrics | InfluxDB, TimescaleDB | Time-series optimization |
| Full-text search | Elasticsearch | Inverted index, relevance scoring |
| Global scale | Cassandra, DynamoDB | Geographic distribution, high availability |
| Analytics | ClickHouse, BigQuery | Columnar storage, fast aggregations |

## Decision Factors

### Performance Requirements
- **Read-heavy**: Consider read replicas, caching, materialized views
- **Write-heavy**: Consider Cassandra, DynamoDB, or sharding
- **Low latency**: Consider Redis, in-memory databases

### Scalability Needs
- **Vertical**: Traditional RDBMS can scale up
- **Horizontal**: NoSQL databases designed for scale-out
- **Global**: Multi-region replication support

### Data Model
- **Structured**: Relational databases
- **Semi-structured**: Document databases
- **Relationships**: Graph databases
- **Time-stamped**: Time-series databases

### Consistency Requirements
- **Strong consistency**: Relational, MongoDB (with appropriate settings)
- **Eventual consistency**: Cassandra, DynamoDB

### Operational Considerations
- **Managed vs Self-hosted**: Cloud managed services reduce operational burden
- **Team expertise**: Consider existing skills
- **Ecosystem**: Integration with existing tools and frameworks

## Multi-Database Architecture

Modern applications often use multiple databases (polyglot persistence):

```
+-------------------+     +-------------------+
|   PostgreSQL      |     |      Redis        |
|   (Transactions)  |     |    (Caching)      |
+-------------------+     +-------------------+
         |                         |
         v                         v
+---------------------------------------------+
|            Application Layer                 |
+---------------------------------------------+
         |                         |
         v                         v
+-------------------+     +-------------------+
|   Elasticsearch   |     |     MongoDB       |
|    (Search)       |     |   (Content)       |
+-------------------+     +-------------------+
```

---

## Diagrams in This Section

- [7.1-database-selection.drawio](./7.1-database-selection.drawio)
