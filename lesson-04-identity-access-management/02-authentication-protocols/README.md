# 02 - Authentication Protocols

[<< Previous: Core Concepts](../01-core-concepts/README.md) | [Back to Lesson 04 Index](../README.md) | [Next: Microsoft Entra ID >>](../03-microsoft-entra-id/README.md)

---

## Overview

This section covers the major authentication and authorization protocols: OAuth 2.0, OpenID Connect (OIDC), and SAML.

## Learning Objectives

- Understand OAuth 2.0 flows and when to use each
- Learn how OpenID Connect extends OAuth 2.0 for authentication
- Compare OIDC and SAML for enterprise scenarios

## Key Concepts

### OAuth 2.0

Authorization framework enabling third-party access to resources. Key flows:
- **Authorization Code Flow**: For server-side apps (most secure)
- **Authorization Code with PKCE**: For public clients (mobile/SPA)
- **Client Credentials Flow**: For machine-to-machine
- **Implicit Flow**: Deprecated

### OpenID Connect (OIDC)

Identity layer on OAuth 2.0 adding authentication:
- **ID Token**: JWT with user identity
- **UserInfo Endpoint**: API for user profile
- **Standard Scopes**: openid, profile, email

### SAML 2.0

XML-based protocol for enterprise SSO with IdP and SP model.

## Diagrams

| Diagram | Description |
|---------|-------------|
| [2.1-oauth2-flows.drawio](./2.1-oauth2-flows.drawio) | OAuth 2.0 authorization flows |
| [2.2-oidc-and-saml.drawio](./2.2-oidc-and-saml.drawio) | OIDC and SAML comparison |

---

[<< Previous: Core Concepts](../01-core-concepts/README.md) | [Back to Lesson 04 Index](../README.md) | [Next: Microsoft Entra ID >>](../03-microsoft-entra-id/README.md)
