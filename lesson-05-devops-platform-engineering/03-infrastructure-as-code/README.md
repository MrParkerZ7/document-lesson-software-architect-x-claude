# 3. Infrastructure as Code (IaC)

[<< Previous: CI/CD Tools](../02-cicd-tools/README.md) | [Back to Lesson 05](../README.md) | [Next: Containerization >>](../04-containerization/README.md)

---

## Overview

Infrastructure as Code (IaC) is the practice of managing and provisioning infrastructure through machine-readable configuration files rather than manual processes. This section covers the major IaC tools and their usage patterns.

---

## Learning Objectives

- Understand IaC principles and benefits
- Write Terraform configurations for cloud infrastructure
- Compare IaC tools and their use cases

---

## 3.1 Terraform

Terraform is a cloud-agnostic IaC tool using HCL (HashiCorp Configuration Language). It enables declarative infrastructure definition and supports multiple cloud providers.

**Workflow**:
1. Write - Define infrastructure in .tf files
2. Plan - Preview changes with terraform plan
3. Apply - Create/modify resources with terraform apply
4. Manage - Iterate and maintain infrastructure

---

## 3.2 IaC Tool Comparison

| Feature | Terraform | Pulumi | ARM/Bicep | CloudFormation |
|---------|-----------|--------|-----------|----------------|
| **Language** | HCL | TypeScript, Python, Go | JSON/Bicep | YAML/JSON |
| **Cloud Support** | Multi-cloud | Multi-cloud | Azure only | AWS only |
| **State Management** | Required | Required | Azure-managed | AWS-managed |
| **Learning Curve** | Medium | Low (if you know lang) | Medium | Medium |

---

## Diagrams

| Diagram | Description |
|---------|-------------|
| [3.1-terraform-workflow.drawio](./3.1-terraform-workflow.drawio) | Terraform workflow and state management |
| [3.2-iac-tools-comparison.drawio](./3.2-iac-tools-comparison.drawio) | Comparison of IaC tools |

---

## Key Takeaways

1. IaC enables version control for infrastructure
2. Terraform is the most widely adopted multi-cloud IaC tool
3. State management is critical for IaC operations
4. Choose IaC tools based on cloud provider and team skills

---

[<< Previous: CI/CD Tools](../02-cicd-tools/README.md) | [Back to Lesson 05](../README.md) | [Next: Containerization >>](../04-containerization/README.md)
