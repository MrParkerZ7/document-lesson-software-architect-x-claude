# 📐 Architecture Principles

> **Navigation**: [⬅️ Back to Lesson Overview](../README.md) | [Previous: Architecture Styles](../02-architecture-styles/README.md) | [Next: API Design ➡️](../04-api-design/README.md)

---

## 📐 3.1 SOLID Principles

| Principle | Description | Example |
|-----------|-------------|---------|
| **🎯 S**ingle Responsibility | A class should have one reason to change | Separate validation from persistence |
| **🔓 O**pen/Closed | Open for extension, closed for modification | Use interfaces for extensibility |
| **🔄 L**iskov Substitution | Subtypes must be substitutable for base types | Proper inheritance hierarchies |
| **✂️ I**nterface Segregation | Many specific interfaces over one general | Split large interfaces |
| **🔌 D**ependency Inversion | Depend on abstractions, not concretions | Use dependency injection |

## 📋 3.2 Other Key Principles

| Principle | Description |
|-----------|-------------|
| **🔁 DRY** (Don't Repeat Yourself) | Avoid code duplication |
| **💡 KISS** (Keep It Simple, Stupid) | Simplicity over complexity |
| **🚫 YAGNI** (You Aren't Gonna Need It) | Don't build features until needed |
| **🧩 Separation of Concerns** | Divide program into distinct sections |
| **🔗 Composition over Inheritance** | Prefer object composition |
| **⚡ Fail Fast** | Detect and report errors immediately |
| **⚙️ Convention over Configuration** | Sensible defaults reduce configuration |

---

## 📊 Diagrams in This Section

- [3.1-solid-principles.drawio](./3.1-solid-principles.drawio)
- [3.2-dependency-inversion.drawio](./3.2-dependency-inversion.drawio)
