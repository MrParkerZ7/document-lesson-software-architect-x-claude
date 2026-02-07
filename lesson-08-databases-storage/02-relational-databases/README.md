# Relational Databases

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Database Types Overview](../01-database-types-overview/README.md) | [Next: NoSQL Databases](../03-nosql-databases/README.md)

---

## When to Use Relational

| Use Case | Why Relational |
|----------|----------------|
| Complex queries with JOINs | Strong query language (SQL) |
| ACID transactions | Data integrity guarantees |
| Structured data with relationships | Normalized schema |
| Reporting and analytics | Aggregations, GROUP BY |
| Financial systems | Strong consistency |

## Database Comparison

| Feature | PostgreSQL | MySQL | SQL Server |
|---------|------------|-------|------------|
| **License** | Open source | Open source | Commercial |
| **JSON Support** | Excellent | Good | Good |
| **Full-text Search** | Built-in | Built-in | Built-in |
| **Replication** | Streaming, Logical | Master-Slave | Always On |
| **Extensions** | Rich ecosystem | Limited | CLR integration |
| **Best For** | Complex queries, JSONB | Web apps, read-heavy | Enterprise, .NET |

## Data Modeling

**Normalization**:
```
+-------------------------------------------------------------+
|              Normalization Forms                             |
|                                                             |
|  1NF: Atomic values, no repeating groups                   |
|       +--------------------------------+                   |
|       | OrderID | Product | Quantity   |                   |
|       | 1       | Apple   | 5          |                   |
|       | 1       | Orange  | 3          |                   |
|       +--------------------------------+                   |
|                                                             |
|  2NF: No partial dependencies                              |
|       (Non-key columns depend on whole primary key)        |
|                                                             |
|  3NF: No transitive dependencies                           |
|       (Non-key columns depend only on primary key)         |
|                                                             |
|  Balance: Normalize for writes, denormalize for reads      |
+-------------------------------------------------------------+
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

## Indexing Strategies

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

## Diagrams in This Section

- [2.1-normalization-indexing.drawio](./2.1-normalization-indexing.drawio)
