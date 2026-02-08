# Lesson 08: Databases & Storage 🗄️

## 📋 Overview

Data is at the heart of most applications. Software Architects must understand various database types, their strengths and weaknesses, and how to design data architectures that meet performance, scalability, and consistency requirements.

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Choose appropriate database types for different use cases
- Design data models for relational and NoSQL databases
- Understand consistency models and CAP theorem
- Implement caching strategies
- Design storage solutions for different workloads

---

## 📁 Files in This Lesson

### 📚 Sub-Lessons
| # | Topic | README |
|---|-------|--------|
| 01 | Database Types Overview | [README](./01-database-types-overview/README.md) |
| 02 | Relational Databases | [README](./02-relational-databases/README.md) |
| 03 | NoSQL Databases | [README](./03-nosql-databases/README.md) |
| 04 | CAP Theorem & Consistency | [README](./04-cap-theorem-consistency/README.md) |
| 05 | Caching | [README](./05-caching/README.md) |
| 06 | Storage Solutions | [README](./06-storage-solutions/README.md) |
| 07 | Database Selection Guide | [README](./07-database-selection-guide/README.md) |

### 📊 Diagrams & Images
| Diagram | Image |
|---------|-------|
| [1.1-database-landscape.drawio](./01-database-types-overview/1.1-database-landscape.drawio) | [PNG](./01-database-types-overview/1.1-database-landscape.png) |
| [2.1-normalization-indexing.drawio](./02-relational-databases/2.1-normalization-indexing.drawio) | [PNG](./02-relational-databases/2.1-normalization-indexing.png) |
| [3.1-nosql-patterns.drawio](./03-nosql-databases/3.1-nosql-patterns.drawio) | [PNG](./03-nosql-databases/3.1-nosql-patterns.png) |
| [4.1-cap-theorem.drawio](./04-cap-theorem-consistency/4.1-cap-theorem.drawio) | [PNG](./04-cap-theorem-consistency/4.1-cap-theorem.png) |
| [5.1-caching-strategies.drawio](./05-caching/5.1-caching-strategies.drawio) | [PNG](./05-caching/5.1-caching-strategies.png) |
| [6.1-storage-types.drawio](./06-storage-solutions/6.1-storage-types.drawio) | [PNG](./06-storage-solutions/6.1-storage-types.png) |
| [7.1-database-selection.drawio](./07-database-selection-guide/7.1-database-selection.drawio) | [PNG](./07-database-selection-guide/7.1-database-selection.png) |

---

## 1. 📊 Database Types Overview

```
┌─────────────────────────────────────────────────────────┐
│                   Database Landscape                     │
│                                                         │
│  ┌──────────────────────────────────────────────────┐  │
│  │                  Relational (SQL)                 │  │
│  │  PostgreSQL, MySQL, SQL Server, Oracle           │  │
│  └──────────────────────────────────────────────────┘  │
│                                                         │
│  ┌────────────┐ ┌────────────┐ ┌────────────┐        │
│  │  Document  │ │ Key-Value  │ │   Graph    │        │
│  │  MongoDB   │ │   Redis    │ │   Neo4j    │        │
│  │  Cosmos DB │ │  DynamoDB  │ │ Neptune    │        │
│  └────────────┘ └────────────┘ └────────────┘        │
│                                                         │
│  ┌────────────┐ ┌────────────┐ ┌────────────┐        │
│  │   Column   │ │Time-Series │ │   Search   │        │
│  │ Cassandra  │ │ InfluxDB   │ │Elasticsearch│       │
│  │   HBase    │ │TimescaleDB │ │   Solr     │        │
│  └────────────┘ └────────────┘ └────────────┘        │
└─────────────────────────────────────────────────────────┘
```

---

## 2. 🔷 Relational Databases

### 2.1 When to Use Relational

| Use Case | Why Relational |
|----------|----------------|
| Complex queries with JOINs | Strong query language (SQL) |
| ACID transactions | Data integrity guarantees |
| Structured data with relationships | Normalized schema |
| Reporting and analytics | Aggregations, GROUP BY |
| Financial systems | Strong consistency |

### 2.2 Database Comparison

| Feature | PostgreSQL | MySQL | SQL Server |
|---------|------------|-------|------------|
| **License** | Open source | Open source | Commercial |
| **JSON Support** | Excellent | Good | Good |
| **Full-text Search** | Built-in | Built-in | Built-in |
| **Replication** | Streaming, Logical | Master-Slave | Always On |
| **Extensions** | Rich ecosystem | Limited | CLR integration |
| **Best For** | Complex queries, JSONB | Web apps, read-heavy | Enterprise, .NET |

### 2.3 Data Modeling

**Normalization**:
```
┌─────────────────────────────────────────────────────────┐
│              Normalization Forms                         │
│                                                         │
│  1NF: Atomic values, no repeating groups               │
│       ┌────────────────────────────────┐               │
│       │ OrderID │ Product │ Quantity   │               │
│       │ 1       │ Apple   │ 5          │               │
│       │ 1       │ Orange  │ 3          │               │
│       └────────────────────────────────┘               │
│                                                         │
│  2NF: No partial dependencies                          │
│       (Non-key columns depend on whole primary key)    │
│                                                         │
│  3NF: No transitive dependencies                       │
│       (Non-key columns depend only on primary key)     │
│                                                         │
│  Balance: Normalize for writes, denormalize for reads  │
└─────────────────────────────────────────────────────────┘
```

**Schema Design Example**:
```sql
-- E-commerce schema
CREATE TABLE customers (
    customer_id SERIAL PRIMARY KEY,
    email VARCHAR(255) UNIQUE NOT NULL,
    name VARCHAR(100) NOT NULL,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE products (
    product_id SERIAL PRIMARY KEY,
    name VARCHAR(255) NOT NULL,
    price DECIMAL(10, 2) NOT NULL,
    stock_quantity INTEGER DEFAULT 0,
    category_id INTEGER REFERENCES categories(category_id)
);

CREATE TABLE orders (
    order_id SERIAL PRIMARY KEY,
    customer_id INTEGER NOT NULL REFERENCES customers(customer_id),
    status VARCHAR(20) DEFAULT 'pending',
    total_amount DECIMAL(10, 2),
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);

CREATE TABLE order_items (
    order_item_id SERIAL PRIMARY KEY,
    order_id INTEGER NOT NULL REFERENCES orders(order_id),
    product_id INTEGER NOT NULL REFERENCES products(product_id),
    quantity INTEGER NOT NULL,
    unit_price DECIMAL(10, 2) NOT NULL,
    UNIQUE(order_id, product_id)
);

-- Indexes for common queries
CREATE INDEX idx_orders_customer ON orders(customer_id);
CREATE INDEX idx_orders_status ON orders(status);
CREATE INDEX idx_order_items_order ON order_items(order_id);
```

### 2.4 Indexing Strategies

| Index Type | Use Case | Example |
|------------|----------|---------|
| **B-Tree** | Equality, range queries | `WHERE price > 100` |
| **Hash** | Equality only | `WHERE id = 123` |
| **GiST** | Geometric, full-text | PostGIS, text search |
| **GIN** | Array, JSONB | `WHERE tags @> '{tech}'` |
| **Partial** | Subset of rows | `WHERE status = 'active'` |
| **Covering** | Include extra columns | Avoid table lookup |

```sql
-- Composite index
CREATE INDEX idx_orders_customer_date ON orders(customer_id, created_at DESC);

-- Partial index
CREATE INDEX idx_active_orders ON orders(created_at)
WHERE status = 'pending';

-- Covering index (PostgreSQL 11+)
CREATE INDEX idx_products_category ON products(category_id)
INCLUDE (name, price);

-- JSONB index
CREATE INDEX idx_products_metadata ON products
USING GIN (metadata jsonb_path_ops);
```

---

## 3. 📦 NoSQL Databases

### 3.1 Document Databases

**MongoDB / Cosmos DB**

**When to Use**:
- Flexible schema requirements
- Hierarchical data
- Rapid development
- Content management

**Data Model**:
```javascript
// MongoDB document
{
  "_id": ObjectId("..."),
  "customer": {
    "name": "John Doe",
    "email": "john@example.com"
  },
  "items": [
    {
      "productId": "prod_123",
      "name": "Widget",
      "quantity": 2,
      "price": 29.99
    }
  ],
  "shippingAddress": {
    "street": "123 Main St",
    "city": "Seattle",
    "state": "WA",
    "zip": "98101"
  },
  "status": "shipped",
  "createdAt": ISODate("2026-01-15T10:30:00Z")
}
```

**Schema Design Patterns**:
```javascript
// Embedding (1:Few)
{
  "post": {
    "title": "...",
    "comments": [          // Embedded
      { "user": "...", "text": "..." }
    ]
  }
}

// Referencing (1:Many, Many:Many)
{
  "order": {
    "customerId": ObjectId("..."),  // Reference
    "items": [
      { "productId": ObjectId("...") }
    ]
  }
}

// Bucket Pattern (Time-series)
{
  "sensor_id": "sensor_1",
  "date": "2026-01-15",
  "readings": [
    { "time": "10:00", "value": 23.5 },
    { "time": "10:05", "value": 24.1 }
  ]
}
```

### 3.2 Key-Value Databases

**Redis / DynamoDB**

**When to Use**:
- Caching
- Session storage
- Real-time data
- Simple data models

**Redis Data Structures**:
```redis
# Strings
SET user:1:name "John"
GET user:1:name

# Hashes
HSET user:1 name "John" email "john@example.com"
HGETALL user:1

# Lists (queues)
LPUSH queue:orders "order_123"
RPOP queue:orders

# Sets
SADD user:1:tags "premium" "verified"
SMEMBERS user:1:tags

# Sorted Sets (leaderboards)
ZADD leaderboard 1000 "player1" 950 "player2"
ZREVRANGE leaderboard 0 9 WITHSCORES

# Streams (event log)
XADD events * type "purchase" user "user_1"
XREAD STREAMS events 0
```

**DynamoDB Design**:
```javascript
// Single-table design
{
  "PK": "USER#123",           // Partition key
  "SK": "PROFILE",            // Sort key
  "name": "John Doe",
  "email": "john@example.com"
}

{
  "PK": "USER#123",
  "SK": "ORDER#2026-01-15#001",
  "total": 150.00,
  "status": "shipped"
}

// GSI for querying by email
// GSI1PK: email, GSI1SK: created_at
```

### 3.3 Graph Databases

**Neo4j / Neptune**

**When to Use**:
- Social networks
- Recommendation engines
- Fraud detection
- Knowledge graphs

**Cypher Query Examples**:
```cypher
// Create nodes and relationships
CREATE (john:Person {name: 'John', age: 30})
CREATE (jane:Person {name: 'Jane', age: 28})
CREATE (john)-[:FRIENDS_WITH {since: 2020}]->(jane)

// Find friends of friends
MATCH (person:Person {name: 'John'})-[:FRIENDS_WITH*2]-(fof)
WHERE NOT (person)-[:FRIENDS_WITH]-(fof)
RETURN DISTINCT fof.name

// Shortest path
MATCH path = shortestPath(
  (start:Person {name: 'John'})-[*]-(end:Person {name: 'Alice'})
)
RETURN path

// Recommendation: Products bought by similar users
MATCH (user:User {id: '123'})-[:PURCHASED]->(product:Product)
      <-[:PURCHASED]-(similar:User)-[:PURCHASED]->(rec:Product)
WHERE NOT (user)-[:PURCHASED]->(rec)
RETURN rec.name, COUNT(*) as score
ORDER BY score DESC
LIMIT 10
```

### 3.4 Wide-Column Databases

**Cassandra / HBase**

**When to Use**:
- Time-series data
- Write-heavy workloads
- Geographic distribution
- High availability

**Cassandra Data Model**:
```cql
-- Time-series sensor data
CREATE TABLE sensor_readings (
    sensor_id UUID,
    date DATE,
    timestamp TIMESTAMP,
    temperature DOUBLE,
    humidity DOUBLE,
    PRIMARY KEY ((sensor_id, date), timestamp)
) WITH CLUSTERING ORDER BY (timestamp DESC);

-- Query recent readings
SELECT * FROM sensor_readings
WHERE sensor_id = ? AND date = '2026-01-15'
LIMIT 100;
```

### 3.5 Time-Series Databases

**InfluxDB / TimescaleDB**

**When to Use**:
- IoT sensor data
- Application metrics
- Financial tick data
- Log data

**InfluxDB Example**:
```influxql
-- Write data
INSERT cpu,host=server01,region=us-west value=0.64 1609459200000000000

-- Query with aggregation
SELECT mean("value")
FROM "cpu"
WHERE time > now() - 1h
GROUP BY time(5m), "host"
```

**TimescaleDB (PostgreSQL extension)**:
```sql
-- Create hypertable
CREATE TABLE metrics (
    time        TIMESTAMPTZ NOT NULL,
    sensor_id   INTEGER,
    temperature DOUBLE PRECISION,
    humidity    DOUBLE PRECISION
);

SELECT create_hypertable('metrics', 'time');

-- Query with time bucket
SELECT
    time_bucket('1 hour', time) AS hour,
    sensor_id,
    AVG(temperature) as avg_temp
FROM metrics
WHERE time > NOW() - INTERVAL '24 hours'
GROUP BY hour, sensor_id;
```

---

## 4. ⚖️ CAP Theorem & Consistency

### 4.1 CAP Theorem

```
┌─────────────────────────────────────────────────────────┐
│                    CAP Theorem                           │
│                                                         │
│              Consistency (C)                            │
│                    /\                                   │
│                   /  \                                  │
│                  /    \                                 │
│                 / CA   \                                │
│                /        \                               │
│               /          \                              │
│              /     CP     \                             │
│             /______________\                            │
│  Availability (A)          Partition                   │
│                            Tolerance (P)               │
│                                                         │
│  Pick 2 (in presence of network partition):            │
│  • CP: Consistent + Partition Tolerant                 │
│    (MongoDB, HBase, Redis Cluster)                     │
│  • AP: Available + Partition Tolerant                  │
│    (Cassandra, DynamoDB, CouchDB)                      │
│  • CA: Consistent + Available                          │
│    (Traditional RDBMS - single node)                   │
└─────────────────────────────────────────────────────────┘
```

### 4.2 Consistency Models

| Model | Description | Example |
|-------|-------------|---------|
| **Strong** | All reads see latest write | PostgreSQL, Spanner |
| **Eventual** | Reads eventually see latest write | Cassandra, DynamoDB |
| **Causal** | Causally related ops seen in order | MongoDB sessions |
| **Read-your-writes** | Client sees own writes | Session consistency |
| **Monotonic reads** | Won't see older data after newer | Client sessions |

### 4.3 ACID vs BASE

```
┌─────────────────────────────────────────────────────────┐
│              ACID vs BASE                                │
│                                                         │
│  ACID (Relational)         BASE (NoSQL)                │
│  ─────────────────         ─────────────                │
│  Atomicity                 Basically Available          │
│  Consistency               Soft state                   │
│  Isolation                 Eventually consistent        │
│  Durability                                             │
│                                                         │
│  Use ACID when:            Use BASE when:              │
│  • Data integrity critical • High availability needed  │
│  • Complex transactions    • Global distribution       │
│  • Financial systems       • Scale is priority         │
│  • Regulatory compliance   • Eventual consistency OK   │
└─────────────────────────────────────────────────────────┘
```

---

## 5. ⚡ Caching

### 5.1 Caching Strategies

```
┌─────────────────────────────────────────────────────────┐
│                 Caching Patterns                         │
│                                                         │
│  Cache-Aside (Lazy Loading):                           │
│  ┌──────┐   1. Check cache                             │
│  │Client│─────────────────▶┌───────┐                   │
│  │      │◀─────────────────│ Cache │                   │
│  │      │   2. Cache miss  └───────┘                   │
│  │      │─────────────────▶┌───────┐                   │
│  │      │   3. Get from DB │  DB   │                   │
│  │      │◀─────────────────└───────┘                   │
│  │      │   4. Update cache                            │
│  │      │─────────────────▶┌───────┐                   │
│  └──────┘                  │ Cache │                   │
│                            └───────┘                   │
│                                                         │
│  Write-Through:                                        │
│  • Write to cache and DB synchronously                 │
│  • Ensures cache consistency                           │
│  • Higher write latency                                │
│                                                         │
│  Write-Behind (Write-Back):                            │
│  • Write to cache, async write to DB                   │
│  • Lower write latency                                 │
│  • Risk of data loss                                   │
└─────────────────────────────────────────────────────────┘
```

### 5.2 Cache Invalidation

| Strategy | Description | Use Case |
|----------|-------------|----------|
| **TTL** | Expire after time | Session data |
| **Event-based** | Invalidate on data change | Real-time updates |
| **Version-based** | Include version in key | Configuration |
| **LRU** | Remove least recently used | Memory management |

### 5.3 Redis Caching Example

```python
import redis
import json
from functools import wraps

redis_client = redis.Redis(host='localhost', port=6379, db=0)

def cache(ttl_seconds=300):
    def decorator(func):
        @wraps(func)
        def wrapper(*args, **kwargs):
            # Generate cache key
            key = f"{func.__name__}:{args}:{kwargs}"

            # Check cache
            cached = redis_client.get(key)
            if cached:
                return json.loads(cached)

            # Execute function
            result = func(*args, **kwargs)

            # Store in cache
            redis_client.setex(key, ttl_seconds, json.dumps(result))

            return result
        return wrapper
    return decorator

@cache(ttl_seconds=60)
def get_user(user_id):
    # Expensive database query
    return db.query(f"SELECT * FROM users WHERE id = {user_id}")
```

---

## 6. 💾 Storage Solutions

### 6.1 Object Storage

**Azure Blob Storage / S3 / GCS**

```
┌─────────────────────────────────────────────────────────┐
│                  Object Storage Tiers                    │
│                                                         │
│  Hot        │ Frequent access    │ Highest cost        │
│  ──────────────────────────────────────────────────────│
│  Cool       │ Infrequent access  │ Lower storage cost  │
│             │ (30+ days)         │ Higher access cost  │
│  ──────────────────────────────────────────────────────│
│  Archive    │ Rare access        │ Lowest storage cost │
│             │ (180+ days)        │ Retrieval delays    │
└─────────────────────────────────────────────────────────┘
```

**Use Cases**:
- Static website hosting
- Backup and archival
- Data lakes
- Media storage

### 6.2 Block vs File vs Object

| Type | Protocol | Use Case |
|------|----------|----------|
| **Block** | iSCSI, FC | Databases, VMs |
| **File** | NFS, SMB | Shared files, legacy apps |
| **Object** | HTTP/REST | Unstructured data, scalability |

---

## 7. 🧭 Database Selection Guide

```
┌─────────────────────────────────────────────────────────┐
│              Database Selection Flowchart                │
│                                                         │
│                    Start                                │
│                      │                                  │
│              Need ACID transactions?                    │
│                 /          \                            │
│               Yes           No                          │
│                │             │                          │
│          Relational     Data structure?                 │
│          (PostgreSQL,      /    |    \                  │
│           MySQL)       Key-   Doc   Graph              │
│                       Value                             │
│                         │      │      │                 │
│                      Redis  MongoDB  Neo4j             │
│                                                         │
│  Consider also:                                        │
│  • Scale requirements → Cassandra, DynamoDB            │
│  • Time-series → InfluxDB, TimescaleDB                │
│  • Search → Elasticsearch                              │
│  • Analytics → ClickHouse, BigQuery                    │
└─────────────────────────────────────────────────────────┘
```

---

## 🏋️ Practical Exercises

1. **Schema Design**: Design a database schema for a social media platform
2. **NoSQL Modeling**: Model an e-commerce system in MongoDB with proper embedding/referencing
3. **Caching Strategy**: Implement caching for a read-heavy API
4. **Performance Tuning**: Analyze and optimize slow SQL queries

---

## 📖 Further Reading

- "Designing Data-Intensive Applications" - Martin Kleppmann
- "Database Internals" - Alex Petrov
- "Seven Databases in Seven Weeks" - Luc Perkins
- "NoSQL Distilled" - Pramod Sadalage & Martin Fowler

---

## 📝 Summary

Choosing the right database is crucial for application success. Relational databases excel at complex queries and transactions, while NoSQL databases offer flexibility and scalability for specific use cases. Understanding CAP theorem, consistency models, and caching strategies enables architects to design data architectures that meet performance, availability, and consistency requirements.
