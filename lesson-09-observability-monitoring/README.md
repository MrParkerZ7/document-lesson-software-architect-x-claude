# Lesson 09: Observability & Monitoring 📊

## 📋 Overview

Observability is the ability to understand the internal state of a system by examining its external outputs. For Software Architects, implementing proper observability is crucial for maintaining reliable, performant systems and quickly diagnosing issues in production.

---

## 🎯 Learning Objectives

By the end of this lesson, you will be able to:

- Understand the three pillars of observability (logs, metrics, traces)
- Design logging strategies and implement structured logging
- Set up metrics collection and dashboards
- Implement distributed tracing
- Create effective alerting strategies

---

## 1. 🔭 Three Pillars of Observability

```
┌─────────────────────────────────────────────────────────┐
│             Three Pillars of Observability               │
│                                                         │
│  ┌─────────────┐  ┌─────────────┐  ┌─────────────┐    │
│  │    Logs     │  │   Metrics   │  │   Traces    │    │
│  │             │  │             │  │             │    │
│  │ • Events    │  │ • Counters  │  │ • Request   │    │
│  │ • Errors    │  │ • Gauges    │  │   flow      │    │
│  │ • Debug     │  │ • Histograms│  │ • Latency   │    │
│  │ • Audit     │  │ • Summaries │  │ • Dependencies│  │
│  │             │  │             │  │             │    │
│  │ What        │  │ How much/   │  │ Where/Why   │    │
│  │ happened?   │  │ How many?   │  │ the problem?│    │
│  └─────────────┘  └─────────────┘  └─────────────┘    │
│                                                         │
│  Together they answer: What went wrong, where, and why │
└─────────────────────────────────────────────────────────┘
```

---

## 2. 📝 Logging

### 2.1 Logging Levels

| Level | Usage | Example |
|-------|-------|---------|
| **TRACE** | Very detailed debugging | Method entry/exit |
| **DEBUG** | Diagnostic information | Variable values |
| **INFO** | Normal operations | Request completed |
| **WARN** | Potential issues | Retry attempt |
| **ERROR** | Failures | Exception caught |
| **FATAL** | Critical failures | Application crash |

### 2.2 Structured Logging

```json
// Structured log entry
{
  "timestamp": "2026-01-31T10:30:45.123Z",
  "level": "ERROR",
  "service": "order-service",
  "version": "1.2.3",
  "traceId": "abc123def456",
  "spanId": "789xyz",
  "userId": "user_123",
  "message": "Failed to process order",
  "error": {
    "type": "PaymentException",
    "message": "Card declined",
    "stackTrace": "..."
  },
  "context": {
    "orderId": "order_456",
    "amount": 150.00,
    "currency": "USD"
  }
}
```

### 2.3 Logging Implementation

**Node.js with Winston**:
```javascript
const winston = require('winston');

const logger = winston.createLogger({
  level: process.env.LOG_LEVEL || 'info',
  format: winston.format.combine(
    winston.format.timestamp(),
    winston.format.errors({ stack: true }),
    winston.format.json()
  ),
  defaultMeta: {
    service: 'order-service',
    version: process.env.APP_VERSION
  },
  transports: [
    new winston.transports.Console(),
    new winston.transports.File({ filename: 'error.log', level: 'error' }),
    new winston.transports.File({ filename: 'combined.log' })
  ]
});

// Usage
logger.info('Order created', {
  orderId: order.id,
  userId: user.id,
  amount: order.total
});

logger.error('Payment failed', {
  orderId: order.id,
  error: err.message,
  errorCode: err.code
});
```

**Python with structlog**:
```python
import structlog

structlog.configure(
    processors=[
        structlog.stdlib.filter_by_level,
        structlog.stdlib.add_logger_name,
        structlog.stdlib.add_log_level,
        structlog.processors.TimeStamper(fmt="iso"),
        structlog.processors.JSONRenderer()
    ],
    wrapper_class=structlog.stdlib.BoundLogger,
    context_class=dict,
    logger_factory=structlog.stdlib.LoggerFactory(),
)

logger = structlog.get_logger()

# Add context that persists
logger = logger.bind(service="order-service", version="1.2.3")

# Log with additional context
logger.info("order_created", order_id="123", user_id="456", amount=150.00)
```

### 2.4 Logging Stack (ELK)

```
┌─────────────────────────────────────────────────────────┐
│                    ELK Stack                             │
│                                                         │
│  ┌─────────────┐                                       │
│  │ Application │──┐                                    │
│  └─────────────┘  │                                    │
│  ┌─────────────┐  │    ┌─────────────┐                │
│  │ Application │──┼───▶│  Logstash/  │                │
│  └─────────────┘  │    │  Filebeat   │                │
│  ┌─────────────┐  │    │  (Collect)  │                │
│  │ Application │──┘    └──────┬──────┘                │
│  └─────────────┘              │                        │
│                               ▼                        │
│                     ┌─────────────────┐               │
│                     │ Elasticsearch   │               │
│                     │    (Store)      │               │
│                     └────────┬────────┘               │
│                              │                         │
│                              ▼                         │
│                     ┌─────────────────┐               │
│                     │     Kibana      │               │
│                     │   (Visualize)   │               │
│                     └─────────────────┘               │
└─────────────────────────────────────────────────────────┘
```

### 2.5 Azure Monitor Logs

```kusto
// KQL Query Examples

// Error rate by service
traces
| where timestamp > ago(1h)
| where severityLevel >= 3
| summarize ErrorCount = count() by cloud_RoleName, bin(timestamp, 5m)
| render timechart

// Slow requests
requests
| where timestamp > ago(1h)
| where duration > 1000
| project timestamp, name, duration, resultCode
| order by duration desc
| take 100

// Exception analysis
exceptions
| where timestamp > ago(24h)
| summarize count() by type, outerMessage
| order by count_ desc
```

---

## 3. 📈 Metrics

### 3.1 Metric Types

```
┌─────────────────────────────────────────────────────────┐
│                    Metric Types                          │
│                                                         │
│  Counter: Cumulative value (only increases)            │
│  ──────────────────────────────────────────            │
│  • Total requests                                      │
│  • Total errors                                        │
│  • Bytes processed                                     │
│                                                         │
│  Gauge: Current value (can go up or down)              │
│  ──────────────────────────────────────────            │
│  • Current temperature                                 │
│  • Active connections                                  │
│  • Queue size                                          │
│                                                         │
│  Histogram: Distribution of values                     │
│  ──────────────────────────────────────────            │
│  • Request duration                                    │
│  • Response size                                       │
│  • Provides percentiles (p50, p90, p99)               │
│                                                         │
│  Summary: Similar to histogram, calculated client-side │
│  ──────────────────────────────────────────            │
│  • Pre-calculated quantiles                            │
└─────────────────────────────────────────────────────────┘
```

### 3.2 RED Method (Request-focused)

| Metric | Description | Example |
|--------|-------------|---------|
| **R**ate | Requests per second | 1000 req/s |
| **E**rrors | Failed requests | 10 errors/s |
| **D**uration | Request latency | p99 = 200ms |

### 3.3 USE Method (Resource-focused)

| Metric | Description | Example |
|--------|-------------|---------|
| **U**tilization | % time resource busy | CPU 80% |
| **S**aturation | Work queued | Queue depth: 50 |
| **E**rrors | Error count | 5 disk errors |

### 3.4 Prometheus

**Metrics Definition**:
```python
from prometheus_client import Counter, Histogram, Gauge, start_http_server

# Counter
REQUEST_COUNT = Counter(
    'http_requests_total',
    'Total HTTP requests',
    ['method', 'endpoint', 'status']
)

# Histogram
REQUEST_LATENCY = Histogram(
    'http_request_duration_seconds',
    'HTTP request latency',
    ['method', 'endpoint'],
    buckets=[0.01, 0.05, 0.1, 0.25, 0.5, 1.0, 2.5, 5.0, 10.0]
)

# Gauge
ACTIVE_CONNECTIONS = Gauge(
    'active_connections',
    'Number of active connections'
)

# Usage
@app.route('/api/orders')
def get_orders():
    with REQUEST_LATENCY.labels(method='GET', endpoint='/api/orders').time():
        # Handle request
        result = process_request()
        REQUEST_COUNT.labels(
            method='GET',
            endpoint='/api/orders',
            status='200'
        ).inc()
        return result

# Start metrics server
start_http_server(9090)
```

**PromQL Queries**:
```promql
# Request rate
rate(http_requests_total[5m])

# Error rate percentage
sum(rate(http_requests_total{status=~"5.."}[5m]))
/ sum(rate(http_requests_total[5m])) * 100

# 99th percentile latency
histogram_quantile(0.99,
  sum(rate(http_request_duration_seconds_bucket[5m])) by (le, endpoint)
)

# CPU usage
100 - (avg(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100)

# Memory usage percentage
(node_memory_MemTotal_bytes - node_memory_MemAvailable_bytes)
/ node_memory_MemTotal_bytes * 100
```

### 3.5 Grafana Dashboards

```
┌─────────────────────────────────────────────────────────┐
│                 Grafana Dashboard                        │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │  Request Rate        Error Rate        Latency  │   │
│  │  ┌──────────────┐   ┌──────────────┐  ┌──────┐ │   │
│  │  │    Graph     │   │    Graph     │  │ p99  │ │   │
│  │  │     📈       │   │     📈       │  │ 245ms│ │   │
│  │  └──────────────┘   └──────────────┘  └──────┘ │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │           Resource Utilization                   │   │
│  │  CPU: [████████░░] 80%   Memory: [██████░░░░] 60%│   │
│  │  Disk: [████░░░░░░] 40%  Network: [█████░░░░░] 50%│  │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │           Top Endpoints by Latency               │   │
│  │  /api/search     ████████████████████  450ms    │   │
│  │  /api/orders     ████████████         300ms    │   │
│  │  /api/users      ██████               150ms    │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

---

## 4. 🔍 Distributed Tracing

### 4.1 Tracing Concepts

```
┌─────────────────────────────────────────────────────────┐
│                  Distributed Trace                       │
│                                                         │
│  Trace ID: abc123                                       │
│  ─────────────────────────────────────────────────────  │
│                                                         │
│  ┌──────────────────────────────────────────────────┐  │
│  │ Span A: API Gateway (Root Span)                  │  │
│  │ ID: span_1, Parent: none                         │  │
│  │ ├──────────────────────────────────────────────┤ │  │
│  │ │ Duration: 500ms                               │ │  │
│  │ └──────────────────────────────────────────────┘ │  │
│  │   │                                              │  │
│  │   ├─▶ ┌────────────────────────────┐            │  │
│  │   │   │ Span B: Order Service      │            │  │
│  │   │   │ ID: span_2, Parent: span_1 │            │  │
│  │   │   │ Duration: 300ms            │            │  │
│  │   │   │   │                        │            │  │
│  │   │   │   ├─▶ ┌──────────────────┐ │            │  │
│  │   │   │   │   │ Span C: Database │ │            │  │
│  │   │   │   │   │ Duration: 50ms   │ │            │  │
│  │   │   │   │   └──────────────────┘ │            │  │
│  │   │   │   │                        │            │  │
│  │   │   │   └─▶ ┌──────────────────┐ │            │  │
│  │   │   │       │ Span D: Cache    │ │            │  │
│  │   │   │       │ Duration: 5ms    │ │            │  │
│  │   │   │       └──────────────────┘ │            │  │
│  │   │   └────────────────────────────┘            │  │
│  │   │                                              │  │
│  │   └─▶ ┌────────────────────────────┐            │  │
│  │       │ Span E: Payment Service    │            │  │
│  │       │ Duration: 150ms            │            │  │
│  │       └────────────────────────────┘            │  │
│  └──────────────────────────────────────────────────┘  │
└─────────────────────────────────────────────────────────┘
```

### 4.2 OpenTelemetry

**Setup**:
```javascript
// Node.js OpenTelemetry setup
const { NodeSDK } = require('@opentelemetry/sdk-node');
const { getNodeAutoInstrumentations } = require('@opentelemetry/auto-instrumentations-node');
const { JaegerExporter } = require('@opentelemetry/exporter-jaeger');
const { Resource } = require('@opentelemetry/resources');
const { SemanticResourceAttributes } = require('@opentelemetry/semantic-conventions');

const sdk = new NodeSDK({
  resource: new Resource({
    [SemanticResourceAttributes.SERVICE_NAME]: 'order-service',
    [SemanticResourceAttributes.SERVICE_VERSION]: '1.0.0',
  }),
  traceExporter: new JaegerExporter({
    endpoint: 'http://jaeger:14268/api/traces',
  }),
  instrumentations: [getNodeAutoInstrumentations()],
});

sdk.start();
```

**Manual Instrumentation**:
```javascript
const { trace, SpanStatusCode } = require('@opentelemetry/api');

const tracer = trace.getTracer('order-service');

async function processOrder(orderId) {
  // Create span
  return tracer.startActiveSpan('processOrder', async (span) => {
    try {
      // Add attributes
      span.setAttribute('order.id', orderId);

      // Child span for database
      await tracer.startActiveSpan('db.query', async (dbSpan) => {
        const order = await db.findOrder(orderId);
        dbSpan.setAttribute('db.statement', 'SELECT * FROM orders WHERE id = ?');
        dbSpan.end();
        return order;
      });

      // Add event
      span.addEvent('order_validated');

      span.setStatus({ code: SpanStatusCode.OK });
    } catch (error) {
      span.setStatus({
        code: SpanStatusCode.ERROR,
        message: error.message,
      });
      span.recordException(error);
      throw error;
    } finally {
      span.end();
    }
  });
}
```

### 4.3 Tracing Tools

| Tool | Type | Features |
|------|------|----------|
| **Jaeger** | Open source | Full-featured, CNCF |
| **Zipkin** | Open source | Simple, Twitter-origin |
| **Azure Application Insights** | Managed | Azure integration |
| **AWS X-Ray** | Managed | AWS integration |
| **Datadog APM** | Commercial | Full platform |
| **Honeycomb** | Commercial | High cardinality |

### 4.4 Application Insights

```csharp
// .NET Application Insights
using Microsoft.ApplicationInsights;
using Microsoft.ApplicationInsights.DataContracts;

public class OrderService
{
    private readonly TelemetryClient _telemetry;

    public async Task<Order> ProcessOrder(string orderId)
    {
        using var operation = _telemetry.StartOperation<RequestTelemetry>("ProcessOrder");
        operation.Telemetry.Properties["OrderId"] = orderId;

        try
        {
            // Track dependency
            using (_telemetry.StartOperation<DependencyTelemetry>("Database"))
            {
                var order = await _db.GetOrderAsync(orderId);
            }

            // Track custom event
            _telemetry.TrackEvent("OrderProcessed", new Dictionary<string, string>
            {
                {"OrderId", orderId},
                {"Status", "Success"}
            });

            operation.Telemetry.Success = true;
            return order;
        }
        catch (Exception ex)
        {
            _telemetry.TrackException(ex);
            operation.Telemetry.Success = false;
            throw;
        }
    }
}
```

---

## 5. 🚨 Alerting

### 5.1 Alerting Strategy

```
┌─────────────────────────────────────────────────────────┐
│                 Alerting Pyramid                         │
│                                                         │
│                    /\                                   │
│                   /  \                                  │
│                  / P1 \    Critical (Page)             │
│                 /──────\   • Service down              │
│                /   P2   \  • Data loss risk            │
│               /──────────\                              │
│              /     P3     \ Important (Ticket)         │
│             /──────────────\ • Degraded performance    │
│            /      P4        \ • High error rate        │
│           /──────────────────\                          │
│          /        P5          \ Warning (Log)          │
│         /──────────────────────\ • Approaching limits  │
│        /                        \ • Minor issues       │
│       ────────────────────────────                      │
│                                                         │
│  Rule: Alert on symptoms, not causes                   │
│        User impact > Internal metrics                  │
└─────────────────────────────────────────────────────────┘
```

### 5.2 Alert Rules

**Prometheus Alerting Rules**:
```yaml
groups:
  - name: application
    rules:
      # High error rate
      - alert: HighErrorRate
        expr: |
          sum(rate(http_requests_total{status=~"5.."}[5m]))
          / sum(rate(http_requests_total[5m])) > 0.05
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "High error rate detected"
          description: "Error rate is {{ $value | humanizePercentage }}"

      # High latency
      - alert: HighLatency
        expr: |
          histogram_quantile(0.99,
            sum(rate(http_request_duration_seconds_bucket[5m])) by (le)
          ) > 1
        for: 10m
        labels:
          severity: warning
        annotations:
          summary: "High latency detected"
          description: "p99 latency is {{ $value }}s"

      # Service down
      - alert: ServiceDown
        expr: up == 0
        for: 1m
        labels:
          severity: critical
        annotations:
          summary: "Service {{ $labels.instance }} is down"

  - name: infrastructure
    rules:
      # High CPU
      - alert: HighCPU
        expr: |
          100 - (avg(rate(node_cpu_seconds_total{mode="idle"}[5m])) * 100) > 80
        for: 15m
        labels:
          severity: warning
        annotations:
          summary: "High CPU usage"
          description: "CPU usage is {{ $value }}%"

      # Low disk space
      - alert: LowDiskSpace
        expr: |
          (node_filesystem_avail_bytes / node_filesystem_size_bytes) * 100 < 10
        for: 5m
        labels:
          severity: critical
        annotations:
          summary: "Low disk space"
          description: "Only {{ $value }}% disk space remaining"
```

### 5.3 Alertmanager Configuration

```yaml
# alertmanager.yml
global:
  resolve_timeout: 5m
  slack_api_url: 'https://hooks.slack.com/services/...'

route:
  receiver: 'default'
  group_by: ['alertname', 'service']
  group_wait: 30s
  group_interval: 5m
  repeat_interval: 4h
  routes:
    - match:
        severity: critical
      receiver: 'pagerduty-critical'
    - match:
        severity: warning
      receiver: 'slack-warnings'

receivers:
  - name: 'default'
    slack_configs:
      - channel: '#alerts'

  - name: 'pagerduty-critical'
    pagerduty_configs:
      - service_key: '<service-key>'
        severity: critical

  - name: 'slack-warnings'
    slack_configs:
      - channel: '#alerts-warnings'
        title: '{{ .GroupLabels.alertname }}'
        text: '{{ range .Alerts }}{{ .Annotations.description }}{{ end }}'

inhibit_rules:
  - source_match:
      severity: 'critical'
    target_match:
      severity: 'warning'
    equal: ['alertname', 'service']
```

### 5.4 On-Call Best Practices

| Practice | Description |
|----------|-------------|
| **Runbooks** | Document response procedures |
| **Escalation** | Clear escalation paths |
| **Rotation** | Fair on-call rotation |
| **Post-mortems** | Blameless incident reviews |
| **Alert fatigue** | Tune alerts to reduce noise |

---

## 6. 🏗️ Observability Architecture

```
┌─────────────────────────────────────────────────────────┐
│              Observability Architecture                  │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │                  Applications                    │   │
│  │  ┌───────┐  ┌───────┐  ┌───────┐  ┌───────┐   │   │
│  │  │ App 1 │  │ App 2 │  │ App 3 │  │ App N │   │   │
│  │  └───┬───┘  └───┬───┘  └───┬───┘  └───┬───┘   │   │
│  │      │          │          │          │        │   │
│  │      └──────────┴──────────┴──────────┘        │   │
│  │                      │                          │   │
│  │               OpenTelemetry                     │   │
│  │                 Collector                       │   │
│  └──────────────────────┬──────────────────────────┘   │
│                         │                              │
│           ┌─────────────┼─────────────┐               │
│           ▼             ▼             ▼               │
│     ┌──────────┐  ┌──────────┐  ┌──────────┐        │
│     │  Loki    │  │Prometheus│  │  Tempo   │        │
│     │ (Logs)   │  │(Metrics) │  │ (Traces) │        │
│     └────┬─────┘  └────┬─────┘  └────┬─────┘        │
│          │             │             │               │
│          └─────────────┼─────────────┘               │
│                        ▼                             │
│               ┌─────────────────┐                    │
│               │    Grafana      │                    │
│               │  (Dashboards)   │                    │
│               └────────┬────────┘                    │
│                        │                             │
│                        ▼                             │
│               ┌─────────────────┐                    │
│               │  Alertmanager   │                    │
│               │   (Alerting)    │                    │
│               └─────────────────┘                    │
└─────────────────────────────────────────────────────────┘
```

---

## 🏋️ Practical Exercises

1. **Structured Logging**: Implement structured logging in an application
2. **Prometheus Setup**: Set up Prometheus and create custom metrics
3. **Distributed Tracing**: Implement tracing across multiple services
4. **Dashboard Creation**: Build a Grafana dashboard for a web application
5. **Alert Configuration**: Create meaningful alerts with proper thresholds

---

## 📖 Further Reading

- "Observability Engineering" - Charity Majors, Liz Fong-Jones
- "Site Reliability Engineering" - Google
- "Distributed Systems Observability" - Cindy Sridharan
- Prometheus Documentation
- OpenTelemetry Documentation

---

## 📝 Summary

Observability is essential for operating reliable systems at scale. The three pillars—logs, metrics, and traces—provide complementary views into system behavior. Effective observability enables teams to quickly detect, diagnose, and resolve issues, reducing mean time to recovery (MTTR) and improving overall system reliability. The key is implementing comprehensive instrumentation while avoiding alert fatigue through thoughtful alerting strategies.
