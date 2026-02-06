# 04 - Access Control Models

[<< Previous: Microsoft Entra ID](../03-microsoft-entra-id/README.md) | [Back to Lesson 04 Index](../README.md) | [Next: Zero Trust Architecture >>](../05-zero-trust-architecture/README.md)

---

## Overview

This section covers Role-Based Access Control (RBAC) and Attribute-Based Access Control (ABAC) models.

## Learning Objectives

- Understand RBAC principles and implementation
- Learn ABAC concepts and policy structure
- Choose the right model for your use case

## Key Concepts

### Role-Based Access Control (RBAC)

Access based on assigned roles:
- **Role**: Collection of permissions
- **Permission**: Ability to perform actions
- **Role Hierarchy**: Inheritance structure

Azure RBAC: Owner, Contributor, Reader roles at various scopes.

### Attribute-Based Access Control (ABAC)

Access decisions based on attributes of:
- **Subject**: User attributes (department, clearance)
- **Resource**: Classification, owner
- **Action**: Read, write, delete
- **Environment**: Time, location

### Comparison

| Aspect | RBAC | ABAC |
|--------|------|------|
| Complexity | Simple | Complex |
| Flexibility | Limited | High |
| Best for | Static structures | Dynamic requirements |

## Diagrams

| Diagram | Description |
|---------|-------------|
| [4.1-rbac-abac-models.drawio](./4.1-rbac-abac-models.drawio) | RBAC and ABAC comparison |

---

[<< Previous: Microsoft Entra ID](../03-microsoft-entra-id/README.md) | [Back to Lesson 04 Index](../README.md) | [Next: Zero Trust Architecture >>](../05-zero-trust-architecture/README.md)
