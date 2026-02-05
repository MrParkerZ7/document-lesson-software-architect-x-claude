# Lesson 01: Software Design & Architecture

## Overview

Software Design & Architecture is the foundation of building scalable, maintainable, and robust systems. As a Software Architect, mastering design patterns, architecture styles, and principles is essential for making informed decisions that impact the entire software lifecycle.

---

## Learning Objectives

By the end of this lesson, you will be able to:

- Understand and apply common design patterns
- Choose appropriate architecture styles for different scenarios
- Apply SOLID principles and other architectural guidelines
- Design effective APIs
- Model data architectures for various use cases

---

## 1. Design Patterns

Design patterns are reusable solutions to commonly occurring problems in software design.

### 1.1 Gang of Four (GoF) Patterns

#### Creational Patterns
| Pattern | Purpose | Use Case |
|---------|---------|----------|
| **Singleton** | Ensure a class has only one instance | Database connections, logging |
| **Factory Method** | Create objects without specifying exact class | Plugin systems, UI components |
| **Abstract Factory** | Create families of related objects | Cross-platform UI toolkits |
| **Builder** | Construct complex objects step by step | Configuration objects, queries |
| **Prototype** | Clone existing objects | Object caching, expensive object creation |

#### Structural Patterns
| Pattern | Purpose | Use Case |
|---------|---------|----------|
| **Adapter** | Convert interface of a class to another | Legacy system integration |
| **Bridge** | Separate abstraction from implementation | Cross-platform applications |
| **Composite** | Compose objects into tree structures | File systems, UI hierarchies |
| **Decorator** | Add responsibilities dynamically | Stream wrappers, middleware |
| **Facade** | Provide simplified interface to complex subsystem | API gateways, libraries |
| **Proxy** | Control access to an object | Lazy loading, access control |

#### Behavioral Patterns
| Pattern | Purpose | Use Case |
|---------|---------|----------|
| **Observer** | Define one-to-many dependency | Event systems, reactive programming |
| **Strategy** | Define family of interchangeable algorithms | Payment processing, sorting |
| **Command** | Encapsulate request as object | Undo/redo, task queues |
| **State** | Alter behavior when state changes | Workflow engines, game states |
| **Template Method** | Define skeleton of algorithm | Frameworks, lifecycle hooks |
| **Chain of Responsibility** | Pass request along chain of handlers | Middleware, validation |

### 1.2 Enterprise Patterns

| Pattern | Description |
|---------|-------------|
| **Repository** | Mediates between domain and data mapping layers |
| **Unit of Work** | Maintains list of objects affected by transaction |
| **Service Layer** | Defines application's boundary with a layer of services |
| **Domain Model** | Object model incorporating behavior and data |
| **Data Transfer Object (DTO)** | Object that carries data between processes |
| **Data Mapper** | Layer that moves data between objects and database |

### 1.3 Domain-Driven Design (DDD)

#### Strategic Design
- **Bounded Context**: Explicit boundary within which a domain model exists
- **Ubiquitous Language**: Common language between developers and domain experts
- **Context Mapping**: Relationships between bounded contexts

> **Diagrams**: See `1.5-ddd-bounded-context.drawio` and `1.6-ddd-context-mapping.drawio`

#### Tactical Design (Building Blocks)

The tactical patterns are the building blocks for implementing domain models within a Bounded Context.

> **Diagrams**: See `1.5.1-ddd-tactical-overview.drawio` for complete overview

##### Entities vs Value Objects

| Aspect | Entity | Value Object |
|--------|--------|--------------|
| **Identity** | Has unique ID | No ID, defined by attributes |
| **Equality** | By ID only | By all attribute values |
| **Mutability** | Mutable state | Always immutable |
| **Lifecycle** | Create → Modify → Delete | Create → Replace |
| **Example** | `User(id=123)` | `Money(100, USD)` |

**Entity**: Objects with unique identity that persists over time
- Use when you need to track individual instances
- Examples: User, Order, Product (with SKU), Account

**Value Object**: Objects defined entirely by their attributes
- Use when objects are interchangeable if they have same values
- Examples: Money, Address, DateRange, Email

> **Diagram**: See `1.5.2-ddd-entity-vs-value-object.drawio`

##### Aggregates

An **Aggregate** is a cluster of domain objects treated as a single unit for data changes. It defines a consistency boundary.

**Aggregate Rules**:
1. Reference other Aggregates by ID only (not object references)
2. One transaction per Aggregate
3. Keep Aggregates small (only what's needed for invariants)
4. Protect invariants through the Aggregate Root
5. External access only through the Root

```
┌─────────────────────────────────────────────────┐
│              Aggregate Boundary                 │
│  ┌─────────────────────────────────────────┐   │
│  │     Order (Aggregate Root)              │◄──── Only entry point
│  │     - orderId, customerId, status       │   │
│  │     + addItem(), confirm(), cancel()    │   │
│  └─────────────────────────────────────────┘   │
│           │                                     │
│           ▼                                     │
│  ┌─────────────────┐  ┌─────────────────┐      │
│  │ OrderLine       │  │ Money (VO)      │      │
│  │ (Entity)        │  │ Address (VO)    │      │
│  └─────────────────┘  └─────────────────┘      │
└─────────────────────────────────────────────────┘
```

> **Diagram**: See `1.5.3-ddd-aggregate-design.drawio`

##### Domain Events

**Domain Events** capture something significant that happened in the domain. They enable eventual consistency between Aggregates.

**Characteristics**:
- Immutable (record of the past)
- Past-tense naming: `OrderPlaced`, `PaymentReceived`
- Contains all relevant event data
- Decouples producer from consumers

**Flow**: Aggregate → publishes Event → Event Bus → Handlers (other Aggregates/Services)

> **Diagram**: See `1.5.4-ddd-domain-events.drawio`

##### Repository & Domain Service

| Pattern | Purpose | Contains |
|---------|---------|----------|
| **Repository** | Persist/retrieve Aggregates | Data access logic, mapping |
| **Domain Service** | Logic that spans entities | Stateless business operations |

**Repository Rules**:
- One per Aggregate
- Returns complete Aggregates only
- Domain-centric interface (not data-centric)
- Works with Aggregate Roots only

**Domain Service** - Use when:
- Operation involves multiple Aggregates
- Logic doesn't naturally belong to any Entity
- Requires external data for calculation

> **Diagram**: See `1.5.5-ddd-repository-service.drawio`

##### Complete Tactical Pattern Overview

```
┌─────────────────────────────────────────────────┐
│              Bounded Context                    │
│  ┌─────────┐  ┌─────────┐  ┌─────────────────┐ │
│  │ Entity  │  │ Value   │  │   Aggregate     │ │
│  │         │  │ Object  │  │   Root          │ │
│  └─────────┘  └─────────┘  └─────────────────┘ │
│                    │                            │
│  ┌─────────────┐   │    ┌──────────────────┐   │
│  │  Domain     │   │    │  Domain Event    │   │
│  │  Service    │   │    │  (OrderPlaced)   │   │
│  └─────────────┘   │    └──────────────────┘   │
│              ┌─────┴─────┐                      │
│              │Repository │                      │
│              └───────────┘                      │
└─────────────────────────────────────────────────┘
```

---

## 2. Architecture Styles

### 2.1 Monolithic Architecture

**Description**: Single deployable unit containing all application functionality.

```
┌────────────────────────────────────────┐
│            Monolithic App              │
│  ┌──────────┬──────────┬────────────┐ │
│  │    UI    │ Business │   Data     │ │
│  │   Layer  │  Logic   │   Layer    │ │
│  └──────────┴──────────┴────────────┘ │
│                  │                     │
│            ┌─────┴─────┐               │
│            │ Database  │               │
│            └───────────┘               │
└────────────────────────────────────────┘
```

| Pros | Cons |
|------|------|
| Simple to develop and deploy | Difficult to scale individual components |
| Easy debugging and testing | Technology lock-in |
| Low operational complexity | Large codebase becomes unwieldy |
| Good for small teams | Long deployment cycles |

**When to Use**: Small applications, startups, MVPs, small teams

### 2.2 Microservices Architecture

**Description**: Application as suite of small, independently deployable services.

```
┌─────────┐  ┌─────────┐  ┌─────────┐
│Service A│  │Service B│  │Service C│
│   API   │  │   API   │  │   API   │
│   DB    │  │   DB    │  │   DB    │
└────┬────┘  └────┬────┘  └────┬────┘
     │            │            │
     └────────────┼────────────┘
                  │
            ┌─────┴─────┐
            │API Gateway│
            └───────────┘
```

| Pros | Cons |
|------|------|
| Independent scaling | Distributed system complexity |
| Technology flexibility | Network latency |
| Fault isolation | Data consistency challenges |
| Team autonomy | Operational overhead |

**When to Use**: Large applications, multiple teams, need for independent scaling

### 2.3 Service-Oriented Architecture (SOA)

**Description**: Services communicate over a network using an Enterprise Service Bus (ESB).

```
┌─────────┐  ┌─────────┐  ┌─────────┐
│Service 1│  │Service 2│  │Service 3│
└────┬────┘  └────┬────┘  └────┬────┘
     │            │            │
┌────┴────────────┴────────────┴────┐
│       Enterprise Service Bus       │
└───────────────────────────────────┘
```

**Key Concepts**:
- Service contracts
- Service reusability
- Service composability
- Enterprise Service Bus (ESB)

### 2.4 Serverless Architecture

**Description**: Application logic runs in stateless compute containers, event-triggered and fully managed.

```
┌──────────┐    ┌──────────┐    ┌──────────┐
│  Event   │───▶│ Function │───▶│ Database │
│  Source  │    │ (Lambda) │    │ (DynamoDB│
└──────────┘    └──────────┘    └──────────┘
     │
     ▼
┌──────────┐    ┌──────────┐
│  API     │───▶│ Function │
│ Gateway  │    │          │
└──────────┘    └──────────┘
```

| Pros | Cons |
|------|------|
| No server management | Cold start latency |
| Auto-scaling | Vendor lock-in |
| Pay-per-use | Limited execution time |
| Reduced operational cost | Debugging complexity |

**When to Use**: Event-driven workloads, variable traffic, rapid prototyping

### 2.5 Event-Driven Architecture (EDA)

**Description**: Communication through events; producers emit events, consumers react to them.

```
┌──────────┐    ┌─────────────┐    ┌──────────┐
│ Producer │───▶│Event Broker │───▶│ Consumer │
└──────────┘    │(Kafka/RMQ)  │    └──────────┘
                └──────┬──────┘
                       │
                ┌──────┴──────┐
                │  Consumer   │
                └─────────────┘
```

*(Covered in detail in Lesson 02)*

---

## 3. Architecture Principles

### 3.1 SOLID Principles

| Principle | Description | Example |
|-----------|-------------|---------|
| **S**ingle Responsibility | A class should have one reason to change | Separate validation from persistence |
| **O**pen/Closed | Open for extension, closed for modification | Use interfaces for extensibility |
| **L**iskov Substitution | Subtypes must be substitutable for base types | Proper inheritance hierarchies |
| **I**nterface Segregation | Many specific interfaces over one general | Split large interfaces |
| **D**ependency Inversion | Depend on abstractions, not concretions | Use dependency injection |

### 3.2 Other Key Principles

| Principle | Description |
|-----------|-------------|
| **DRY** (Don't Repeat Yourself) | Avoid code duplication |
| **KISS** (Keep It Simple, Stupid) | Simplicity over complexity |
| **YAGNI** (You Aren't Gonna Need It) | Don't build features until needed |
| **Separation of Concerns** | Divide program into distinct sections |
| **Composition over Inheritance** | Prefer object composition |
| **Fail Fast** | Detect and report errors immediately |
| **Convention over Configuration** | Sensible defaults reduce configuration |

---

## 4. API Design

### 4.1 REST (Representational State Transfer)

**Principles**:
- Stateless communication
- Uniform interface
- Resource-based URLs
- HTTP methods for operations

**HTTP Methods**:
| Method | Purpose | Idempotent |
|--------|---------|------------|
| GET | Retrieve resource | Yes |
| POST | Create resource | No |
| PUT | Update/replace resource | Yes |
| PATCH | Partial update | No |
| DELETE | Remove resource | Yes |

**Best Practices**:
```
GET    /api/users          # List users
GET    /api/users/123      # Get user 123
POST   /api/users          # Create user
PUT    /api/users/123      # Replace user 123
PATCH  /api/users/123      # Update user 123
DELETE /api/users/123      # Delete user 123
```

### 4.2 GraphQL

**Description**: Query language for APIs, allowing clients to request exactly what they need.

```graphql
# Query
query {
  user(id: "123") {
    name
    email
    orders {
      id
      total
    }
  }
}
```

| Pros | Cons |
|------|------|
| Client specifies data needs | Complexity in server implementation |
| Single endpoint | Caching is harder |
| Strong typing | N+1 query problem |
| Introspection | Learning curve |

### 4.3 gRPC

**Description**: High-performance RPC framework using Protocol Buffers.

```protobuf
service UserService {
  rpc GetUser (UserRequest) returns (UserResponse);
  rpc ListUsers (ListRequest) returns (stream User);
}
```

| Pros | Cons |
|------|------|
| High performance (binary) | Less human-readable |
| Bi-directional streaming | Browser support limited |
| Strong typing | Requires HTTP/2 |
| Code generation | Steeper learning curve |

### 4.4 WebSocket

**Description**: Full-duplex communication channel over a single TCP connection.

**Use Cases**:
- Real-time applications
- Chat applications
- Live feeds
- Gaming

---

## 5. Data Architecture

### 5.1 Data Modeling

**Approaches**:
- **Conceptual Model**: High-level business entities
- **Logical Model**: Detailed structure without implementation
- **Physical Model**: Database-specific implementation

### 5.2 ETL vs ELT

| Aspect | ETL | ELT |
|--------|-----|-----|
| **Process** | Extract → Transform → Load | Extract → Load → Transform |
| **Transformation** | Outside data warehouse | Inside data warehouse |
| **Best For** | Structured data, smaller volumes | Large volumes, cloud DW |
| **Tools** | Informatica, Talend | dbt, Spark |

### 5.3 Data Warehouse vs Data Lake

| Aspect | Data Warehouse | Data Lake |
|--------|----------------|-----------|
| **Data Type** | Structured | Raw (any format) |
| **Schema** | Schema-on-write | Schema-on-read |
| **Processing** | ETL | ELT |
| **Users** | Business analysts | Data scientists |
| **Cost** | Higher storage cost | Lower storage cost |

---

## Practical Exercises

1. **Design Pattern Exercise**: Implement a payment processing system using the Strategy pattern
2. **Architecture Decision**: Given a startup scenario, justify choosing between monolithic and microservices
3. **API Design**: Design a RESTful API for an e-commerce platform
4. **DDD Exercise**: Identify bounded contexts for a hospital management system

---

## Further Reading

- "Design Patterns: Elements of Reusable Object-Oriented Software" - Gang of Four
- "Domain-Driven Design" - Eric Evans
- "Clean Architecture" - Robert C. Martin
- "Building Microservices" - Sam Newman
- "Fundamentals of Software Architecture" - Mark Richards & Neal Ford

---

## Diagrams Reference

All diagrams are located in the `diagrams/` folder in Draw.io format.

### 1. Design Patterns
| File | Description |
|------|-------------|
| `1.1-creational-patterns-overview.drawio` | Singleton, Factory, Builder, Prototype patterns |
| `1.2-structural-patterns-overview.drawio` | Adapter, Bridge, Composite, Decorator, Facade, Proxy |
| `1.3-behavioral-patterns-overview.drawio` | Observer, Strategy, Command, State, Chain of Responsibility |
| `1.4-enterprise-patterns-layers.drawio` | Repository, Unit of Work, Service Layer, DTO |
| `1.5-ddd-bounded-context.drawio` | DDD Tactical Patterns Overview (legacy naming) |
| `1.5.1-ddd-tactical-overview.drawio` | All DDD building blocks overview |
| `1.5.2-ddd-entity-vs-value-object.drawio` | Entity vs Value Object comparison |
| `1.5.3-ddd-aggregate-design.drawio` | Aggregate design rules and examples |
| `1.5.4-ddd-domain-events.drawio` | Domain Events flow and patterns |
| `1.5.5-ddd-repository-service.drawio` | Repository and Domain Service patterns |
| `1.6-ddd-context-mapping.drawio` | DDD Strategic - Context Mapping |

### 2. Architecture Styles
| File | Description |
|------|-------------|
| `2.1-monolithic-architecture.drawio` | Monolithic architecture overview |
| `2.2-microservices-architecture.drawio` | Microservices patterns and communication |
| `2.3-soa-esb-architecture.drawio` | SOA with Enterprise Service Bus |
| `2.4-serverless-architecture.drawio` | FaaS and serverless patterns |
| `2.5-event-driven-architecture.drawio` | Event-driven patterns (intro) |
| `2.6-architecture-styles-comparison.drawio` | Comparison of all styles |

### 3. Principles
| File | Description |
|------|-------------|
| `3.1-solid-principles.drawio` | SOLID principles visualization |
| `3.2-dependency-inversion.drawio` | DIP in detail |

### 4. API Design
| File | Description |
|------|-------------|
| `4.1-rest-api-flow.drawio` | REST API request/response flow |
| `4.2-graphql-flow.drawio` | GraphQL query resolution |
| `4.3-grpc-communication.drawio` | gRPC bidirectional streaming |
| `4.4-websocket-flow.drawio` | WebSocket connection lifecycle |
| `4.5-api-styles-comparison.drawio` | REST vs GraphQL vs gRPC comparison |

### 5. Data Architecture
| File | Description |
|------|-------------|
| `5.1-data-modeling-levels.drawio` | Conceptual, Logical, Physical models |
| `5.2-etl-vs-elt.drawio` | ETL vs ELT comparison |
| `5.3-data-warehouse-vs-lake.drawio` | Data Warehouse vs Data Lake |

---

## Summary

Software Design & Architecture forms the backbone of any successful software system. Understanding patterns, principles, and styles enables architects to make informed decisions that balance immediate needs with long-term maintainability. The key is choosing the right approach for the specific context rather than applying one-size-fits-all solutions.
