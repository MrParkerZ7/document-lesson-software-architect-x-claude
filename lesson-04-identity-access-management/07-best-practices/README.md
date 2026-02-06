# 07 - Best Practices

[<< Previous: B2B and B2C Scenarios](../06-b2b-b2c-scenarios/README.md) | [Back to Lesson 04 Index](../README.md)

---

## Overview

This section consolidates Identity and Access Management best practices.

## Learning Objectives

- Apply IAM security best practices
- Implement secure authentication patterns
- Design for compliance and auditability

## Best Practices

### Authentication

- Always require MFA, especially for privileged accounts
- Implement passwordless (FIDO2, Windows Hello)
- Block legacy authentication protocols
- Implement SSO

### Authorization

- Apply least privilege principle
- Use just-in-time access (PIM)
- Regular access reviews
- Assign permissions to groups, not users

### Token Management

- Use short token lifetimes
- Secure token storage (avoid localStorage for SPAs)
- Implement proper token refresh
- Validate tokens completely

### Application Security

- Use managed identities
- Store secrets in Key Vault
- Validate redirect URIs
- Always use PKCE for public clients

### Operations

- Enable comprehensive logging
- Monitor for anomalies
- Automate provisioning with SCIM
- Include IAM in security testing

## Diagrams

| Diagram | Description |
|---------|-------------|
| [7.1-iam-best-practices.drawio](./7.1-iam-best-practices.drawio) | IAM best practices overview |

---

[<< Previous: B2B and B2C Scenarios](../06-b2b-b2c-scenarios/README.md) | [Back to Lesson 04 Index](../README.md)
