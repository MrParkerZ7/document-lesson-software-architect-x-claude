# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is an educational curriculum repository for Software Architects. It contains 11 structured lessons (00-10) covering architecture principles, cloud platforms, security, DevOps, and soft skills. This is NOT a software application - it's a knowledge base with markdown documentation and visual diagrams.

## Repository Structure

```
lesson-XX-topic-name/
├── README.md                    # Lesson overview, objectives, sub-lesson links
├── 01-sub-topic/
│   ├── README.md               # Sub-lesson content
│   ├── topic.drawio            # DrawIO diagram source
│   └── topic.png               # PNG export of diagram
├── 02-sub-topic/
└── ...
```

## Lesson Topics

- **00**: Software Architect Overview (role, responsibilities, positioning)
- **01**: Software Design & Architecture (patterns, styles, SOLID, API design)
- **02**: Event-Driven Architecture (message brokers, streaming, async patterns)
- **03**: Cloud Platforms & Services (AWS, Azure, GCP, cloud-native)
- **04**: Identity & Access Management (OAuth2, RBAC, zero trust)
- **05**: DevOps & Platform Engineering (CI/CD, IaC, Kubernetes, GitOps)
- **06**: Security (secure coding, secrets management, compliance)
- **07**: Networking (load balancing, service mesh, API gateway)
- **08**: Databases & Storage (relational, NoSQL, CAP theorem, caching)
- **09**: Observability & Monitoring (logging, metrics, distributed tracing)
- **10**: Soft Skills (communication, leadership, decision-making)

## Working with Diagrams

- All diagrams use DrawIO format (`.drawio` XML files)
- Each diagram has a corresponding PNG export for viewing
- When modifying diagrams, update both the `.drawio` source and regenerate the `.png` export
- Diagrams include visual styling: shadows, curved arrows, flow animations

## Content Conventions

- Sub-lessons are numbered sequentially (01-, 02-, etc.)
- Each README follows a consistent structure: overview, detailed content, practical exercises, further reading
- Lesson READMEs include file structure tables linking to all sub-lessons and diagrams

## Git Workflow

Commits in this repository include Claude as co-author:
```
Co-Authored-By: Claude Opus 4.5 <noreply@anthropic.com>
```
