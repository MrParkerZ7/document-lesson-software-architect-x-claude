# Interview Preparation

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: Building Your Portfolio](../04-building-your-portfolio/README.md) | [Next: Networking & Community](../06-networking-community/README.md)

---

## 5.1 Architecture Interview Format

```
┌─────────────────────────────────────────────────────────────┐
│              Typical Architecture Interview Loop             │
│                                                              │
│  ROUND 1: Recruiter/HR Screen (30 min)                      │
│  • Background and experience overview                       │
│  • Salary expectations                                      │
│  • Role fit assessment                                      │
│                                                              │
│  ROUND 2: Technical Screen (45-60 min)                      │
│  • Technical background deep dive                           │
│  • High-level system design question                        │
│  • Technology-specific questions                            │
│                                                              │
│  ROUND 3: System Design (60-90 min)                         │
│  • Design a complex system from scratch                     │
│  • Scale and optimize existing design                       │
│  • Handle failure scenarios                                 │
│                                                              │
│  ROUND 4: Behavioral (45-60 min)                            │
│  • Past project deep dives (STAR method)                    │
│  • Leadership and conflict resolution                       │
│  • Stakeholder management examples                          │
│                                                              │
│  ROUND 5: Architecture Review (45-60 min)                   │
│  • Critique an existing architecture                        │
│  • Propose improvements                                     │
│  • Discuss trade-offs                                       │
│                                                              │
│  ROUND 6: Executive/Hiring Manager (30-45 min)              │
│  • Culture fit                                              │
│  • Career goals alignment                                   │
│  • Questions about the role                                 │
└─────────────────────────────────────────────────────────────┘
```

---

## 5.2 System Design Framework

### The CLEARS Framework

```
┌─────────────────────────────────────────────────────────────┐
│              CLEARS System Design Framework                  │
│                                                              │
│  C - CLARIFY (5 min)                                        │
│      • What are the functional requirements?                │
│      • What are the non-functional requirements?            │
│      • What's in scope vs out of scope?                     │
│      • What are the constraints?                            │
│                                                              │
│  L - LOAD (5 min)                                           │
│      • How many users? (DAU, MAU)                          │
│      • How many requests per second?                        │
│      • What's the read/write ratio?                        │
│      • How much data storage needed?                        │
│                                                              │
│  E - ENTITIES (5 min)                                       │
│      • What are the core data entities?                     │
│      • What are the relationships?                          │
│      • What's the access pattern?                          │
│                                                              │
│  A - API (5 min)                                            │
│      • What are the main APIs?                              │
│      • RESTful or GraphQL or gRPC?                         │
│      • Authentication approach?                             │
│                                                              │
│  R - REFINE (15 min)                                        │
│      • High-level architecture                              │
│      • Component breakdown                                  │
│      • Data flow                                           │
│      • Database selection                                   │
│                                                              │
│  S - SCALE (10 min)                                         │
│      • Identify bottlenecks                                 │
│      • Add caching                                          │
│      • Add load balancing                                   │
│      • Consider sharding/partitioning                       │
│      • Handle failures                                      │
└─────────────────────────────────────────────────────────────┘
```

---

## 5.3 Common System Design Questions

### Category: Social Networks

| System | Key Challenges |
|--------|----------------|
| **Twitter/X** | Fan-out, timeline generation, trending |
| **Instagram** | Image storage, feed ranking, stories |
| **Facebook Feed** | Ranking algorithm, edge caching |
| **LinkedIn** | Connection degrees, job matching |

### Category: Messaging

| System | Key Challenges |
|--------|----------------|
| **WhatsApp** | E2E encryption, delivery guarantees, presence |
| **Slack** | Channels, search, real-time sync |
| **Notification System** | Multi-channel, rate limiting, prioritization |

### Category: Media & Streaming

| System | Key Challenges |
|--------|----------------|
| **YouTube** | Video encoding, CDN, recommendations |
| **Netflix** | Adaptive streaming, personalization |
| **Spotify** | Audio streaming, offline mode, playlists |

### Category: E-commerce & Payments

| System | Key Challenges |
|--------|----------------|
| **Amazon** | Inventory, recommendations, checkout |
| **Stripe** | Idempotency, PCI compliance, webhooks |
| **Uber** | Matching, surge pricing, ETA |

### Category: Infrastructure

| System | Key Challenges |
|--------|----------------|
| **URL Shortener** | Collision handling, analytics |
| **Rate Limiter** | Distributed counting, fairness |
| **Search Engine** | Indexing, ranking, crawling |
| **Key-Value Store** | Partitioning, replication, consistency |

---

## 5.4 System Design Building Blocks

```
┌─────────────────────────────────────────────────────────────┐
│              Common Architecture Components                  │
│                                                              │
│  COMPUTE                                                    │
│  • Web servers (stateless application tier)                 │
│  • Workers (background job processing)                      │
│  • Serverless functions (event-driven compute)              │
│                                                              │
│  STORAGE                                                    │
│  • Relational DB (ACID, complex queries)                    │
│  • NoSQL DB (scale, flexibility)                            │
│  • Object storage (files, media)                            │
│  • Time-series DB (metrics, logs)                           │
│                                                              │
│  CACHING                                                    │
│  • Application cache (Redis, Memcached)                     │
│  • CDN (static content, edge caching)                       │
│  • Database cache (query results)                           │
│                                                              │
│  MESSAGING                                                  │
│  • Message queues (async processing)                        │
│  • Pub/sub (event distribution)                             │
│  • Stream processing (real-time analytics)                  │
│                                                              │
│  NETWORKING                                                 │
│  • Load balancers (traffic distribution)                    │
│  • API gateways (rate limiting, auth)                       │
│  • Service mesh (service-to-service)                        │
└─────────────────────────────────────────────────────────────┘
```

---

## 5.5 Behavioral Interview Preparation

### STAR Method

```
┌─────────────────────────────────────────────────────────────┐
│                    STAR Method                               │
│                                                              │
│  S - SITUATION                                              │
│      Set the context                                        │
│      "In my previous role at Company X..."                 │
│                                                              │
│  T - TASK                                                   │
│      Describe your responsibility                           │
│      "I was responsible for..."                            │
│                                                              │
│  A - ACTION                                                 │
│      Explain what YOU did (use "I", not "we")              │
│      "I decided to... I implemented... I led..."           │
│                                                              │
│  R - RESULT                                                 │
│      Share the outcome (quantify when possible)             │
│      "As a result, we achieved..."                         │
└─────────────────────────────────────────────────────────────┘
```

### Common Behavioral Questions

| Category | Sample Questions |
|----------|------------------|
| **Technical Leadership** | Tell me about a time you made a significant architectural decision |
| **Conflict Resolution** | Describe a disagreement with another engineer and how you resolved it |
| **Failure** | Tell me about a project that failed and what you learned |
| **Influence** | How did you convince stakeholders to adopt a new technology? |
| **Trade-offs** | Describe a difficult trade-off you made and why |
| **Mentorship** | How have you helped other engineers grow? |

### Preparing Your Stories

Create 5-7 stories that can be adapted for multiple questions:

```
┌─────────────────────────────────────────────────────────────┐
│              Story Preparation Template                      │
│                                                              │
│  Project: _____________________________                     │
│                                                              │
│  Situation: What was the context?                           │
│  ________________________________________________           │
│                                                              │
│  Challenge: What made it difficult?                         │
│  ________________________________________________           │
│                                                              │
│  Actions: What specific things did YOU do?                  │
│  1. ____________________________________________            │
│  2. ____________________________________________            │
│  3. ____________________________________________            │
│                                                              │
│  Results: What were the outcomes? (use numbers)             │
│  ________________________________________________           │
│                                                              │
│  Learnings: What did you learn?                             │
│  ________________________________________________           │
│                                                              │
│  Can be used for these question types:                      │
│  [ ] Leadership  [ ] Conflict  [ ] Failure                 │
│  [ ] Trade-offs  [ ] Influence [ ] Technical depth         │
└─────────────────────────────────────────────────────────────┘
```

---

## 5.6 Questions to Ask Interviewers

### About the Role

- What does success look like in this role in the first 6-12 months?
- What are the biggest architectural challenges the team is facing?
- How much is greenfield vs maintaining existing systems?
- How do architects collaborate with engineering teams?

### About the Team

- What's the team structure and who would I work with closely?
- How are architectural decisions made and documented?
- What's the balance between hands-on coding and strategic work?
- How does the team handle technical debt?

### About the Company

- What's the technology strategy for the next 2-3 years?
- How does engineering leadership view the architecture function?
- What investment is being made in technical enablement?

---

## 5.7 Interview Day Tips

```
┌─────────────────────────────────────────────────────────────┐
│              Interview Day Checklist                         │
│                                                              │
│  BEFORE                                                     │
│  [ ] Research the company thoroughly                        │
│  [ ] Review the job description                             │
│  [ ] Prepare your stories                                   │
│  [ ] Practice system design out loud                        │
│  [ ] Prepare questions to ask                               │
│  [ ] Get a good night's sleep                               │
│                                                              │
│  DURING                                                     │
│  [ ] Listen carefully to questions                          │
│  [ ] Ask clarifying questions                               │
│  [ ] Think out loud during design                           │
│  [ ] Draw diagrams (even for virtual)                       │
│  [ ] Be honest about what you don't know                    │
│  [ ] Show enthusiasm and curiosity                          │
│                                                              │
│  AFTER                                                      │
│  [ ] Send thank you notes                                   │
│  [ ] Reflect on what went well/poorly                       │
│  [ ] Note questions for future preparation                  │
└─────────────────────────────────────────────────────────────┘
```

---

## Diagrams in This Section

- [5.1-interview-framework.drawio](./5.1-interview-framework.drawio)
