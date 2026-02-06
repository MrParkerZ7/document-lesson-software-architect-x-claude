# Organizational Positioning

> **Navigation**: [Back to Lesson Overview](../README.md) | [Previous: How a Software Architect Works](../01-how-a-software-architect-works/README.md) | [Next: Common Challenges](../03-common-challenges/README.md)

---

## 2.1 Where Architects Fit in the Organization

The placement of architects varies based on organization size and structure:

### Small Organization (Startup/SMB)

```
┌─────────────────────────────────────────────┐
│                    CTO                       │
│                     │                        │
│     ┌───────────────┼───────────────┐       │
│     │               │               │       │
│     ▼               ▼               ▼       │
│ ┌───────┐     ┌───────────┐    ┌───────┐   │
│ │Dev    │     │Software   │    │DevOps │   │
│ │Team   │◄───▶│Architect  │◄──▶│Team   │   │
│ └───────┘     │(1 person) │    └───────┘   │
│               └───────────┘                 │
│                                             │
│  Note: Architect often also codes           │
└─────────────────────────────────────────────┘
```

### Medium Organization

```
┌───────────────────────────────────────────────────────────────┐
│                         CTO                                    │
│                          │                                     │
│          ┌───────────────┼───────────────┐                    │
│          │               │               │                    │
│          ▼               ▼               ▼                    │
│    ┌───────────┐  ┌───────────┐  ┌───────────────┐           │
│    │Engineering│  │ Principal │  │  Platform     │           │
│    │  Manager  │  │ Architect │  │  Engineering  │           │
│    └─────┬─────┘  └─────┬─────┘  └───────┬───────┘           │
│          │              │                │                    │
│          │         ┌────┴────┐           │                    │
│          │         │         │           │                    │
│          ▼         ▼         ▼           ▼                    │
│    ┌─────────┐┌─────────┐┌─────────┐┌─────────┐              │
│    │Dev Team ││Solution ││Solution ││Platform │              │
│    │  Lead   ││Architect││Architect││  Team   │              │
│    └─────────┘│ Team A  ││ Team B  │└─────────┘              │
│               └─────────┘└─────────┘                          │
└───────────────────────────────────────────────────────────────┘
```

### Large Enterprise

```
┌────────────────────────────────────────────────────────────────────┐
│                             CTO                                     │
│                              │                                      │
│              ┌───────────────┼───────────────┐                     │
│              │               │               │                     │
│              ▼               ▼               ▼                     │
│       ┌───────────┐   ┌───────────┐   ┌───────────┐               │
│       │   VP of   │   │  Chief    │   │   VP of   │               │
│       │Engineering│   │ Architect │   │ Platform  │               │
│       └─────┬─────┘   └─────┬─────┘   └─────┬─────┘               │
│             │               │               │                      │
│             │        ┌──────┴──────┐        │                      │
│             │        │             │        │                      │
│             ▼        ▼             ▼        ▼                      │
│       ┌─────────┐┌─────────┐┌─────────┐┌─────────┐                │
│       │ Dir of  ││Enterprise││ Domain  ││Platform │                │
│       │   Eng   ││Architects││Architects│ Arch    │                │
│       └────┬────┘└────┬────┘└────┬────┘└────┬────┘                │
│            │          │          │          │                      │
│            ▼          ▼          ▼          ▼                      │
│       ┌─────────┐┌─────────┐┌─────────┐┌─────────┐                │
│       │ Dev     ││Solution ││Solution ││ Cloud   │                │
│       │ Teams   ││Architects││Architects││ Arch   │                │
│       └─────────┘└─────────┘└─────────┘└─────────┘                │
│                                                                    │
└────────────────────────────────────────────────────────────────────┘
```

## 2.2 Reporting Structures

Common reporting relationships for architects:

| Structure | Description | Pros | Cons |
|-----------|-------------|------|------|
| **Reports to CTO** | Direct line to technical leadership | Strong influence, clear authority | May be disconnected from teams |
| **Reports to Engineering Manager** | Part of engineering organization | Close to development | May lack strategic influence |
| **Separate Architecture Team** | Dedicated architecture function | Consistency across projects | Risk of ivory tower syndrome |
| **Embedded in Product Teams** | Architect per product/domain | Deep domain knowledge | Potential inconsistency |
| **Matrix Structure** | Reports to both technical and business | Balanced perspective | Conflicting priorities |

## 2.3 Types of Architect Roles

Different architect roles have different scopes and focuses:

```
┌────────────────────────────────────────────────────────────────────┐
│                    Architect Role Hierarchy                         │
├────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  Scope                                                              │
│    ▲                                                                │
│    │    ┌─────────────────────────────────────────────┐            │
│    │    │         Chief/Principal Architect           │            │
│    │    │    (Organization-wide technical vision)     │            │
│    │    └─────────────────────────────────────────────┘            │
│    │                         │                                      │
│    │    ┌────────────────────┼────────────────────┐                │
│    │    │                    │                    │                │
│    │    ▼                    ▼                    ▼                │
│    │ ┌─────────┐      ┌───────────┐      ┌────────────┐           │
│    │ │Enterprise│      │  Domain   │      │  Platform  │           │
│    │ │Architect │      │ Architect │      │  Architect │           │
│    │ │(Strategy)│      │(Business  │      │(Infra/     │           │
│    │ │          │      │ Domain)   │      │ Cloud)     │           │
│    │ └─────────┘      └───────────┘      └────────────┘           │
│    │        │                │                  │                   │
│    │        └────────────────┼──────────────────┘                   │
│    │                         │                                      │
│    │                         ▼                                      │
│    │              ┌───────────────────┐                            │
│    │              │ Solution Architect │                            │
│    │              │ (Project/Product   │                            │
│    │              │  specific)         │                            │
│    │              └───────────────────┘                            │
│    │                         │                                      │
│    │                         ▼                                      │
│    │              ┌───────────────────┐                            │
│    │              │Technical Lead/     │                            │
│    │              │Application Arch    │                            │
│    │              │(Team level)        │                            │
│    │              └───────────────────┘                            │
│    │                                                                │
│    └────────────────────────────────────────────────────▶ Detail   │
│                                                                     │
└────────────────────────────────────────────────────────────────────┘
```

### Role Comparison

| Role | Scope | Focus | Typical Deliverables |
|------|-------|-------|---------------------|
| **Chief/Principal Architect** | Organization | Technical vision, standards, governance | Architecture principles, technology radar |
| **Enterprise Architect** | Enterprise | Business-IT alignment, portfolio | Capability maps, roadmaps |
| **Domain Architect** | Business domain | Domain-specific solutions | Domain models, integration patterns |
| **Platform Architect** | Infrastructure | Cloud, infrastructure, DevOps | Platform architecture, IaC standards |
| **Solution Architect** | Project/Product | End-to-end solution design | Solution architecture, technical specs |
| **Application Architect** | Application | Application design, frameworks | Application architecture, coding standards |

## 2.4 Architect's Relationship with Other Roles

```
┌────────────────────────────────────────────────────────────────────┐
│              Architect's Relationship Map                           │
├────────────────────────────────────────────────────────────────────┤
│                                                                     │
│                        ┌───────────────┐                           │
│                        │   Executives  │                           │
│                        │  (Strategy)   │                           │
│                        └───────┬───────┘                           │
│                                │                                    │
│                         Align & Report                              │
│                                │                                    │
│    ┌───────────────┐   ┌──────┴──────┐   ┌───────────────┐        │
│    │   Product     │   │             │   │    Project    │        │
│    │   Management  │◄─▶│  ARCHITECT  │◄─▶│    Manager    │        │
│    │(Requirements) │   │             │   │  (Planning)   │        │
│    └───────────────┘   └──────┬──────┘   └───────────────┘        │
│                               │                                     │
│          ┌────────────────────┼────────────────────┐               │
│          │                    │                    │               │
│          ▼                    ▼                    ▼               │
│   ┌─────────────┐     ┌─────────────┐     ┌─────────────┐         │
│   │ Development │     │   DevOps/   │     │   QA/Test   │         │
│   │    Team     │     │   Platform  │     │    Team     │         │
│   │ (Implement) │     │  (Deploy)   │     │ (Validate)  │         │
│   └─────────────┘     └─────────────┘     └─────────────┘         │
│                                                                     │
│   Legend:                                                           │
│   ◄─▶ Bidirectional collaboration                                  │
│   ─▶  Direction/Guidance                                           │
│                                                                     │
└────────────────────────────────────────────────────────────────────┘
```

### Interaction Patterns

| Role | Architect's Role in Interaction |
|------|--------------------------------|
| **Developers** | Guide, mentor, review, unblock |
| **Tech Lead** | Collaborate on design, delegate implementation details |
| **Product Manager** | Translate requirements, advise on feasibility |
| **Project Manager** | Provide estimates, identify risks, track technical progress |
| **DevOps** | Define infrastructure requirements, deployment strategy |
| **Security** | Collaborate on security architecture, threat modeling |
| **Other Architects** | Coordinate across domains, ensure consistency |

## 2.5 Authority and Influence

Architects typically operate through influence rather than direct authority:

```
┌────────────────────────────────────────────────────────────────────┐
│                  Architect's Authority Model                        │
├────────────────────────────────────────────────────────────────────┤
│                                                                     │
│  DIRECT AUTHORITY                      INFLUENCE-BASED             │
│  (Rare)                                (Common)                     │
│                                                                     │
│  ┌─────────────────┐                  ┌─────────────────┐          │
│  │ Technical       │                  │ Persuasion &    │          │
│  │ Standards       │                  │ Education       │          │
│  └─────────────────┘                  └─────────────────┘          │
│                                                                     │
│  ┌─────────────────┐                  ┌─────────────────┐          │
│  │ Architecture    │                  │ Building Trust  │          │
│  │ Approval Gates  │                  │ & Credibility   │          │
│  └─────────────────┘                  └─────────────────┘          │
│                                                                     │
│  ┌─────────────────┐                  ┌─────────────────┐          │
│  │ Technology      │                  │ Leading by      │          │
│  │ Selection       │                  │ Example         │          │
│  └─────────────────┘                  └─────────────────┘          │
│                                                                     │
│  ┌─────────────────┐                  ┌─────────────────┐          │
│  │ Code/Design     │                  │ Mentoring &     │          │
│  │ Review Veto     │                  │ Collaboration   │          │
│  └─────────────────┘                  └─────────────────┘          │
│                                                                     │
└────────────────────────────────────────────────────────────────────┘
```

**Key Success Factors**:
- Build credibility through technical excellence
- Communicate decisions clearly with rationale
- Be open to feedback and alternative viewpoints
- Maintain hands-on involvement when appropriate
- Develop relationships across the organization

---

## Diagrams in This Section

- [2.1-org-structure-small.drawio](./2.1-org-structure-small.drawio) | [PNG](./2.1-org-structure-small.png) | [SVG](./2.1-org-structure-small.svg)
- [2.2-org-structure-medium.drawio](./2.2-org-structure-medium.drawio) | [PNG](./2.2-org-structure-medium.png) | [SVG](./2.2-org-structure-medium.svg)
- [2.3-org-structure-large.drawio](./2.3-org-structure-large.drawio) | [PNG](./2.3-org-structure-large.png) | [SVG](./2.3-org-structure-large.svg)
- [2.4-architect-role-hierarchy.drawio](./2.4-architect-role-hierarchy.drawio) | [PNG](./2.4-architect-role-hierarchy.png) | [SVG](./2.4-architect-role-hierarchy.svg)
- [2.5-relationship-map.drawio](./2.5-relationship-map.drawio) | [PNG](./2.5-relationship-map.png) | [SVG](./2.5-relationship-map.svg)
- [2.6-authority-vs-influence.drawio](./2.6-authority-vs-influence.drawio) | [PNG](./2.6-authority-vs-influence.png) | [SVG](./2.6-authority-vs-influence.svg)
