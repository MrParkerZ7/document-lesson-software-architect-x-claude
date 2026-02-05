# Lesson 07: Networking 🌐

## 📋 Overview

Networking knowledge is fundamental for Software Architects designing distributed systems and cloud-native applications. This lesson covers networking fundamentals, cloud networking concepts, and modern networking patterns like service mesh.

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Understand core networking concepts (TCP/IP, DNS, HTTP)
- Design cloud network architectures
- Implement load balancing strategies
- Configure service mesh for microservices
- Design and manage API gateways

---

## 1. 💡 Networking Fundamentals

### 1.1 OSI Model

```
┌─────────────────────────────────────────────────────────┐
│                      OSI Model                           │
│                                                         │
│  Layer 7 │ Application  │ HTTP, HTTPS, DNS, SMTP       │
│  ────────┼──────────────┼─────────────────────────────│
│  Layer 6 │ Presentation │ SSL/TLS, Encryption          │
│  ────────┼──────────────┼─────────────────────────────│
│  Layer 5 │ Session      │ Session management           │
│  ────────┼──────────────┼─────────────────────────────│
│  Layer 4 │ Transport    │ TCP, UDP                     │
│  ────────┼──────────────┼─────────────────────────────│
│  Layer 3 │ Network      │ IP, ICMP, Routing            │
│  ────────┼──────────────┼─────────────────────────────│
│  Layer 2 │ Data Link    │ Ethernet, MAC addresses      │
│  ────────┼──────────────┼─────────────────────────────│
│  Layer 1 │ Physical     │ Cables, signals              │
└─────────────────────────────────────────────────────────┘
```

### 1.2 TCP/IP

**TCP (Transmission Control Protocol)**:
- Connection-oriented
- Reliable delivery
- Ordered packets
- Flow control
- Use cases: HTTP, FTP, SMTP, databases

**UDP (User Datagram Protocol)**:
- Connectionless
- No guaranteed delivery
- No ordering
- Lower latency
- Use cases: DNS, video streaming, gaming, VoIP

**TCP Handshake**:
```
Client                    Server
  │                         │
  │──────── SYN ──────────▶│
  │                         │
  │◀─────── SYN-ACK ───────│
  │                         │
  │──────── ACK ──────────▶│
  │                         │
  │     Connection          │
  │     Established         │
```

### 1.3 DNS (Domain Name System)

```
┌─────────────────────────────────────────────────────────┐
│                    DNS Resolution                        │
│                                                         │
│  Browser ──▶ Local Cache ──▶ OS Cache                  │
│                                   │                     │
│                                   ▼                     │
│                           Recursive Resolver            │
│                                   │                     │
│              ┌────────────────────┼────────────────┐   │
│              ▼                    ▼                ▼   │
│         Root DNS           TLD DNS (.com)    Auth DNS  │
│              │                    │               │    │
│              └────────────────────┴───────────────┘    │
│                                   │                     │
│                                   ▼                     │
│                              IP Address                 │
│                            (93.184.216.34)             │
└─────────────────────────────────────────────────────────┘
```

**DNS Record Types**:
| Type | Purpose | Example |
|------|---------|---------|
| **A** | IPv4 address | example.com → 93.184.216.34 |
| **AAAA** | IPv6 address | example.com → 2606:2800:220:1:... |
| **CNAME** | Canonical name (alias) | www → example.com |
| **MX** | Mail server | example.com → mail.example.com |
| **TXT** | Text record | SPF, DKIM, verification |
| **NS** | Name server | example.com → ns1.example.com |
| **SRV** | Service location | _sip._tcp.example.com |

### 1.4 HTTP/HTTPS

**HTTP Methods**:
| Method | Purpose | Idempotent | Safe |
|--------|---------|------------|------|
| GET | Retrieve resource | Yes | Yes |
| POST | Create resource | No | No |
| PUT | Replace resource | Yes | No |
| PATCH | Partial update | No | No |
| DELETE | Remove resource | Yes | No |
| HEAD | Get headers only | Yes | Yes |
| OPTIONS | Get capabilities | Yes | Yes |

**HTTP Status Codes**:
| Range | Category | Examples |
|-------|----------|----------|
| 1xx | Informational | 101 Switching Protocols |
| 2xx | Success | 200 OK, 201 Created, 204 No Content |
| 3xx | Redirection | 301 Moved, 302 Found, 304 Not Modified |
| 4xx | Client Error | 400 Bad Request, 401/403, 404 Not Found |
| 5xx | Server Error | 500 Internal, 502 Bad Gateway, 503 Unavailable |

**HTTP/2 vs HTTP/1.1**:
| Feature | HTTP/1.1 | HTTP/2 |
|---------|----------|--------|
| Connections | Multiple per domain | Single multiplexed |
| Headers | Text | Binary, compressed |
| Server Push | No | Yes |
| Prioritization | No | Yes |
| Head-of-Line Blocking | Yes | No (at HTTP level) |

**HTTP/3**:
- Based on QUIC (UDP)
- Built-in encryption
- Faster connection establishment
- No head-of-line blocking at transport level

---

## 2. ☁️ Cloud Networking

### 2.1 Virtual Networks

```
┌─────────────────────────────────────────────────────────┐
│                Virtual Network (VNet/VPC)                │
│                   10.0.0.0/16                           │
│                                                         │
│  ┌─────────────────────┐  ┌─────────────────────┐     │
│  │   Web Subnet        │  │   App Subnet        │     │
│  │   10.0.1.0/24       │  │   10.0.2.0/24       │     │
│  │                     │  │                     │     │
│  │  ┌───┐  ┌───┐      │  │  ┌───┐  ┌───┐      │     │
│  │  │VM1│  │VM2│      │  │  │VM3│  │VM4│      │     │
│  │  └───┘  └───┘      │  │  └───┘  └───┘      │     │
│  │                     │  │                     │     │
│  │  NSG: Allow 80,443  │  │  NSG: Allow 8080   │     │
│  └─────────────────────┘  └─────────────────────┘     │
│                                                         │
│  ┌─────────────────────┐                               │
│  │   Data Subnet       │                               │
│  │   10.0.3.0/24       │                               │
│  │                     │                               │
│  │  ┌───────────────┐  │                               │
│  │  │   Database    │  │                               │
│  │  └───────────────┘  │                               │
│  │                     │                               │
│  │  NSG: Allow 5432    │                               │
│  │  from App Subnet    │                               │
│  └─────────────────────┘                               │
└─────────────────────────────────────────────────────────┘
```

### 2.2 Network Security Groups (NSGs)

```hcl
# Terraform - Azure NSG
resource "azurerm_network_security_group" "web" {
  name                = "web-nsg"
  location            = azurerm_resource_group.main.location
  resource_group_name = azurerm_resource_group.main.name

  security_rule {
    name                       = "AllowHTTPS"
    priority                   = 100
    direction                  = "Inbound"
    access                     = "Allow"
    protocol                   = "Tcp"
    source_port_range          = "*"
    destination_port_range     = "443"
    source_address_prefix      = "*"
    destination_address_prefix = "*"
  }

  security_rule {
    name                       = "DenyAllInbound"
    priority                   = 4096
    direction                  = "Inbound"
    access                     = "Deny"
    protocol                   = "*"
    source_port_range          = "*"
    destination_port_range     = "*"
    source_address_prefix      = "*"
    destination_address_prefix = "*"
  }
}
```

### 2.3 VNet Peering

```
┌─────────────────┐         ┌─────────────────┐
│   VNet A        │         │   VNet B        │
│   10.0.0.0/16   │◀───────▶│   10.1.0.0/16   │
│                 │ Peering │                 │
│   Region: East  │         │   Region: West  │
└─────────────────┘         └─────────────────┘

Features:
• Low latency, high bandwidth
• Traffic stays on Microsoft backbone
• Cross-region peering supported
• Cross-subscription peering supported
```

### 2.4 Private Endpoints

```
┌─────────────────────────────────────────────────────────┐
│                    Private Endpoint                      │
│                                                         │
│  ┌─────────────────┐         ┌─────────────────┐       │
│  │   Your VNet     │         │   Azure Service │       │
│  │                 │         │   (Storage/SQL) │       │
│  │  ┌───────────┐  │ Private │                 │       │
│  │  │   VM      │──│─────────│▶ 10.0.1.5      │       │
│  │  └───────────┘  │   Link  │  (Private IP)  │       │
│  │                 │         │                 │       │
│  │  No public     │         │  Public access  │       │
│  │  internet!     │         │  disabled       │       │
│  └─────────────────┘         └─────────────────┘       │
└─────────────────────────────────────────────────────────┘
```

### 2.5 ExpressRoute / Direct Connect

```
┌─────────────────────────────────────────────────────────┐
│              ExpressRoute Architecture                   │
│                                                         │
│  ┌─────────────────┐                                   │
│  │  On-Premises    │                                   │
│  │  Data Center    │                                   │
│  │                 │                                   │
│  │  ┌───────────┐  │                                   │
│  │  │  Router   │──│──┐                                │
│  │  └───────────┘  │  │                                │
│  └─────────────────┘  │                                │
│                       │ Private Connection              │
│                       │ (Not over Internet)            │
│                       ▼                                │
│              ┌─────────────────┐                       │
│              │ ExpressRoute    │                       │
│              │ Circuit         │                       │
│              │ (Partner/Direct)│                       │
│              └────────┬────────┘                       │
│                       │                                │
│                       ▼                                │
│              ┌─────────────────┐                       │
│              │  Azure VNet     │                       │
│              │                 │                       │
│              └─────────────────┘                       │
│                                                         │
│  Benefits:                                             │
│  • Private connectivity (not internet)                 │
│  • Predictable performance                             │
│  • Higher bandwidth (up to 100 Gbps)                  │
│  • Lower latency                                       │
└─────────────────────────────────────────────────────────┘
```

---

## 3. ⚖️ Load Balancing

### 3.1 Load Balancing Types

```
┌─────────────────────────────────────────────────────────┐
│               Load Balancing Comparison                  │
│                                                         │
│  Layer 4 (Transport)          Layer 7 (Application)    │
│  ────────────────────         ─────────────────────    │
│  • TCP/UDP level              • HTTP/HTTPS level       │
│  • Fast, simple               • Content-based routing  │
│  • IP + Port routing          • URL/Header routing     │
│  • No content inspection      • SSL termination        │
│                                                         │
│  Use Cases:                   Use Cases:               │
│  • Database load balancing    • Web applications       │
│  • Non-HTTP protocols         • API gateways          │
│  • Gaming servers             • Microservices         │
└─────────────────────────────────────────────────────────┘
```

### 3.2 Load Balancing Algorithms

| Algorithm | Description | Use Case |
|-----------|-------------|----------|
| **Round Robin** | Sequential distribution | Equal capacity servers |
| **Weighted Round Robin** | Based on server capacity | Mixed capacity servers |
| **Least Connections** | To server with fewest connections | Long-lived connections |
| **IP Hash** | Based on client IP | Session persistence |
| **Least Response Time** | To fastest responding server | Performance-critical |
| **Random** | Random server selection | Simple distribution |

### 3.3 Azure Load Balancing Options

```
┌─────────────────────────────────────────────────────────┐
│             Azure Load Balancing Decision                │
│                                                         │
│                    Global or Regional?                  │
│                    ┌────────┴────────┐                  │
│                    ▼                 ▼                  │
│               Global             Regional               │
│                    │                 │                  │
│           HTTP or Non-HTTP?    HTTP or Non-HTTP?       │
│           ┌────┴────┐          ┌────┴────┐            │
│           ▼         ▼          ▼         ▼            │
│         HTTP     Non-HTTP    HTTP     Non-HTTP        │
│           │         │          │         │            │
│           ▼         ▼          ▼         ▼            │
│      ┌─────────┐ ┌───────┐ ┌───────┐ ┌───────┐       │
│      │ Front   │ │Traffic│ │ App   │ │ Load  │       │
│      │ Door    │ │Manager│ │Gateway│ │Balancer│       │
│      └─────────┘ └───────┘ └───────┘ └───────┘       │
└─────────────────────────────────────────────────────────┘
```

| Service | Layer | Scope | Features |
|---------|-------|-------|----------|
| **Azure Load Balancer** | L4 | Regional | Basic TCP/UDP, fast |
| **Application Gateway** | L7 | Regional | WAF, SSL, URL routing |
| **Front Door** | L7 | Global | CDN, WAF, global routing |
| **Traffic Manager** | DNS | Global | DNS-based routing |

### 3.4 Application Gateway Configuration

```yaml
# ARM Template snippet
{
  "type": "Microsoft.Network/applicationGateways",
  "properties": {
    "sku": {
      "name": "WAF_v2",
      "tier": "WAF_v2"
    },
    "gatewayIPConfigurations": [...],
    "frontendIPConfigurations": [...],
    "frontendPorts": [
      { "name": "https", "port": 443 }
    ],
    "backendAddressPools": [
      {
        "name": "webServers",
        "backendAddresses": [
          { "ipAddress": "10.0.1.4" },
          { "ipAddress": "10.0.1.5" }
        ]
      }
    ],
    "httpListeners": [...],
    "requestRoutingRules": [
      {
        "name": "rule1",
        "ruleType": "PathBasedRouting",
        "urlPathMap": {
          "pathRules": [
            {
              "paths": ["/api/*"],
              "backendAddressPool": "apiServers"
            },
            {
              "paths": ["/*"],
              "backendAddressPool": "webServers"
            }
          ]
        }
      }
    ],
    "webApplicationFirewallConfiguration": {
      "enabled": true,
      "firewallMode": "Prevention",
      "ruleSetType": "OWASP",
      "ruleSetVersion": "3.2"
    }
  }
}
```

---

## 4. 🕸️ Service Mesh

### 4.1 What is a Service Mesh?

```
┌─────────────────────────────────────────────────────────┐
│                    Service Mesh                          │
│                                                         │
│  Without Service Mesh:                                  │
│  ┌─────────┐    Direct    ┌─────────┐                  │
│  │Service A│──────────────│Service B│                  │
│  └─────────┘              └─────────┘                  │
│                                                         │
│  With Service Mesh:                                     │
│  ┌─────────────────┐      ┌─────────────────┐         │
│  │ ┌─────────┐     │      │     ┌─────────┐ │         │
│  │ │Service A│     │      │     │Service B│ │         │
│  │ └────┬────┘     │      │     └────┬────┘ │         │
│  │      │          │      │          │      │         │
│  │ ┌────┴────┐     │      │     ┌────┴────┐ │         │
│  │ │  Proxy  │◀────│──────│────▶│  Proxy  │ │         │
│  │ │ (Envoy) │     │      │     │ (Envoy) │ │         │
│  │ └─────────┘     │      │     └─────────┘ │         │
│  └─────────────────┘      └─────────────────┘         │
│           │                        │                   │
│           └────────────┬───────────┘                   │
│                        │                               │
│               ┌────────┴────────┐                      │
│               │  Control Plane  │                      │
│               │   (Istiod)      │                      │
│               └─────────────────┘                      │
└─────────────────────────────────────────────────────────┘
```

### 4.2 Service Mesh Features

| Feature | Description |
|---------|-------------|
| **Traffic Management** | Load balancing, routing, retries, timeouts |
| **Security** | mTLS, authorization policies |
| **Observability** | Metrics, tracing, logging |
| **Resilience** | Circuit breaking, fault injection |

### 4.3 Istio

**Traffic Management**:
```yaml
# Virtual Service - Routing rules
apiVersion: networking.istio.io/v1beta1
kind: VirtualService
metadata:
  name: reviews
spec:
  hosts:
    - reviews
  http:
    - match:
        - headers:
            end-user:
              exact: jason
      route:
        - destination:
            host: reviews
            subset: v2
    - route:
        - destination:
            host: reviews
            subset: v1
          weight: 90
        - destination:
            host: reviews
            subset: v2
          weight: 10
---
# Destination Rule - Traffic policies
apiVersion: networking.istio.io/v1beta1
kind: DestinationRule
metadata:
  name: reviews
spec:
  host: reviews
  trafficPolicy:
    connectionPool:
      tcp:
        maxConnections: 100
      http:
        http1MaxPendingRequests: 100
    outlierDetection:
      consecutive5xxErrors: 5
      interval: 30s
      baseEjectionTime: 30s
  subsets:
    - name: v1
      labels:
        version: v1
    - name: v2
      labels:
        version: v2
```

**Security - mTLS**:
```yaml
# Peer Authentication - Enable mTLS
apiVersion: security.istio.io/v1beta1
kind: PeerAuthentication
metadata:
  name: default
  namespace: istio-system
spec:
  mtls:
    mode: STRICT
---
# Authorization Policy
apiVersion: security.istio.io/v1beta1
kind: AuthorizationPolicy
metadata:
  name: httpbin
spec:
  selector:
    matchLabels:
      app: httpbin
  rules:
    - from:
        - source:
            principals: ["cluster.local/ns/default/sa/frontend"]
      to:
        - operation:
            methods: ["GET"]
```

### 4.4 Service Mesh Comparison

| Feature | Istio | Linkerd | Consul Connect |
|---------|-------|---------|----------------|
| **Proxy** | Envoy | linkerd2-proxy | Envoy |
| **Complexity** | High | Low | Medium |
| **Performance** | Good | Excellent | Good |
| **Features** | Comprehensive | Core | Core + Service Discovery |
| **Multi-cluster** | Yes | Yes | Yes |

---

## 5. 🚪 API Gateway

### 5.1 API Gateway Functions

```
┌─────────────────────────────────────────────────────────┐
│                    API Gateway                           │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │                   Features                       │   │
│  │                                                  │   │
│  │  • Request Routing                              │   │
│  │  • Authentication / Authorization               │   │
│  │  • Rate Limiting / Throttling                   │   │
│  │  • Request/Response Transformation              │   │
│  │  • Caching                                      │   │
│  │  • Load Balancing                               │   │
│  │  • SSL Termination                              │   │
│  │  • Monitoring / Analytics                       │   │
│  │  • API Versioning                               │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  Client ──▶ API Gateway ──▶ Microservices              │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 5.2 Azure API Management

```xml
<!-- APIM Policy Example -->
<policies>
  <inbound>
    <!-- Rate limiting -->
    <rate-limit calls="100" renewal-period="60" />

    <!-- JWT validation -->
    <validate-jwt header-name="Authorization">
      <openid-config url="https://login.microsoftonline.com/{tenant}/.well-known/openid-configuration" />
      <required-claims>
        <claim name="aud">
          <value>api://myapi</value>
        </claim>
      </required-claims>
    </validate-jwt>

    <!-- Header transformation -->
    <set-header name="X-Request-ID" exists-action="skip">
      <value>@(context.RequestId.ToString())</value>
    </set-header>

    <base />
  </inbound>

  <backend>
    <base />
  </backend>

  <outbound>
    <!-- Response caching -->
    <cache-store duration="3600" />

    <!-- Remove internal headers -->
    <set-header name="X-Powered-By" exists-action="delete" />

    <base />
  </outbound>

  <on-error>
    <base />
  </on-error>
</policies>
```

### 5.3 Kong Gateway

```yaml
# Kong configuration
_format_version: "3.0"

services:
  - name: user-service
    url: http://user-service:8080
    routes:
      - name: user-route
        paths:
          - /api/users
        strip_path: true
    plugins:
      - name: rate-limiting
        config:
          minute: 100
          policy: local

      - name: jwt
        config:
          claims_to_verify:
            - exp

      - name: cors
        config:
          origins:
            - https://example.com
          methods:
            - GET
            - POST
          headers:
            - Authorization
            - Content-Type
```

### 5.4 API Gateway Comparison

| Feature | Azure APIM | AWS API Gateway | Kong |
|---------|------------|-----------------|------|
| **Type** | Managed | Managed | Self-hosted/Cloud |
| **Protocols** | REST, SOAP, WebSocket | REST, WebSocket, HTTP | REST, gRPC, GraphQL |
| **Auth** | OAuth, JWT, Keys | IAM, Cognito, Lambda | OAuth, JWT, Keys, OIDC |
| **Pricing** | Per call + features | Per call + data | Open source / Enterprise |

---

## 🏋️ Practical Exercises

1. **VNet Design**: Design a hub-spoke network topology for a multi-tier application
2. **Load Balancer**: Configure Application Gateway with path-based routing
3. **Service Mesh**: Deploy Istio and configure traffic splitting for canary deployment
4. **API Gateway**: Set up Azure APIM with rate limiting and JWT validation

---

## 📖 Further Reading

- "Computer Networking: A Top-Down Approach" - Kurose & Ross
- "Network Programmability and Automation" - Jason Edelman
- Istio Documentation
- Azure Networking Documentation
- "Zero Trust Networks" - Evan Gilman

---

## 📝 Summary

Networking is the backbone of distributed systems and cloud architectures. Understanding fundamentals like TCP/IP, DNS, and HTTP, combined with cloud networking concepts like VNets, load balancing, and private connectivity, enables architects to design secure, performant, and resilient network architectures. Modern patterns like service mesh and API gateways provide additional capabilities for managing complex microservices environments.
