# Lesson 06: Security 🔒

## 📋 Overview

Security is a critical concern for Software Architects. This lesson covers application security, network security, and compliance requirements that architects must understand to design secure systems that protect data and maintain user trust.

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Understand and mitigate OWASP Top 10 vulnerabilities
- Implement secure coding practices
- Design network security architectures
- Manage secrets securely
- Ensure compliance with regulatory requirements
- Implement security testing strategies

---

## 1. 🛡️ Application Security

### 1.1 OWASP Top 10 (2021)

```
┌─────────────────────────────────────────────────────────┐
│                    OWASP Top 10 2021                     │
│                                                         │
│  A01: Broken Access Control          (most critical)   │
│  A02: Cryptographic Failures                           │
│  A03: Injection                                        │
│  A04: Insecure Design                                  │
│  A05: Security Misconfiguration                        │
│  A06: Vulnerable Components                            │
│  A07: Authentication Failures                          │
│  A08: Software & Data Integrity Failures               │
│  A09: Security Logging & Monitoring Failures           │
│  A10: Server-Side Request Forgery (SSRF)               │
│                                                         │
└─────────────────────────────────────────────────────────┘
```

### 1.2 Vulnerability Details & Mitigations

#### A01: Broken Access Control

**Description**: Users acting outside their intended permissions.

**Examples**:
- Accessing other users' data by changing URL parameters
- Privilege escalation
- CORS misconfiguration

**Mitigations**:
```javascript
// BAD: Direct object reference
app.get('/api/users/:id', (req, res) => {
  const user = db.getUser(req.params.id);
  res.json(user);
});

// GOOD: Authorization check
app.get('/api/users/:id', authorize, (req, res) => {
  const userId = req.params.id;

  // Verify user can access this resource
  if (req.user.id !== userId && !req.user.isAdmin) {
    return res.status(403).json({ error: 'Forbidden' });
  }

  const user = db.getUser(userId);
  res.json(user);
});
```

#### A02: Cryptographic Failures

**Description**: Failures related to cryptography leading to data exposure.

**Mitigations**:
| Do | Don't |
|-----|-------|
| Use TLS 1.2+ | Use SSL/TLS 1.0/1.1 |
| Use strong algorithms (AES-256, RSA-2048) | Use MD5, SHA1, DES |
| Use bcrypt/Argon2 for passwords | Store plaintext passwords |
| Encrypt sensitive data at rest | Store sensitive data unencrypted |
| Use secure random generators | Use predictable random |

```python
# Password hashing
import bcrypt

def hash_password(password: str) -> str:
    salt = bcrypt.gensalt(rounds=12)
    return bcrypt.hashpw(password.encode(), salt).decode()

def verify_password(password: str, hashed: str) -> bool:
    return bcrypt.checkpw(password.encode(), hashed.encode())
```

#### A03: Injection

**Description**: Untrusted data sent to interpreter as part of command/query.

**Types**:
- SQL Injection
- NoSQL Injection
- Command Injection
- LDAP Injection
- XPath Injection

**Mitigations**:
```python
# BAD: SQL Injection vulnerable
query = f"SELECT * FROM users WHERE id = {user_id}"

# GOOD: Parameterized query
query = "SELECT * FROM users WHERE id = %s"
cursor.execute(query, (user_id,))

# GOOD: ORM usage
user = User.objects.get(id=user_id)
```

```javascript
// BAD: Command injection
const { exec } = require('child_process');
exec(`ping ${userInput}`);

// GOOD: Use specific libraries, validate input
const { spawn } = require('child_process');
const ping = spawn('ping', ['-c', '4', sanitizedHost]);
```

#### A04: Insecure Design

**Description**: Missing or ineffective security controls in design.

**Mitigations**:
- Threat modeling during design
- Secure design patterns
- Security requirements in user stories
- Security architecture review

```
Threat Modeling (STRIDE):
┌─────────────────────────────────────────────────────────┐
│  S - Spoofing         Can attacker pretend to be       │
│                       someone else?                     │
│  T - Tampering        Can attacker modify data?        │
│  R - Repudiation      Can attacker deny actions?       │
│  I - Information      Can attacker access private      │
│      Disclosure       information?                      │
│  D - Denial of        Can attacker crash/slow the      │
│      Service          system?                          │
│  E - Elevation of     Can attacker gain higher         │
│      Privilege        privileges?                      │
└─────────────────────────────────────────────────────────┘
```

#### A05: Security Misconfiguration

**Description**: Insecure default configurations, missing security hardening.

**Examples**:
- Default credentials
- Unnecessary features enabled
- Verbose error messages
- Missing security headers

**Security Headers**:
```nginx
# nginx.conf
add_header X-Frame-Options "DENY" always;
add_header X-Content-Type-Options "nosniff" always;
add_header X-XSS-Protection "1; mode=block" always;
add_header Referrer-Policy "strict-origin-when-cross-origin" always;
add_header Content-Security-Policy "default-src 'self'; script-src 'self'" always;
add_header Strict-Transport-Security "max-age=31536000; includeSubDomains" always;
add_header Permissions-Policy "geolocation=(), microphone=(), camera=()" always;
```

#### A06: Vulnerable and Outdated Components

**Description**: Using components with known vulnerabilities.

**Mitigations**:
```bash
# npm - Check for vulnerabilities
npm audit
npm audit fix

# Python - Check for vulnerabilities
pip-audit
safety check

# .NET
dotnet list package --vulnerable

# Container scanning
trivy image myapp:latest
```

**Dependency Management**:
```yaml
# GitHub Dependabot configuration
# .github/dependabot.yml
version: 2
updates:
  - package-ecosystem: "npm"
    directory: "/"
    schedule:
      interval: "weekly"
    open-pull-requests-limit: 10

  - package-ecosystem: "docker"
    directory: "/"
    schedule:
      interval: "weekly"
```

#### A07: Identification and Authentication Failures

**Mitigations**:
- Implement MFA
- Use strong password policies
- Limit failed login attempts
- Use secure session management
- Don't expose session IDs in URLs

```javascript
// Session configuration
const session = require('express-session');

app.use(session({
  secret: process.env.SESSION_SECRET,
  name: 'sessionId', // Custom name, not default
  resave: false,
  saveUninitialized: false,
  cookie: {
    secure: true,        // HTTPS only
    httpOnly: true,      // No JavaScript access
    sameSite: 'strict',  // CSRF protection
    maxAge: 3600000      // 1 hour
  }
}));
```

#### A08: Software and Data Integrity Failures

**Description**: Code and infrastructure that doesn't protect against integrity violations.

**Mitigations**:
- Verify digital signatures
- Use integrity checks (SRI)
- Secure CI/CD pipelines
- Code signing

```html
<!-- Subresource Integrity -->
<script
  src="https://cdn.example.com/lib.js"
  integrity="sha384-oqVuAfXRKap7fdgcCY5uykM6+R9GqQ8K/ux..."
  crossorigin="anonymous">
</script>
```

#### A09: Security Logging and Monitoring Failures

**What to Log**:
| Event | Priority |
|-------|----------|
| Authentication (success/failure) | High |
| Authorization failures | High |
| Input validation failures | Medium |
| Security configuration changes | High |
| High-value transactions | High |

```python
import logging
from datetime import datetime

security_logger = logging.getLogger('security')

def log_security_event(event_type, user_id, details, severity='INFO'):
    security_logger.log(
        getattr(logging, severity),
        f"[{event_type}] user={user_id} details={details} " +
        f"ip={request.remote_addr} timestamp={datetime.utcnow().isoformat()}"
    )

# Usage
log_security_event('AUTH_FAILURE', user_id, 'Invalid password', 'WARNING')
```

#### A10: Server-Side Request Forgery (SSRF)

**Description**: Application fetches remote resource without validating user-supplied URL.

**Mitigations**:
```python
from urllib.parse import urlparse
import ipaddress

ALLOWED_HOSTS = ['api.example.com', 'cdn.example.com']

def is_safe_url(url):
    parsed = urlparse(url)

    # Check scheme
    if parsed.scheme not in ['http', 'https']:
        return False

    # Check against allowlist
    if parsed.hostname not in ALLOWED_HOSTS:
        return False

    # Block private IPs
    try:
        ip = ipaddress.ip_address(parsed.hostname)
        if ip.is_private or ip.is_loopback:
            return False
    except ValueError:
        pass  # Not an IP address

    return True
```

---

## 2. 💻 Secure Coding Practices

### 2.1 Input Validation

```
┌─────────────────────────────────────────────────────────┐
│                 Input Validation Strategy                │
│                                                         │
│  1. Validate on Server Side (always)                   │
│  2. Whitelist over Blacklist                           │
│  3. Validate Type, Length, Format, Range               │
│  4. Encode Output (context-aware)                      │
│  5. Reject Invalid Input (don't sanitize)              │
└─────────────────────────────────────────────────────────┘
```

```typescript
// Input validation with Zod
import { z } from 'zod';

const UserSchema = z.object({
  email: z.string().email().max(255),
  password: z.string().min(8).max(128),
  age: z.number().int().min(18).max(120),
  username: z.string().regex(/^[a-zA-Z0-9_]{3,20}$/)
});

// Usage
try {
  const validatedData = UserSchema.parse(userInput);
} catch (error) {
  // Handle validation error
}
```

### 2.2 Output Encoding

```javascript
// Context-aware encoding

// HTML context
function encodeHTML(str) {
  return str.replace(/[&<>"']/g, char => ({
    '&': '&amp;',
    '<': '&lt;',
    '>': '&gt;',
    '"': '&quot;',
    "'": '&#x27;'
  }[char]));
}

// JavaScript context
function encodeJS(str) {
  return JSON.stringify(str);
}

// URL context
function encodeURL(str) {
  return encodeURIComponent(str);
}

// CSS context
function encodeCSS(str) {
  return str.replace(/[^a-zA-Z0-9]/g, char =>
    '\\' + char.charCodeAt(0).toString(16) + ' '
  );
}
```

### 2.3 Error Handling

```python
# BAD: Exposes internal details
@app.errorhandler(Exception)
def handle_error(e):
    return str(e), 500  # Exposes stack trace

# GOOD: Generic error message, log details
@app.errorhandler(Exception)
def handle_error(e):
    error_id = generate_error_id()
    logger.error(f"Error {error_id}: {str(e)}", exc_info=True)
    return {
        "error": "An unexpected error occurred",
        "error_id": error_id  # For support reference
    }, 500
```

---

## 3. 🔐 Secrets Management

### 3.1 Azure Key Vault

```csharp
// Using Azure Key Vault in .NET
using Azure.Identity;
using Azure.Security.KeyVault.Secrets;

var client = new SecretClient(
    new Uri("https://myvault.vault.azure.net/"),
    new DefaultAzureCredential()
);

KeyVaultSecret secret = await client.GetSecretAsync("DatabasePassword");
string password = secret.Value;
```

### 3.2 HashiCorp Vault

```bash
# Store secret
vault kv put secret/database password=mysecret

# Retrieve secret
vault kv get secret/database
```

```go
// Using Vault in Go
import (
    vault "github.com/hashicorp/vault/api"
)

client, _ := vault.NewClient(vault.DefaultConfig())
secret, _ := client.Logical().Read("secret/data/database")
password := secret.Data["data"].(map[string]interface{})["password"]
```

### 3.3 Secret Management Best Practices

```
┌─────────────────────────────────────────────────────────┐
│             Secrets Management Best Practices            │
│                                                         │
│  DO:                                                    │
│  ✓ Use dedicated secrets management tools               │
│  ✓ Rotate secrets regularly                            │
│  ✓ Use short-lived credentials                         │
│  ✓ Audit secret access                                 │
│  ✓ Use managed identities where possible               │
│                                                         │
│  DON'T:                                                 │
│  ✗ Store secrets in code                               │
│  ✗ Store secrets in environment variables (production) │
│  ✗ Log secrets                                         │
│  ✗ Share secrets via email/chat                        │
│  ✗ Use the same secret across environments             │
└─────────────────────────────────────────────────────────┘
```

---

## 4. 🌐 Network Security

### 4.1 Defense in Depth

```
┌─────────────────────────────────────────────────────────┐
│                   Defense in Depth                       │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │              Perimeter Security                  │   │
│  │         DDoS Protection, WAF, Firewall          │   │
│  │  ┌─────────────────────────────────────────┐   │   │
│  │  │           Network Security              │   │   │
│  │  │      NSGs, VNet, Private Endpoints      │   │   │
│  │  │  ┌─────────────────────────────────┐   │   │   │
│  │  │  │      Application Security       │   │   │   │
│  │  │  │    AuthN, AuthZ, Input Valid.   │   │   │   │
│  │  │  │  ┌─────────────────────────┐   │   │   │   │
│  │  │  │  │     Data Security       │   │   │   │   │
│  │  │  │  │  Encryption, Masking    │   │   │   │   │
│  │  │  │  └─────────────────────────┘   │   │   │   │
│  │  │  └─────────────────────────────────┘   │   │   │
│  │  └─────────────────────────────────────────┘   │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### 4.2 Web Application Firewall (WAF)

```
┌──────────────────────────────────────────────────────────┐
│                     WAF Rules                             │
│                                                          │
│  ┌────────────────────────────────────────────────────┐ │
│  │ OWASP Core Rule Set (CRS)                          │ │
│  │ • SQL Injection detection                          │ │
│  │ • XSS detection                                    │ │
│  │ • Command injection detection                      │ │
│  │ • Path traversal detection                         │ │
│  └────────────────────────────────────────────────────┘ │
│                                                          │
│  ┌────────────────────────────────────────────────────┐ │
│  │ Custom Rules                                        │ │
│  │ • Rate limiting                                    │ │
│  │ • Geo-blocking                                     │ │
│  │ • Bot protection                                   │ │
│  │ • IP allowlist/blocklist                           │ │
│  └────────────────────────────────────────────────────┘ │
└──────────────────────────────────────────────────────────┘
```

### 4.3 DDoS Protection

| Layer | Attack Type | Mitigation |
|-------|-------------|------------|
| **L3/L4** | SYN flood, UDP flood | Cloud DDoS protection |
| **L7** | HTTP flood, Slowloris | WAF, rate limiting |
| **Application** | Resource exhaustion | Auto-scaling, caching |

### 4.4 Network Segmentation

```
┌─────────────────────────────────────────────────────────┐
│                  Network Segmentation                    │
│                                                         │
│  ┌─────────────────┐  ┌─────────────────┐             │
│  │   DMZ Subnet    │  │   App Subnet    │             │
│  │  (Public-facing)│  │   (Internal)    │             │
│  │                 │  │                 │             │
│  │  ┌───────────┐  │  │  ┌───────────┐  │             │
│  │  │ WAF / LB  │──│──│─▶│ App Servers│  │             │
│  │  └───────────┘  │  │  └─────┬─────┘  │             │
│  │                 │  │        │        │             │
│  └─────────────────┘  └────────│────────┘             │
│                                │                       │
│                       ┌────────▼────────┐             │
│                       │   Data Subnet   │             │
│                       │   (Isolated)    │             │
│                       │                 │             │
│                       │  ┌───────────┐  │             │
│                       │  │ Databases │  │             │
│                       │  └───────────┘  │             │
│                       └─────────────────┘             │
│                                                        │
│  NSG Rules:                                           │
│  • DMZ → App: HTTPS only (443)                       │
│  • App → Data: DB port only (5432)                   │
│  • Data → Internet: Blocked                          │
└─────────────────────────────────────────────────────────┘
```

### 4.5 TLS/SSL Configuration

```nginx
# Modern TLS configuration
ssl_protocols TLSv1.2 TLSv1.3;
ssl_prefer_server_ciphers on;
ssl_ciphers ECDHE-ECDSA-AES128-GCM-SHA256:ECDHE-RSA-AES128-GCM-SHA256;
ssl_session_cache shared:SSL:10m;
ssl_session_timeout 1d;
ssl_session_tickets off;

# OCSP Stapling
ssl_stapling on;
ssl_stapling_verify on;
resolver 8.8.8.8 8.8.4.4 valid=300s;
```

---

## 5. 🧪 Security Testing

### 5.1 Static Application Security Testing (SAST)

```yaml
# GitHub Actions SAST
- name: Run SAST
  uses: github/codeql-action/analyze@v2
  with:
    languages: javascript, python
```

**SAST Tools**:
| Tool | Languages | Type |
|------|-----------|------|
| **SonarQube** | Multiple | Self-hosted/Cloud |
| **CodeQL** | Multiple | GitHub native |
| **Checkmarx** | Multiple | Enterprise |
| **Semgrep** | Multiple | Open source |
| **Bandit** | Python | Open source |

### 5.2 Dynamic Application Security Testing (DAST)

```yaml
# OWASP ZAP in CI/CD
- name: OWASP ZAP Scan
  uses: zaproxy/action-full-scan@v0.4.0
  with:
    target: 'https://staging.example.com'
    rules_file_name: '.zap/rules.tsv'
```

**DAST Tools**:
| Tool | Type | Best For |
|------|------|----------|
| **OWASP ZAP** | Open source | CI/CD integration |
| **Burp Suite** | Commercial | Manual testing |
| **Nikto** | Open source | Web server scanning |
| **Nuclei** | Open source | Template-based |

### 5.3 Software Composition Analysis (SCA)

```bash
# Snyk CLI
snyk test
snyk monitor

# npm audit
npm audit --production

# OWASP Dependency-Check
dependency-check --project MyApp --scan ./
```

### 5.4 Security Testing in CI/CD

```yaml
# Complete security pipeline
name: Security Pipeline

on: [push, pull_request]

jobs:
  sast:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Semgrep
        uses: returntocorp/semgrep-action@v1

  sca:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Run Snyk
        uses: snyk/actions/node@master
        env:
          SNYK_TOKEN: ${{ secrets.SNYK_TOKEN }}

  secrets-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Detect secrets
        uses: trufflesecurity/trufflehog@main
        with:
          path: ./

  container-scan:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Build image
        run: docker build -t myapp:${{ github.sha }} .
      - name: Run Trivy
        uses: aquasecurity/trivy-action@master
        with:
          image-ref: myapp:${{ github.sha }}
```

---

## 6. 📋 Compliance & Governance

### 6.1 Regulatory Frameworks

| Framework | Region | Focus |
|-----------|--------|-------|
| **GDPR** | EU | Data privacy |
| **HIPAA** | US | Healthcare data |
| **PCI-DSS** | Global | Payment card data |
| **SOC 2** | Global | Service organizations |
| **ISO 27001** | Global | Information security |
| **CCPA** | California | Consumer privacy |

### 6.2 Data Classification

```
┌─────────────────────────────────────────────────────────┐
│                  Data Classification                     │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │ PUBLIC                                           │   │
│  │ Marketing materials, public documentation        │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │ INTERNAL                                         │   │
│  │ Internal policies, non-sensitive business data   │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │ CONFIDENTIAL                                     │   │
│  │ Customer data, financial data, contracts        │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │ RESTRICTED                                       │   │
│  │ PII, PHI, payment data, credentials             │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### 6.3 Security Incident Response

```
┌─────────────────────────────────────────────────────────┐
│              Incident Response Process                   │
│                                                         │
│  1. PREPARATION                                        │
│     • Incident response plan                           │
│     • Contact lists, runbooks                          │
│                                                         │
│  2. IDENTIFICATION                                     │
│     • Detect and analyze                               │
│     • Determine scope and impact                       │
│                                                         │
│  3. CONTAINMENT                                        │
│     • Short-term: Stop the bleeding                    │
│     • Long-term: Prepare for eradication              │
│                                                         │
│  4. ERADICATION                                        │
│     • Remove threat                                    │
│     • Patch vulnerabilities                            │
│                                                         │
│  5. RECOVERY                                           │
│     • Restore systems                                  │
│     • Verify functionality                             │
│                                                         │
│  6. LESSONS LEARNED                                    │
│     • Post-incident review                             │
│     • Update procedures                                │
└─────────────────────────────────────────────────────────┘
```

---

## 🏋️ Practical Exercises

1. **OWASP Exercise**: Identify and fix vulnerabilities in a sample application
2. **Threat Modeling**: Perform threat modeling for an e-commerce application
3. **Security Testing**: Set up a security testing pipeline with SAST and SCA
4. **Secrets Management**: Implement secrets rotation using Azure Key Vault

---

## 📖 Further Reading

- "The Web Application Hacker's Handbook" - Dafydd Stuttard
- OWASP Testing Guide
- NIST Cybersecurity Framework
- "Security Engineering" - Ross Anderson
- CIS Benchmarks

---

## 📝 Summary

Security must be integrated throughout the software development lifecycle. Understanding common vulnerabilities (OWASP Top 10), implementing secure coding practices, managing secrets properly, and designing secure network architectures are essential skills for Software Architects. Regular security testing and compliance with regulatory requirements ensure that systems remain secure as threats evolve.
