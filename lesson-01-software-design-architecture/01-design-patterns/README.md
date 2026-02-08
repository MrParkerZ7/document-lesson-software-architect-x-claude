# 🧩 Design Patterns

> **Navigation**: [⬅️ Back to Lesson Overview](../README.md) | [Next: Architecture Styles ➡️](../02-architecture-styles/README.md)

---

Design patterns are reusable solutions to commonly occurring problems in software design.

## 🏭 1.1 Gang of Four (GoF) Patterns

### 🏭 Creational Patterns
| Pattern | Purpose | Use Case |
|---------|---------|----------|
| **🔒 Singleton** | Ensure a class has only one instance | Database connections, logging |
| **🏭 Factory Method** | Create objects without specifying exact class | Plugin systems, UI components |
| **🏗️ Abstract Factory** | Create families of related objects | Cross-platform UI toolkits |
| **🔧 Builder** | Construct complex objects step by step | Configuration objects, queries |
| **📋 Prototype** | Clone existing objects | Object caching, expensive object creation |

### 🧱 Structural Patterns
| Pattern | Purpose | Use Case |
|---------|---------|----------|
| **🔌 Adapter** | Convert interface of a class to another | Legacy system integration |
| **🌉 Bridge** | Separate abstraction from implementation | Cross-platform applications |
| **🌳 Composite** | Compose objects into tree structures | File systems, UI hierarchies |
| **🎨 Decorator** | Add responsibilities dynamically | Stream wrappers, middleware |
| **🏠 Facade** | Provide simplified interface to complex subsystem | API gateways, libraries |
| **🔗 Proxy** | Control access to an object | Lazy loading, access control |

### 🔀 Behavioral Patterns
| Pattern | Purpose | Use Case |
|---------|---------|----------|
| **👁️ Observer** | Define one-to-many dependency | Event systems, reactive programming |
| **🎯 Strategy** | Define family of interchangeable algorithms | Payment processing, sorting |
| **📦 Command** | Encapsulate request as object | Undo/redo, task queues |
| **🔄 State** | Alter behavior when state changes | Workflow engines, game states |
| **📝 Template Method** | Define skeleton of algorithm | Frameworks, lifecycle hooks |
| **⛓️ Chain of Responsibility** | Pass request along chain of handlers | Middleware, validation |

## 🏢 1.2 Enterprise Patterns

| Pattern | Description |
|---------|-------------|
| **💾 Repository** | Mediates between domain and data mapping layers |
| **📦 Unit of Work** | Maintains list of objects affected by transaction |
| **🎯 Service Layer** | Defines application's boundary with a layer of services |
| **🧠 Domain Model** | Object model incorporating behavior and data |
| **📤 Data Transfer Object (DTO)** | Object that carries data between processes |
| **🗺️ Data Mapper** | Layer that moves data between objects and database |

## 🗺️ 1.3 Domain-Driven Design (DDD)

### 🌍 Strategic Design
- **🗺️ Bounded Context**: Explicit boundary within which a domain model exists
- **💬 Ubiquitous Language**: Common language between developers and domain experts
- **🔗 Context Mapping**: Relationships between bounded contexts

### 🔧 Tactical Design (Building Blocks)

The tactical patterns are the building blocks for implementing domain models within a Bounded Context.

#### 🆚 Entities vs Value Objects

| Aspect | 📌 Entity | 💎 Value Object |
|--------|--------|--------------|
| **Identity** | Has unique ID | No ID, defined by attributes |
| **Equality** | By ID only | By all attribute values |
| **Mutability** | Mutable state | Always immutable |
| **Lifecycle** | Create → Modify → Delete | Create → Replace |
| **Example** | `User(id=123)` | `Money(100, USD)` |

**📌 Entity**: Objects with unique identity that persists over time
- Use when you need to track individual instances
- Examples: User, Order, Product (with SKU), Account

**💎 Value Object**: Objects defined entirely by their attributes
- Use when objects are interchangeable if they have same values
- Examples: Money, Address, DateRange, Email

#### 📦 Aggregates

An **Aggregate** is a cluster of domain objects treated as a single unit for data changes. It defines a consistency boundary.

**📋 Aggregate Rules**:
1. ✅ Reference other Aggregates by ID only (not object references)
2. ✅ One transaction per Aggregate
3. ✅ Keep Aggregates small (only what's needed for invariants)
4. ✅ Protect invariants through the Aggregate Root
5. ✅ External access only through the Root

```
┌─────────────────────────────────────────────────┐
│              📦 Aggregate Boundary               │
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

#### ⚡ Domain Events

**Domain Events** capture something significant that happened in the domain. They enable eventual consistency between Aggregates.

**📋 Characteristics**:
- 📌 Immutable (record of the past)
- 📝 Past-tense naming: `OrderPlaced`, `PaymentReceived`
- 📦 Contains all relevant event data
- 🔗 Decouples producer from consumers

**Flow**: Aggregate → publishes Event → Event Bus → Handlers (other Aggregates/Services)

#### 💾 Repository & Domain Service

| Pattern | Purpose | Contains |
|---------|---------|----------|
| **💾 Repository** | Persist/retrieve Aggregates | Data access logic, mapping |
| **⚙️ Domain Service** | Logic that spans entities | Stateless business operations |

**💾 Repository Rules**:
- ✅ One per Aggregate
- ✅ Returns complete Aggregates only
- ✅ Domain-centric interface (not data-centric)
- ✅ Works with Aggregate Roots only

**⚙️ Domain Service** - Use when:
- Operation involves multiple Aggregates
- Logic doesn't naturally belong to any Entity
- Requires external data for calculation

#### 🧩 Complete Tactical Pattern Overview

```
┌─────────────────────────────────────────────────┐
│              🗺️ Bounded Context                 │
│  ┌─────────┐  ┌─────────┐  ┌─────────────────┐ │
│  │📌 Entity│  │💎 Value │  │ 📦 Aggregate    │ │
│  │         │  │  Object │  │    Root         │ │
│  └─────────┘  └─────────┘  └─────────────────┘ │
│                    │                            │
│  ┌─────────────┐   │    ┌──────────────────┐   │
│  │⚙️ Domain   │   │    │ ⚡ Domain Event  │   │
│  │   Service   │   │    │  (OrderPlaced)   │   │
│  └─────────────┘   │    └──────────────────┘   │
│              ┌─────┴─────┐                      │
│              │💾Repository│                      │
│              └───────────┘                      │
└─────────────────────────────────────────────────┘
```

---

## 📊 Diagrams in This Section

- [1.1-creational-patterns-overview.drawio](./1.1-creational-patterns-overview.drawio)
- [1.2-structural-patterns-overview.drawio](./1.2-structural-patterns-overview.drawio)
- [1.3-behavioral-patterns-overview.drawio](./1.3-behavioral-patterns-overview.drawio)
- [1.4-enterprise-patterns-layers.drawio](./1.4-enterprise-patterns-layers.drawio)
- [1.5-ddd-bounded-context.drawio](./1.5-ddd-bounded-context.drawio)
- [1.5.1-ddd-tactical-overview.drawio](./1.5.1-ddd-tactical-overview.drawio)
- [1.5.2-ddd-entity-vs-value-object.drawio](./1.5.2-ddd-entity-vs-value-object.drawio)
- [1.5.3-ddd-aggregate-design.drawio](./1.5.3-ddd-aggregate-design.drawio)
- [1.5.4-ddd-domain-events.drawio](./1.5.4-ddd-domain-events.drawio)
- [1.5.5-ddd-repository-service.drawio](./1.5.5-ddd-repository-service.drawio)
- [1.6-ddd-context-mapping.drawio](./1.6-ddd-context-mapping.drawio)
