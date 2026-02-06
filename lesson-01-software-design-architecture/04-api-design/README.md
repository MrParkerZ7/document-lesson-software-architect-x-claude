# API Design

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Architecture Principles](../03-architecture-principles/README.md) | [Next: Data Architecture](../05-data-architecture/README.md)

---

## 4.1 REST (Representational State Transfer)

**Principles**:
- Stateless communication
- Uniform interface
- Resource-based URLs
- HTTP methods for operations

**HTTP Methods**:
| Method | Purpose | Idempotent |
|--------|---------|------------|
| GET | Retrieve resource | Yes |
| POST | Create resource | No |
| PUT | Update/replace resource | Yes |
| PATCH | Partial update | No |
| DELETE | Remove resource | Yes |

**Best Practices**:
```
GET    /api/users          # List users
GET    /api/users/123      # Get user 123
POST   /api/users          # Create user
PUT    /api/users/123      # Replace user 123
PATCH  /api/users/123      # Update user 123
DELETE /api/users/123      # Delete user 123
```

## 4.2 GraphQL

**Description**: Query language for APIs, allowing clients to request exactly what they need.

```graphql
# Query
query {
  user(id: "123") {
    name
    email
    orders {
      id
      total
    }
  }
}
```

| Pros | Cons |
|------|------|
| Client specifies data needs | Complexity in server implementation |
| Single endpoint | Caching is harder |
| Strong typing | N+1 query problem |
| Introspection | Learning curve |

## 4.3 gRPC

**Description**: High-performance RPC framework using Protocol Buffers.

```protobuf
service UserService {
  rpc GetUser (UserRequest) returns (UserResponse);
  rpc ListUsers (ListRequest) returns (stream User);
}
```

| Pros | Cons |
|------|------|
| High performance (binary) | Less human-readable |
| Bi-directional streaming | Browser support limited |
| Strong typing | Requires HTTP/2 |
| Code generation | Steeper learning curve |

## 4.4 WebSocket

**Description**: Full-duplex communication channel over a single TCP connection.

**Use Cases**:
- Real-time applications
- Chat applications
- Live feeds
- Gaming

---

## Diagrams in This Section

- [4.1-rest-api-flow.drawio](./4.1-rest-api-flow.drawio) | [PNG](./4.1-rest-api-flow.png) | [SVG](./4.1-rest-api-flow.svg)
- [4.2-graphql-flow.drawio](./4.2-graphql-flow.drawio) | [PNG](./4.2-graphql-flow.png) | [SVG](./4.2-graphql-flow.svg)
- [4.3-grpc-communication.drawio](./4.3-grpc-communication.drawio) | [PNG](./4.3-grpc-communication.png) | [SVG](./4.3-grpc-communication.svg)
- [4.4-websocket-flow.drawio](./4.4-websocket-flow.drawio) | [PNG](./4.4-websocket-flow.png) | [SVG](./4.4-websocket-flow.svg)
- [4.5-api-styles-comparison.drawio](./4.5-api-styles-comparison.drawio) | [PNG](./4.5-api-styles-comparison.png) | [SVG](./4.5-api-styles-comparison.svg)
