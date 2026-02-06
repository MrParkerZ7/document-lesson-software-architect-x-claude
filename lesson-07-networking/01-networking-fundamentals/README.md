# 1. Networking Fundamentals

[Previous: Main Lesson](../README.md) | [Next: Cloud Networking](../02-cloud-networking/README.md)

---

## Overview

Networking knowledge is fundamental for Software Architects designing distributed systems and cloud-native applications. This section covers the core networking concepts that every architect must understand including the OSI model, TCP/IP protocols, DNS, and HTTP.

---

## Learning Objectives

By the end of this section, you will be able to:

- Understand the OSI model and its layers
- Differentiate between TCP and UDP protocols
- Explain DNS resolution process and record types
- Understand HTTP methods, status codes, and protocol versions

---

## 1.1 OSI Model

The OSI (Open Systems Interconnection) model is a conceptual framework with 7 layers:

| Layer | Name | Protocols/Examples |
|-------|------|-------------------|
| 7 | Application | HTTP, HTTPS, DNS, SMTP |
| 6 | Presentation | SSL/TLS, Encryption |
| 5 | Session | Session management |
| 4 | Transport | TCP, UDP |
| 3 | Network | IP, ICMP, Routing |
| 2 | Data Link | Ethernet, MAC addresses |
| 1 | Physical | Cables, signals |

---

## 1.2 TCP/IP

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

---

## 1.3 DNS (Domain Name System)

**DNS Record Types**:

| Type | Purpose | Example |
|------|---------|---------|
| A | IPv4 address | example.com to 93.184.216.34 |
| AAAA | IPv6 address | example.com to 2606:2800:220:1:... |
| CNAME | Canonical name (alias) | www to example.com |
| MX | Mail server | example.com to mail.example.com |
| TXT | Text record | SPF, DKIM, verification |
| NS | Name server | example.com to ns1.example.com |
| SRV | Service location | _sip._tcp.example.com |

---

## 1.4 HTTP/HTTPS

**HTTP Methods**:

| Method | Purpose | Idempotent | Safe |
|--------|---------|------------|------|
| GET | Retrieve resource | Yes | Yes |
| POST | Create resource | No | No |
| PUT | Replace resource | Yes | No |
| PATCH | Partial update | No | No |
| DELETE | Remove resource | Yes | No |

**HTTP Status Codes**:

| Range | Category | Examples |
|-------|----------|----------|
| 1xx | Informational | 101 Switching Protocols |
| 2xx | Success | 200 OK, 201 Created |
| 3xx | Redirection | 301 Moved, 302 Found |
| 4xx | Client Error | 400 Bad Request, 404 Not Found |
| 5xx | Server Error | 500 Internal, 502 Bad Gateway |

---

## Diagrams

| Diagram | Description |
|---------|-------------|
| [1.1-osi-model.drawio](./1.1-osi-model.drawio) | OSI model layers and protocols |
| [1.2-tcp-udp-comparison.drawio](./1.2-tcp-udp-comparison.drawio) | TCP vs UDP protocol comparison |
| [1.3-dns-resolution.drawio](./1.3-dns-resolution.drawio) | DNS resolution flow |
| [1.4-http-overview.drawio](./1.4-http-overview.drawio) | HTTP protocol overview |

---

## Key Takeaways

1. **OSI Model** provides a conceptual framework for understanding network communication across 7 layers
2. **TCP** ensures reliable, ordered delivery while **UDP** prioritizes speed over reliability
3. **DNS** translates human-readable domain names to IP addresses
4. **HTTP/HTTPS** is the foundation of web communication

---

[Previous: Main Lesson](../README.md) | [Next: Cloud Networking](../02-cloud-networking/README.md)
