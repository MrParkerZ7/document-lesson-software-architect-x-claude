# Lesson 04: Identity & Access Management (IAM)

## Overview

Identity and Access Management (IAM) is the security discipline that ensures the right individuals have appropriate access to technology resources. As a Software Architect, understanding authentication, authorization, and identity federation is critical for building secure systems.

---

## Learning Objectives

By the end of this lesson, you will be able to:

- Understand authentication protocols (OAuth 2.0, OpenID Connect, SAML)
- Design identity solutions using Microsoft Entra ID (Azure AD)
- Implement various access control models (RBAC, ABAC)
- Apply Zero Trust security principles
- Configure B2B and B2C identity scenarios

---

## Sub-lessons

| # | Topic | Diagrams |
|---|-------|----------|
| 01 | [Core Concepts](./01-core-concepts/README.md) | 2 |
| 02 | [Authentication Protocols](./02-authentication-protocols/README.md) | 2 |
| 03 | [Microsoft Entra ID (Azure AD)](./03-microsoft-entra-id/README.md) | 3 |
| 04 | [Access Control Models](./04-access-control-models/README.md) | 1 |
| 05 | [Zero Trust Architecture](./05-zero-trust-architecture/README.md) | 1 |
| 06 | [B2B and B2C Scenarios](./06-b2b-b2c-scenarios/README.md) | 1 |
| 07 | [Best Practices](./07-best-practices/README.md) | 1 |

---

## Key Concepts Quick Reference

### Authentication vs Authorization

| Concept | Question | Purpose |
|---------|----------|---------|
| **Authentication (AuthN)** | "Who are you?" | Verify identity |
| **Authorization (AuthZ)** | "What can you do?" | Control access |

### OAuth 2.0 Flows

| Flow | Use Case |
|------|----------|
| Authorization Code | Web applications |
| Authorization Code + PKCE | Mobile/SPA (public clients) |
| Client Credentials | Service-to-service |
| Device Code | Input-constrained devices |

### Access Control Models

| Model | Description | Best For |
|-------|-------------|----------|
| **RBAC** | Role-based permissions | Static organizational structures |
| **ABAC** | Attribute-based policies | Dynamic, context-aware access |

---

## Practical Exercises

1. **OAuth Flow Implementation**: Implement Authorization Code flow with PKCE for a SPA
2. **Entra ID Integration**: Configure app registration and managed identity
3. **Conditional Access**: Design policies for a hybrid work environment
4. **RBAC Design**: Create role hierarchy for a multi-tenant application

---

## Further Reading

- Microsoft Entra ID Documentation
- OAuth 2.0 RFC 6749
- OpenID Connect Core 1.0
- NIST Zero Trust Architecture (SP 800-207)
- "Identity and Data Security for Web Development" - Jonathan LeBlanc

---

## Summary

Identity and Access Management is foundational to secure systems. Understanding authentication protocols (OAuth 2.0, OIDC, SAML), implementing proper access control (RBAC/ABAC), and adopting Zero Trust principles enables architects to build systems that protect resources while providing seamless user experiences.
