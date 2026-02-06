# 03 - Microsoft Entra ID (Azure AD)

[<< Previous: Authentication Protocols](../02-authentication-protocols/README.md) | [Back to Lesson 04 Index](../README.md) | [Next: Access Control Models >>](../04-access-control-models/README.md)

---

## Overview

This section explores Microsoft Entra ID (formerly Azure AD), covering app registrations, managed identities, and conditional access policies.

## Learning Objectives

- Understand Microsoft Entra ID architecture
- Configure app registrations and managed identities
- Implement conditional access policies

## Key Concepts

### Entra ID Overview

Cloud-based identity service providing:
- Identity and group management
- Application integration and SSO
- B2B/B2C identity scenarios
- Conditional Access and PIM

### App Registration & Managed Identity

**App Registration** for OAuth 2.0/OIDC integration:
- Application and Directory IDs
- Client secrets or certificates
- API permissions

**Managed Identity** for Azure resources:
- System-assigned: Tied to one resource
- User-assigned: Shareable identity

### Conditional Access

If-then policies based on signals (user, location, device, risk) with decisions (allow, block, require MFA).

## Diagrams

| Diagram | Description |
|---------|-------------|
| [3.1-entra-id-overview.drawio](./3.1-entra-id-overview.drawio) | Entra ID architecture |
| [3.2-app-registration-managed-identity.drawio](./3.2-app-registration-managed-identity.drawio) | App registration and managed identity |
| [3.3-conditional-access.drawio](./3.3-conditional-access.drawio) | Conditional access framework |

---

[<< Previous: Authentication Protocols](../02-authentication-protocols/README.md) | [Back to Lesson 04 Index](../README.md) | [Next: Access Control Models >>](../04-access-control-models/README.md)
