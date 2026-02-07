# Communication

> **Navigation**: [Back to Lesson Overview](../README.md) | [Next: Leadership](../02-leadership/README.md)

---

## 1.1 Communication Matrix

```
┌─────────────────────────────────────────────────────────┐
│              Audience Communication Matrix               │
│                                                         │
│  Audience        │ Focus On          │ Avoid           │
│  ────────────────┼───────────────────┼────────────────│
│  Executives      │ Business value,   │ Technical      │
│                  │ ROI, risk, time   │ jargon         │
│  ────────────────┼───────────────────┼────────────────│
│  Product         │ Features, user    │ Implementation │
│  Managers        │ impact, tradeoffs │ details        │
│  ────────────────┼───────────────────┼────────────────│
│  Developers      │ Technical details,│ Over-          │
│                  │ patterns, why     │ simplification │
│  ────────────────┼───────────────────┼────────────────│
│  Operations      │ Deployment, SLAs, │ Code-level     │
│                  │ monitoring        │ details        │
│  ────────────────┼───────────────────┼────────────────│
│  Security        │ Threats, controls,│ Feature        │
│                  │ compliance        │ requirements   │
└─────────────────────────────────────────────────────────┘
```

## 1.2 Technical Writing

**Architecture Decision Records (ADRs)**:
```markdown
# ADR-001: Use PostgreSQL for Primary Database

## Status
Accepted

## Context
We need to select a primary database for our e-commerce platform.
Requirements include:
- ACID compliance for transactions
- JSON support for flexible product attributes
- Strong ecosystem and community support
- Cost-effective licensing

## Decision
We will use PostgreSQL 15 as our primary database.

## Consequences
### Positive
- Excellent JSON/JSONB support for flexible schemas
- Strong community and extensive documentation
- No licensing costs
- Advanced indexing capabilities

### Negative
- Team has more experience with MySQL
- Requires training investment
- Different syntax from SQL Server (legacy systems)

## Alternatives Considered
1. MySQL 8 - Less advanced JSON support
2. SQL Server - Licensing costs prohibitive
3. MongoDB - ACID limitations for transactions
```

**Documentation Principles**:
| Principle | Description |
|-----------|-------------|
| **Audience-first** | Write for your reader, not yourself |
| **Concise** | Remove unnecessary words |
| **Structured** | Use headings, lists, diagrams |
| **Maintained** | Keep documentation current |
| **Accessible** | Easy to find and navigate |

## 1.3 Presentation Skills

**Structure for Technical Presentations**:
```
┌─────────────────────────────────────────────────────────┐
│            Presentation Structure                        │
│                                                         │
│  1. CONTEXT (Why are we here?)                         │
│     • Business problem                                 │
│     • Current state                                    │
│     • Why action is needed                             │
│                                                         │
│  2. PROPOSAL (What do we recommend?)                   │
│     • Solution overview                                │
│     • Key benefits                                     │
│     • High-level architecture                          │
│                                                         │
│  3. DETAILS (How will it work?)                        │
│     • Technical approach                               │
│     • Implementation plan                              │
│     • Resource requirements                            │
│                                                         │
│  4. RISKS & MITIGATIONS                                │
│     • What could go wrong                              │
│     • How we'll address it                             │
│                                                         │
│  5. ASK (What do we need?)                             │
│     • Decision requested                               │
│     • Resources needed                                 │
│     • Next steps                                       │
└─────────────────────────────────────────────────────────┘
```

## 1.4 Visual Communication

**Diagram Types**:
| Diagram | Purpose | When to Use |
|---------|---------|-------------|
| **C4 Context** | System boundaries | Executive overview |
| **C4 Container** | High-level architecture | Technical planning |
| **C4 Component** | Internal structure | Design discussions |
| **Sequence** | Interactions over time | API design, flows |
| **State** | State transitions | Workflow design |
| **ER Diagram** | Data relationships | Database design |

**C4 Model**:
```
┌─────────────────────────────────────────────────────────┐
│                    C4 Model Levels                       │
│                                                         │
│  Level 1: Context                                       │
│  ┌─────────────────────────────────────────────────┐   │
│  │  [Person]                                        │   │
│  │     │                                            │   │
│  │     ▼                                            │   │
│  │  [Software System] ←→ [External System]         │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  Level 2: Container                                     │
│  ┌─────────────────────────────────────────────────┐   │
│  │  [Web App] → [API] → [Database]                 │   │
│  │               ↓                                  │   │
│  │          [Message Queue]                         │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  Level 3: Component                                     │
│  ┌─────────────────────────────────────────────────┐   │
│  │  [Controller] → [Service] → [Repository]        │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  Level 4: Code (optional)                              │
│  • Class diagrams, code structure                      │
└─────────────────────────────────────────────────────────┘
```

---

## Diagrams in This Section

- [1.1-communication-skills.drawio](./1.1-communication-skills.drawio)
