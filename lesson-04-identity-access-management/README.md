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

## 📁 Files in This Lesson

### 📚 Sub-Lessons
| # | Topic | README |
|---|-------|--------|
| 01 | Core Concepts | [README](./01-core-concepts/README.md) |
| 02 | Authentication Protocols | [README](./02-authentication-protocols/README.md) |
| 03 | Microsoft Entra ID (Azure AD) | [README](./03-microsoft-entra-id/README.md) |
| 04 | Access Control Models | [README](./04-access-control-models/README.md) |
| 05 | Zero Trust Architecture | [README](./05-zero-trust-architecture/README.md) |
| 06 | B2B and B2C Scenarios | [README](./06-b2b-b2c-scenarios/README.md) |
| 07 | Best Practices | [README](./07-best-practices/README.md) |

### 📊 Diagrams & Images
| Diagram | Image |
|---------|-------|
| [1.1-authn-vs-authz.drawio](./01-core-concepts/1.1-authn-vs-authz.drawio) | [PNG](./01-core-concepts/1.1-authn-vs-authz.png) |
| [1.2-identity-terminology.drawio](./01-core-concepts/1.2-identity-terminology.drawio) | [PNG](./01-core-concepts/1.2-identity-terminology.png) |
| [2.1-oauth2-flows.drawio](./02-authentication-protocols/2.1-oauth2-flows.drawio) | [PNG](./02-authentication-protocols/2.1-oauth2-flows.png) |
| [2.2-oidc-and-saml.drawio](./02-authentication-protocols/2.2-oidc-and-saml.drawio) | [PNG](./02-authentication-protocols/2.2-oidc-and-saml.png) |
| [3.1-entra-id-overview.drawio](./03-microsoft-entra-id/3.1-entra-id-overview.drawio) | [PNG](./03-microsoft-entra-id/3.1-entra-id-overview.png) |
| [3.2-app-registration-managed-identity.drawio](./03-microsoft-entra-id/3.2-app-registration-managed-identity.drawio) | [PNG](./03-microsoft-entra-id/3.2-app-registration-managed-identity.png) |
| [3.3-conditional-access.drawio](./03-microsoft-entra-id/3.3-conditional-access.drawio) | [PNG](./03-microsoft-entra-id/3.3-conditional-access.png) |
| [4.1-rbac-abac-models.drawio](./04-access-control-models/4.1-rbac-abac-models.drawio) | [PNG](./04-access-control-models/4.1-rbac-abac-models.png) |
| [5.1-zero-trust-architecture.drawio](./05-zero-trust-architecture/5.1-zero-trust-architecture.drawio) | [PNG](./05-zero-trust-architecture/5.1-zero-trust-architecture.png) |
| [6.1-b2b-b2c-scenarios.drawio](./06-b2b-b2c-scenarios/6.1-b2b-b2c-scenarios.drawio) | [PNG](./06-b2b-b2c-scenarios/6.1-b2b-b2c-scenarios.png) |
| [7.1-iam-best-practices.drawio](./07-best-practices/7.1-iam-best-practices.drawio) | [PNG](./07-best-practices/7.1-iam-best-practices.png) |

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
