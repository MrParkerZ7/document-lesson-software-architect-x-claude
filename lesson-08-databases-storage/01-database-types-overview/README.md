# Database Types Overview

> **Navigation**: [Back to Lesson Overview](../README.md) | [Next: Relational Databases](../02-relational-databases/README.md)

---

```
+-------------------------------------------------------------+
|                   Database Landscape                         |
|                                                             |
|  +------------------------------------------------------+  |
|  |                  Relational (SQL)                     |  |
|  |  PostgreSQL, MySQL, SQL Server, Oracle               |  |
|  +------------------------------------------------------+  |
|                                                             |
|  +------------+ +------------+ +------------+              |
|  |  Document  | | Key-Value  | |   Graph    |              |
|  |  MongoDB   | |   Redis    | |   Neo4j    |              |
|  |  Cosmos DB | |  DynamoDB  | |  Neptune   |              |
|  +------------+ +------------+ +------------+              |
|                                                             |
|  +------------+ +------------+ +------------+              |
|  |   Column   | |Time-Series | |   Search   |              |
|  | Cassandra  | | InfluxDB   | |Elasticsearch|             |
|  |   HBase    | |TimescaleDB | |   Solr     |              |
|  +------------+ +------------+ +------------+              |
+-------------------------------------------------------------+
```

## Database Categories

### Relational Databases (SQL)
Traditional databases with structured schemas, supporting complex queries and ACID transactions.
- **Examples**: PostgreSQL, MySQL, SQL Server, Oracle

### Document Databases
Store data as flexible JSON-like documents, ideal for hierarchical data.
- **Examples**: MongoDB, Cosmos DB

### Key-Value Databases
Simple but fast storage for data indexed by unique keys.
- **Examples**: Redis, DynamoDB

### Graph Databases
Optimized for storing and querying relationships between entities.
- **Examples**: Neo4j, Neptune

### Column-Family Databases
Designed for write-heavy workloads and time-series data.
- **Examples**: Cassandra, HBase

### Time-Series Databases
Specialized for time-stamped data like metrics and IoT readings.
- **Examples**: InfluxDB, TimescaleDB

### Search Databases
Optimized for full-text search and analytics.
- **Examples**: Elasticsearch, Solr

---

## Diagrams in This Section

- [1.1-database-landscape.drawio](./1.1-database-landscape.drawio)
