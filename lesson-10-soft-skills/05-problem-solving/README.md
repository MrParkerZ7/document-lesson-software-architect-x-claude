# Problem Solving

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Decision Making](../04-decision-making/README.md) | [Next: Continuous Learning](../06-continuous-learning/README.md)

---

## 5.1 Root Cause Analysis

**5 Whys**:
```
Problem: Website is slow

Why 1: Database queries are taking too long
Why 2: Queries are scanning full tables
Why 3: Indexes are missing on frequently queried columns
Why 4: Schema changes weren't reviewed by DBA
Why 5: No process for database change reviews

Root Cause: Missing database change review process
Solution: Implement mandatory DBA review for schema changes
```

**Fishbone Diagram**:
```
┌─────────────────────────────────────────────────────────┐
│                   Fishbone Diagram                       │
│                                                         │
│  People          Process         Technology            │
│      \              |              /                    │
│       \             |             /                     │
│        \            |            /                      │
│         \           |           /                       │
│          ▼          ▼          ▼                        │
│  ════════════════════════════════════▶ [Problem]       │
│          ▲          ▲          ▲                        │
│         /           |           \                       │
│        /            |            \                      │
│       /             |             \                     │
│      /              |              \                    │
│  Environment    Materials      Measurement             │
└─────────────────────────────────────────────────────────┘
```

## 5.2 Structured Problem Solving

```
┌─────────────────────────────────────────────────────────┐
│           Problem Solving Framework                      │
│                                                         │
│  1. DEFINE                                              │
│     • What exactly is the problem?                     │
│     • Who is affected?                                 │
│     • What is the impact?                              │
│                                                         │
│  2. ANALYZE                                             │
│     • Gather data                                      │
│     • Identify root causes                             │
│     • Map dependencies                                 │
│                                                         │
│  3. GENERATE OPTIONS                                    │
│     • Brainstorm solutions                             │
│     • Consider constraints                             │
│     • Evaluate trade-offs                              │
│                                                         │
│  4. SELECT                                              │
│     • Compare against criteria                         │
│     • Get stakeholder input                            │
│     • Make decision                                    │
│                                                         │
│  5. IMPLEMENT                                           │
│     • Create action plan                               │
│     • Execute                                          │
│     • Monitor results                                  │
│                                                         │
│  6. LEARN                                               │
│     • Document outcomes                                │
│     • Share learnings                                  │
│     • Update processes                                 │
└─────────────────────────────────────────────────────────┘
```

---

## Diagrams in This Section

- [5.1-problem-solving.drawio](./5.1-problem-solving.drawio)
