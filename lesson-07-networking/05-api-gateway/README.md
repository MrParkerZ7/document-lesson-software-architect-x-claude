# 5. API Gateway

[Previous: Service Mesh](../04-service-mesh/README.md) | [Back to Main Lesson](../README.md)

---

## Overview

An API Gateway is a single entry point for all client requests to backend services. It provides essential features like authentication, rate limiting, request transformation, and routing.

---

## Learning Objectives

By the end of this section, you will be able to:

- Understand the role and features of API gateways
- Configure Azure API Management policies
- Set up Kong Gateway with plugins
- Compare different API gateway solutions

---

## 5.1 API Gateway Functions

- Request Routing
- Authentication / Authorization
- Rate Limiting / Throttling
- Request/Response Transformation
- Caching
- Load Balancing
- SSL Termination
- Monitoring / Analytics
- API Versioning

---

## 5.2 Azure API Management

Key policy capabilities:
- Rate limiting and throttling
- JWT validation
- Header transformation
- Response caching
- CORS configuration

---

## 5.3 Kong Gateway

Features:
- Plugin-based architecture
- Support for REST, gRPC, GraphQL
- Rate limiting, JWT, CORS plugins
- Open source and Enterprise options

---

## 5.4 API Gateway Comparison

| Feature | Azure APIM | AWS API Gateway | Kong |
|---------|------------|-----------------|------|
| Type | Managed | Managed | Self-hosted/Cloud |
| Protocols | REST, SOAP, WebSocket | REST, WebSocket, HTTP | REST, gRPC, GraphQL |
| Auth | OAuth, JWT, Keys | IAM, Cognito, Lambda | OAuth, JWT, Keys, OIDC |

---

## Diagrams

| Diagram | Description |
|---------|-------------|
| [5.1-api-gateway.drawio](./5.1-api-gateway.drawio) | API Gateway architecture and features |

---

## Key Takeaways

1. **API Gateway** serves as single entry point for backend services
2. **Core features** include authentication, rate limiting, and caching
3. **Azure APIM** provides powerful policy-based configuration
4. **Kong** offers flexibility with plugin architecture

---

[Previous: Service Mesh](../04-service-mesh/README.md) | [Back to Main Lesson](../README.md)
