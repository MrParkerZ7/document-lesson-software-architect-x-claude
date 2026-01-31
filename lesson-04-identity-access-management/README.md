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

## 1. Core Concepts

### 1.1 Authentication vs Authorization

```
┌─────────────────────────────────────────────────────────┐
│                                                         │
│   Authentication (AuthN)      Authorization (AuthZ)     │
│   "Who are you?"              "What can you do?"        │
│                                                         │
│   ┌─────────────┐             ┌─────────────┐          │
│   │   Username  │             │    Roles    │          │
│   │   Password  │             │ Permissions │          │
│   │     MFA     │             │   Policies  │          │
│   └─────────────┘             └─────────────┘          │
│                                                         │
│   Verifies identity           Controls access           │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 1.2 Identity Terminology

| Term | Description |
|------|-------------|
| **Principal** | Entity that can be authenticated (user, service, device) |
| **Identity** | Set of attributes that describe a principal |
| **Credential** | Evidence used to prove identity (password, certificate) |
| **Claim** | Statement about a principal (name, email, role) |
| **Token** | Portable representation of identity and claims |
| **Identity Provider (IdP)** | Service that authenticates and issues tokens |
| **Service Provider (SP)** | Application that relies on IdP for authentication |

---

## 2. Authentication Protocols

### 2.1 OAuth 2.0

**Purpose**: Authorization framework for delegated access (not authentication).

#### OAuth 2.0 Flows

**Authorization Code Flow** (Web Apps):
```
┌──────┐                              ┌──────────┐                    ┌──────┐
│ User │                              │  Client  │                    │ Auth │
│      │                              │  (App)   │                    │Server│
└──┬───┘                              └────┬─────┘                    └──┬───┘
   │ 1. Click "Login"                      │                             │
   │──────────────────────────────────────▶│                             │
   │                                       │ 2. Redirect to Auth Server  │
   │◀──────────────────────────────────────│─────────────────────────────▶
   │                                       │                             │
   │ 3. Login & Consent                    │                             │
   │─────────────────────────────────────────────────────────────────────▶
   │                                       │                             │
   │ 4. Redirect with Authorization Code   │                             │
   │◀─────────────────────────────────────────────────────────────────────
   │                                       │                             │
   │                                       │ 5. Exchange Code for Token  │
   │                                       │─────────────────────────────▶
   │                                       │                             │
   │                                       │ 6. Access Token + Refresh   │
   │                                       │◀─────────────────────────────
```

**Client Credentials Flow** (Service-to-Service):
```
┌──────────┐                                      ┌──────────┐
│  Client  │                                      │   Auth   │
│ (Service)│                                      │  Server  │
└────┬─────┘                                      └────┬─────┘
     │ 1. Request Token (client_id + client_secret)   │
     │───────────────────────────────────────────────▶│
     │                                                │
     │ 2. Access Token                                │
     │◀───────────────────────────────────────────────│
```

#### OAuth 2.0 Grant Types

| Grant Type | Use Case | Security |
|------------|----------|----------|
| **Authorization Code** | Web apps with backend | Most secure |
| **Authorization Code + PKCE** | Mobile/SPA apps | Secure for public clients |
| **Client Credentials** | Service-to-service | No user context |
| **Device Code** | Input-limited devices | TVs, CLI tools |
| **Refresh Token** | Obtain new access tokens | Long-lived sessions |

### 2.2 OpenID Connect (OIDC)

**Purpose**: Identity layer on top of OAuth 2.0 (authentication).

```
OAuth 2.0 + OpenID Connect
┌─────────────────────────────────────────────────┐
│                OpenID Connect                    │
│  ┌─────────────────────────────────────────┐    │
│  │              OAuth 2.0                   │    │
│  │         (Authorization)                  │    │
│  │                                          │    │
│  │  + ID Token (JWT with identity claims)   │    │
│  │  + UserInfo Endpoint                     │    │
│  │  + Standard Scopes (openid, profile)     │    │
│  └─────────────────────────────────────────┘    │
└─────────────────────────────────────────────────┘
```

**ID Token** (JWT):
```json
{
  "iss": "https://login.microsoftonline.com/tenant-id/v2.0",
  "sub": "user-unique-id",
  "aud": "client-app-id",
  "exp": 1706745600,
  "iat": 1706742000,
  "name": "John Doe",
  "email": "john@example.com",
  "preferred_username": "john@example.com"
}
```

**Standard OIDC Scopes**:
| Scope | Claims Returned |
|-------|-----------------|
| `openid` | `sub` (required) |
| `profile` | `name`, `family_name`, `given_name`, `picture` |
| `email` | `email`, `email_verified` |
| `address` | `address` |
| `phone` | `phone_number`, `phone_number_verified` |

### 2.3 SAML 2.0

**Purpose**: XML-based standard for exchanging authentication data.

```
┌──────┐          ┌──────────┐          ┌─────┐
│ User │          │   SP     │          │ IdP │
│      │          │ (App)    │          │     │
└──┬───┘          └────┬─────┘          └──┬──┘
   │ 1. Access App     │                   │
   │──────────────────▶│                   │
   │                   │ 2. SAML Request   │
   │◀──────────────────│──────────────────▶│
   │                   │                   │
   │ 3. Login at IdP   │                   │
   │───────────────────────────────────────▶
   │                   │                   │
   │ 4. SAML Response (Assertion)          │
   │◀───────────────────────────────────────
   │                   │                   │
   │ 5. POST Assertion │                   │
   │──────────────────▶│                   │
   │                   │                   │
   │ 6. Access Granted │                   │
   │◀──────────────────│                   │
```

### 2.4 Protocol Comparison

| Feature | OAuth 2.0 | OIDC | SAML 2.0 |
|---------|-----------|------|----------|
| **Purpose** | Authorization | Authentication | Authentication |
| **Format** | JSON | JSON/JWT | XML |
| **Token Type** | Access Token | ID Token + Access Token | Assertion |
| **Best For** | API access | Modern web/mobile | Enterprise SSO |
| **Complexity** | Medium | Medium | High |

---

## 3. Microsoft Entra ID (Azure AD)

### 3.1 Overview

Microsoft Entra ID (formerly Azure AD) is Microsoft's cloud-based identity and access management service.

```
┌─────────────────────────────────────────────────────────┐
│                  Microsoft Entra ID                      │
│                                                         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐    │
│  │   Users     │  │   Groups    │  │    Apps     │    │
│  └─────────────┘  └─────────────┘  └─────────────┘    │
│                                                         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐    │
│  │  Devices    │  │   Roles     │  │  Policies   │    │
│  └─────────────┘  └─────────────┘  └─────────────┘    │
│                                                         │
│  Features:                                              │
│  • Single Sign-On (SSO)                                │
│  • Multi-Factor Authentication (MFA)                    │
│  • Conditional Access                                   │
│  • Identity Protection                                  │
│  • Privileged Identity Management (PIM)                │
└─────────────────────────────────────────────────────────┘
```

### 3.2 App Registrations

**Purpose**: Register applications to use Entra ID for authentication.

```
App Registration
┌─────────────────────────────────────────────────────────┐
│                                                         │
│  Application (Client) ID: xxxxxxxx-xxxx-xxxx-xxxx      │
│  Directory (Tenant) ID:   yyyyyyyy-yyyy-yyyy-yyyy      │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │ Authentication                                   │   │
│  │ • Redirect URIs                                  │   │
│  │ • Platform configurations                        │   │
│  │ • Implicit grant settings                        │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │ API Permissions                                  │   │
│  │ • Microsoft Graph                                │   │
│  │ • Custom APIs                                    │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │ Certificates & Secrets                           │   │
│  │ • Client secrets                                 │   │
│  │ • Certificates                                   │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 3.3 Service Principals & Managed Identities

**Service Principal**: Identity for an application in a specific tenant.

**Managed Identity**: Azure-managed identity for Azure resources.

| Type | Description | Use Case |
|------|-------------|----------|
| **System-assigned** | Tied to resource lifecycle | Single resource access |
| **User-assigned** | Independent lifecycle | Shared across resources |

```
┌──────────────┐                    ┌──────────────┐
│   Azure VM   │                    │  Key Vault   │
│              │                    │              │
│  ┌────────┐  │   No credentials   │  ┌────────┐ │
│  │Managed │  │   in code!         │  │Secrets │ │
│  │Identity│──│───────────────────▶│  │        │ │
│  └────────┘  │                    │  └────────┘ │
└──────────────┘                    └──────────────┘
```

### 3.4 Conditional Access

**Purpose**: Enforce access controls based on conditions (signals).

```
┌─────────────────────────────────────────────────────────┐
│                 Conditional Access Policy                │
│                                                         │
│  WHEN (Assignments):                                    │
│  ┌──────────┐ ┌──────────┐ ┌──────────┐               │
│  │  Users/  │ │  Cloud   │ │Conditions│               │
│  │  Groups  │ │   Apps   │ │          │               │
│  └──────────┘ └──────────┘ └──────────┘               │
│                                                         │
│  Conditions:                                            │
│  • Device platform (iOS, Android, Windows)             │
│  • Location (IP ranges, countries)                     │
│  • Client apps (browser, mobile, desktop)              │
│  • Sign-in risk level                                  │
│  • Device state (compliant, hybrid joined)             │
│                                                         │
│  THEN (Access Controls):                                │
│  ┌──────────────────────────────────────────────────┐  │
│  │ □ Block access                                    │  │
│  │ □ Grant access                                    │  │
│  │   ├─ Require MFA                                  │  │
│  │   ├─ Require compliant device                     │  │
│  │   ├─ Require approved client app                  │  │
│  │   └─ Require password change                      │  │
│  │ □ Session controls                                │  │
│  │   ├─ App enforced restrictions                    │  │
│  │   └─ Sign-in frequency                            │  │
│  └──────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

**Example Policies**:

| Policy | Condition | Control |
|--------|-----------|---------|
| MFA for admins | User is admin role | Require MFA |
| Block legacy auth | Client app is "Other" | Block |
| Compliant devices for O365 | App is O365, device not compliant | Block |
| MFA outside office | Location is not trusted | Require MFA |

---

## 4. Access Control Models

### 4.1 Role-Based Access Control (RBAC)

**Concept**: Assign permissions to roles, assign roles to users.

```
┌─────────────────────────────────────────────────────────┐
│                        RBAC                              │
│                                                         │
│  User ──▶ Role ──▶ Permissions                         │
│                                                         │
│  ┌──────────┐   ┌──────────────┐   ┌───────────────┐  │
│  │  Alice   │──▶│    Admin     │──▶│ Read, Write,  │  │
│  └──────────┘   │              │   │ Delete        │  │
│                 └──────────────┘   └───────────────┘  │
│                                                         │
│  ┌──────────┐   ┌──────────────┐   ┌───────────────┐  │
│  │   Bob    │──▶│    Editor    │──▶│ Read, Write   │  │
│  └──────────┘   └──────────────┘   └───────────────┘  │
│                                                         │
│  ┌──────────┐   ┌──────────────┐   ┌───────────────┐  │
│  │ Charlie  │──▶│    Viewer    │──▶│ Read          │  │
│  └──────────┘   └──────────────┘   └───────────────┘  │
└─────────────────────────────────────────────────────────┘
```

**Azure RBAC Components**:
- **Security Principal**: User, group, service principal, managed identity
- **Role Definition**: Collection of permissions
- **Scope**: Resource, resource group, subscription, management group
- **Role Assignment**: Attaches role to principal at scope

### 4.2 Attribute-Based Access Control (ABAC)

**Concept**: Access decisions based on attributes of user, resource, and environment.

```
┌─────────────────────────────────────────────────────────┐
│                        ABAC                              │
│                                                         │
│  ┌───────────────┐  ┌───────────────┐                  │
│  │ User Attrs    │  │ Resource Attrs│                  │
│  │ • Department  │  │ • Sensitivity │                  │
│  │ • Clearance   │  │ • Owner       │                  │
│  │ • Location    │  │ • Type        │                  │
│  └───────┬───────┘  └───────┬───────┘                  │
│          │                  │                          │
│          ▼                  ▼                          │
│  ┌─────────────────────────────────────────────────┐   │
│  │              Policy Engine                       │   │
│  │                                                  │   │
│  │  IF user.department == resource.department      │   │
│  │  AND user.clearance >= resource.sensitivity     │   │
│  │  AND environment.time IN business_hours         │   │
│  │  THEN ALLOW                                     │   │
│  └─────────────────────────────────────────────────┘   │
│          │                                             │
│          ▼                                             │
│      ALLOW / DENY                                      │
└─────────────────────────────────────────────────────────┘
```

### 4.3 RBAC vs ABAC

| Aspect | RBAC | ABAC |
|--------|------|------|
| **Complexity** | Simple | Complex |
| **Flexibility** | Limited | High |
| **Scalability** | Role explosion risk | Scales with attributes |
| **Use Case** | Standard access patterns | Dynamic, context-aware |
| **Maintenance** | Easier | Harder |

---

## 5. Zero Trust Architecture

### 5.1 Principles

```
┌─────────────────────────────────────────────────────────┐
│                    Zero Trust                            │
│             "Never Trust, Always Verify"                 │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │ 1. Verify Explicitly                             │   │
│  │    • Always authenticate and authorize           │   │
│  │    • Use all available data points               │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │ 2. Use Least Privilege Access                    │   │
│  │    • Just-in-time (JIT) access                   │   │
│  │    • Just-enough-access (JEA)                    │   │
│  │    • Risk-based adaptive policies                │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │ 3. Assume Breach                                 │   │
│  │    • Minimize blast radius                       │   │
│  │    • Segment access                              │   │
│  │    • Verify end-to-end encryption               │   │
│  │    • Use analytics for visibility               │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### 5.2 Zero Trust Components

```
                    ┌─────────────────┐
                    │  Policy Engine  │
                    │  (Decision Hub) │
                    └────────┬────────┘
                             │
    ┌────────────────────────┼────────────────────────┐
    │                        │                        │
    ▼                        ▼                        ▼
┌─────────┐            ┌─────────┐            ┌─────────┐
│Identity │            │ Device  │            │ Network │
│  Plane  │            │  Plane  │            │  Plane  │
│         │            │         │            │         │
│• MFA    │            │• Health │            │• Micro- │
│• SSO    │            │• Compli-│            │  segment│
│• Risk   │            │  ance   │            │• Encrypt│
└─────────┘            └─────────┘            └─────────┘
    │                        │                        │
    └────────────────────────┼────────────────────────┘
                             │
                             ▼
┌─────────────────────────────────────────────────────────┐
│                    Data & Apps                           │
│  • Classification  • DLP  • Access Controls             │
└─────────────────────────────────────────────────────────┘
```

---

## 6. B2B and B2C Scenarios

### 6.1 B2B (Business-to-Business)

**Purpose**: Enable external partners to access your resources.

```
┌──────────────────┐          ┌──────────────────┐
│  Partner Tenant  │          │   Your Tenant    │
│                  │          │                  │
│  ┌────────────┐  │  Invite  │  ┌────────────┐ │
│  │ Partner    │  │◀─────────│  │ Guest User │ │
│  │ User       │──│──────────│─▶│ (B2B)      │ │
│  └────────────┘  │  Accept  │  └────────────┘ │
│                  │          │        │        │
└──────────────────┘          │        ▼        │
                              │  ┌────────────┐ │
                              │  │ Your Apps  │ │
                              │  │ & Resources│ │
                              │  └────────────┘ │
                              └──────────────────┘
```

**B2B Features**:
- Cross-tenant collaboration
- Guest user management
- External identity providers
- Access reviews

### 6.2 B2C (Business-to-Consumer)

**Purpose**: Customer identity management for consumer-facing apps.

```
┌─────────────────────────────────────────────────────────┐
│                    Azure AD B2C                          │
│                                                         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐    │
│  │   Local     │  │   Social    │  │ Enterprise  │    │
│  │  Accounts   │  │   Login     │  │    IdPs     │    │
│  │  (Email)    │  │(Google,FB)  │  │   (SAML)    │    │
│  └──────┬──────┘  └──────┬──────┘  └──────┬──────┘    │
│         │                │                │            │
│         └────────────────┼────────────────┘            │
│                          │                             │
│                          ▼                             │
│                   ┌─────────────┐                      │
│                   │ User Flows  │                      │
│                   │  (Sign-up,  │                      │
│                   │  Sign-in,   │                      │
│                   │  Profile)   │                      │
│                   └──────┬──────┘                      │
│                          │                             │
│                          ▼                             │
│                   ┌─────────────┐                      │
│                   │ Your App    │                      │
│                   └─────────────┘                      │
└─────────────────────────────────────────────────────────┘
```

**B2C Features**:
- Custom branding
- Social identity providers
- Custom policies (Identity Experience Framework)
- Multi-factor authentication
- Self-service password reset

---

## 7. Best Practices

### 7.1 Authentication Best Practices

| Practice | Description |
|----------|-------------|
| Use MFA | Require multi-factor authentication |
| Avoid passwords | Use passwordless (FIDO2, Authenticator) |
| Short token lifetimes | Reduce impact of token theft |
| Secure token storage | Never store tokens in localStorage |
| Use HTTPS | Always encrypt in transit |

### 7.2 Authorization Best Practices

| Practice | Description |
|----------|-------------|
| Least privilege | Grant minimum necessary access |
| Regular reviews | Audit access periodically |
| Just-in-time access | Elevate privileges only when needed |
| Separation of duties | Split critical functions |
| Centralize policies | Single source of truth |

### 7.3 Token Security

```
DO:
✓ Use Authorization Code + PKCE for SPAs
✓ Store tokens in memory or secure storage
✓ Validate tokens on every request
✓ Use short-lived access tokens
✓ Implement token refresh

DON'T:
✗ Store tokens in localStorage
✗ Include tokens in URLs
✗ Use implicit flow for new apps
✗ Skip token validation
✗ Use long-lived access tokens
```

---

## Practical Exercises

1. **App Registration**: Register an application in Entra ID and configure authentication
2. **Conditional Access**: Create a policy requiring MFA for admin users
3. **RBAC Implementation**: Design a role hierarchy for a multi-tenant application
4. **B2C Setup**: Configure a B2C tenant with social identity providers

---

## Further Reading

- "OAuth 2.0 in Action" - Justin Richer
- Microsoft Entra ID Documentation
- NIST Zero Trust Architecture (SP 800-207)
- "Identity and Data Security for Web Development" - Jonathan LeBlanc

---

## Summary

Identity and Access Management is a cornerstone of application security. Understanding authentication protocols like OAuth 2.0 and OpenID Connect, implementing proper access control with RBAC or ABAC, and applying Zero Trust principles enables architects to build systems that protect user identities and organizational resources while maintaining usability.
