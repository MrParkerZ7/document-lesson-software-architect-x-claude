# Decision Making

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Business Acumen](../03-business-acumen/README.md) | [Next: Problem Solving](../05-problem-solving/README.md)

---

## 4.1 Decision Frameworks

**Weighted Decision Matrix**:
```
┌─────────────────────────────────────────────────────────┐
│        Database Selection Decision Matrix                │
│                                                         │
│  Criteria      │Weight│ PostgreSQL│ MongoDB │ DynamoDB │
│  ──────────────┼──────┼───────────┼─────────┼──────────│
│  Performance   │  25% │     8     │    9    │    9     │
│  Scalability   │  20% │     7     │    9    │   10     │
│  Cost          │  20% │     9     │    7    │    6     │
│  Team Skills   │  15% │     8     │    6    │    5     │
│  Ecosystem     │  10% │     9     │    8    │    7     │
│  Flexibility   │  10% │     8     │    9    │    6     │
│  ──────────────┼──────┼───────────┼─────────┼──────────│
│  Weighted Score│      │   8.05    │   8.00  │   7.45   │
│                                                         │
│  Winner: PostgreSQL                                    │
└─────────────────────────────────────────────────────────┘
```

**RAPID Framework**:
| Role | Responsibility |
|------|----------------|
| **R**ecommend | Proposes the decision |
| **A**gree | Must agree before proceeding |
| **P**erform | Implements the decision |
| **I**nput | Provides information |
| **D**ecide | Makes final call |

## 4.2 Trade-off Analysis

```
┌─────────────────────────────────────────────────────────┐
│              Common Architecture Trade-offs              │
│                                                         │
│  Consistency ◄─────────────────────► Availability       │
│  (Strong guarantees)                 (Always respond)   │
│                                                         │
│  Performance ◄─────────────────────► Cost              │
│  (Fast response)                     (Budget limits)    │
│                                                         │
│  Security ◄────────────────────────► Usability         │
│  (Protection)                        (User experience)  │
│                                                         │
│  Flexibility ◄─────────────────────► Simplicity        │
│  (Adaptability)                      (Maintainability) │
│                                                         │
│  Build ◄───────────────────────────► Buy              │
│  (Custom fit)                        (Faster start)    │
└─────────────────────────────────────────────────────────┘
```

## 4.3 Avoiding Analysis Paralysis

| Technique | Description |
|-----------|-------------|
| **Timeboxing** | Set deadline for decision |
| **Good enough** | Accept satisficing over optimizing |
| **Reversibility** | Prefer reversible decisions |
| **Experiment** | Test with prototype or pilot |
| **Default option** | Have a fallback if no consensus |

---

## Diagrams in This Section

- [4.1-decision-frameworks.drawio](./4.1-decision-frameworks.drawio)
