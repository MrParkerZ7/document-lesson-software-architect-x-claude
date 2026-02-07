# NoSQL Databases

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Relational Databases](../02-relational-databases/README.md) | [Next: CAP Theorem & Consistency](../04-cap-theorem-consistency/README.md)

---

## Document Databases

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

## Key-Value Databases

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

## Graph Databases

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

## Wide-Column Databases

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

## Time-Series Databases

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

## Diagrams in This Section

- [3.1-nosql-patterns.drawio](./3.1-nosql-patterns.drawio)
