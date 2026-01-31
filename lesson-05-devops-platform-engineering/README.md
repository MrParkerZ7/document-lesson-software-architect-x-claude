# Lesson 05: DevOps & Platform Engineering

## Overview

DevOps is a set of practices that combines software development (Dev) and IT operations (Ops) to shorten the development lifecycle while delivering features, fixes, and updates frequently. Platform Engineering extends this by providing self-service capabilities to development teams through internal developer platforms.

---

## Learning Objectives

By the end of this lesson, you will be able to:

- Understand DevOps principles and practices
- Design and implement CI/CD pipelines
- Apply Infrastructure as Code (IaC) practices
- Work with containers and orchestration platforms
- Implement GitOps workflows
- Build internal developer platforms

---

## 1. DevOps Fundamentals

### 1.1 DevOps Lifecycle

```
        ┌─────────────────────────────────────────────────┐
        │                  DevOps Loop                     │
        │                                                  │
        │    ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐     │
        │    │ Plan │─▶│ Code │─▶│Build │─▶│ Test │      │
        │    └──────┘  └──────┘  └──────┘  └──────┘      │
        │        ▲                              │          │
        │        │                              ▼          │
        │    ┌──────┐  ┌──────┐  ┌──────┐  ┌──────┐     │
        │    │Monitor◀─│Operate◀─│Deploy◀─│Release│      │
        │    └──────┘  └──────┘  └──────┘  └──────┘      │
        │                                                  │
        │         Continuous Integration (CI)             │
        │         ─────────────────────────▶               │
        │         Continuous Delivery/Deployment (CD)     │
        │         ─────────────────────────────────────▶   │
        └─────────────────────────────────────────────────┘
```

### 1.2 DevOps Principles

| Principle | Description |
|-----------|-------------|
| **Automation** | Automate repetitive tasks |
| **Continuous Integration** | Merge code frequently, test automatically |
| **Continuous Delivery** | Always keep code deployable |
| **Infrastructure as Code** | Manage infrastructure through code |
| **Monitoring & Logging** | Observe system behavior |
| **Communication** | Break down silos between teams |
| **Continuous Improvement** | Learn from failures, iterate |

### 1.3 DevOps Metrics (DORA)

| Metric | Description | Elite Performance |
|--------|-------------|-------------------|
| **Deployment Frequency** | How often you deploy | Multiple times/day |
| **Lead Time for Changes** | Code commit to production | Less than 1 hour |
| **Change Failure Rate** | % of deployments causing failure | 0-15% |
| **Time to Restore** | Time to recover from failure | Less than 1 hour |

---

## 2. CI/CD Tools

### 2.1 Azure DevOps

**Components**:
- **Azure Repos**: Git repositories
- **Azure Pipelines**: CI/CD pipelines
- **Azure Boards**: Work tracking
- **Azure Artifacts**: Package management
- **Azure Test Plans**: Testing tools

**Pipeline Example** (YAML):
```yaml
# azure-pipelines.yml
trigger:
  branches:
    include:
      - main
      - develop

pool:
  vmImage: 'ubuntu-latest'

stages:
  - stage: Build
    jobs:
      - job: BuildJob
        steps:
          - task: NodeTool@0
            inputs:
              versionSpec: '18.x'

          - script: |
              npm ci
              npm run build
              npm test
            displayName: 'Build and Test'

          - task: PublishBuildArtifacts@1
            inputs:
              pathToPublish: 'dist'
              artifactName: 'drop'

  - stage: Deploy
    dependsOn: Build
    condition: and(succeeded(), eq(variables['Build.SourceBranch'], 'refs/heads/main'))
    jobs:
      - deployment: DeployProd
        environment: 'production'
        strategy:
          runOnce:
            deploy:
              steps:
                - script: echo "Deploying to production"
```

### 2.2 GitHub Actions

**Workflow Example**:
```yaml
# .github/workflows/ci-cd.yml
name: CI/CD Pipeline

on:
  push:
    branches: [main, develop]
  pull_request:
    branches: [main]

jobs:
  build:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: '18'
          cache: 'npm'

      - name: Install dependencies
        run: npm ci

      - name: Run tests
        run: npm test

      - name: Build
        run: npm run build

      - name: Upload artifact
        uses: actions/upload-artifact@v4
        with:
          name: dist
          path: dist/

  deploy:
    needs: build
    runs-on: ubuntu-latest
    if: github.ref == 'refs/heads/main'

    steps:
      - name: Download artifact
        uses: actions/download-artifact@v4
        with:
          name: dist

      - name: Deploy to Azure
        uses: azure/webapps-deploy@v2
        with:
          app-name: 'my-app'
          publish-profile: ${{ secrets.AZURE_WEBAPP_PUBLISH_PROFILE }}
          package: .
```

### 2.3 GitLab CI

**Pipeline Example**:
```yaml
# .gitlab-ci.yml
stages:
  - build
  - test
  - deploy

variables:
  NODE_VERSION: "18"

build:
  stage: build
  image: node:${NODE_VERSION}
  script:
    - npm ci
    - npm run build
  artifacts:
    paths:
      - dist/

test:
  stage: test
  image: node:${NODE_VERSION}
  script:
    - npm ci
    - npm test
  coverage: '/Coverage: \d+.\d+%/'

deploy_staging:
  stage: deploy
  script:
    - echo "Deploying to staging"
  environment:
    name: staging
  only:
    - develop

deploy_production:
  stage: deploy
  script:
    - echo "Deploying to production"
  environment:
    name: production
  only:
    - main
  when: manual
```

### 2.4 Jenkins

**Jenkinsfile** (Declarative):
```groovy
pipeline {
    agent any

    environment {
        NODE_VERSION = '18'
    }

    stages {
        stage('Checkout') {
            steps {
                checkout scm
            }
        }

        stage('Build') {
            steps {
                sh 'npm ci'
                sh 'npm run build'
            }
        }

        stage('Test') {
            steps {
                sh 'npm test'
            }
            post {
                always {
                    junit 'test-results/*.xml'
                }
            }
        }

        stage('Deploy') {
            when {
                branch 'main'
            }
            steps {
                sh 'npm run deploy'
            }
        }
    }

    post {
        failure {
            mail to: 'team@example.com',
                 subject: "Pipeline Failed: ${currentBuild.fullDisplayName}",
                 body: "Check console output at ${BUILD_URL}"
        }
    }
}
```

### CI/CD Tool Comparison

| Feature | Azure DevOps | GitHub Actions | GitLab CI | Jenkins |
|---------|--------------|----------------|-----------|---------|
| **Hosting** | Cloud/Server | Cloud | Cloud/Self | Self-hosted |
| **Config** | YAML | YAML | YAML | Groovy |
| **Integration** | Azure | GitHub | GitLab | Any |
| **Marketplace** | Extensions | Actions | Templates | Plugins |
| **Free Tier** | 1800 min/mo | 2000 min/mo | 400 min/mo | Free |

---

## 3. Infrastructure as Code (IaC)

### 3.1 Terraform

**Description**: Cloud-agnostic IaC tool using HCL (HashiCorp Configuration Language).

```hcl
# main.tf
terraform {
  required_providers {
    azurerm = {
      source  = "hashicorp/azurerm"
      version = "~> 3.0"
    }
  }

  backend "azurerm" {
    resource_group_name  = "tfstate-rg"
    storage_account_name = "tfstatestorage"
    container_name       = "tfstate"
    key                  = "prod.terraform.tfstate"
  }
}

provider "azurerm" {
  features {}
}

# Variables
variable "environment" {
  description = "Environment name"
  type        = string
  default     = "prod"
}

variable "location" {
  description = "Azure region"
  type        = string
  default     = "East US"
}

# Resource Group
resource "azurerm_resource_group" "main" {
  name     = "rg-${var.environment}"
  location = var.location

  tags = {
    Environment = var.environment
    ManagedBy   = "Terraform"
  }
}

# App Service Plan
resource "azurerm_service_plan" "main" {
  name                = "asp-${var.environment}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  os_type             = "Linux"
  sku_name            = "P1v2"
}

# App Service
resource "azurerm_linux_web_app" "main" {
  name                = "app-${var.environment}"
  resource_group_name = azurerm_resource_group.main.name
  location            = azurerm_resource_group.main.location
  service_plan_id     = azurerm_service_plan.main.id

  site_config {
    application_stack {
      node_version = "18-lts"
    }
  }
}

# Outputs
output "app_url" {
  value = azurerm_linux_web_app.main.default_hostname
}
```

### 3.2 Pulumi

**Description**: IaC using general-purpose programming languages.

```typescript
// index.ts
import * as pulumi from "@pulumi/pulumi";
import * as azure from "@pulumi/azure-native";

const config = new pulumi.Config();
const environment = config.require("environment");

// Resource Group
const resourceGroup = new azure.resources.ResourceGroup("rg", {
    resourceGroupName: `rg-${environment}`,
    location: "EastUS",
    tags: {
        Environment: environment,
        ManagedBy: "Pulumi"
    }
});

// App Service Plan
const appServicePlan = new azure.web.AppServicePlan("asp", {
    resourceGroupName: resourceGroup.name,
    location: resourceGroup.location,
    kind: "Linux",
    reserved: true,
    sku: {
        name: "P1v2",
        tier: "PremiumV2"
    }
});

// Web App
const webApp = new azure.web.WebApp("app", {
    resourceGroupName: resourceGroup.name,
    location: resourceGroup.location,
    serverFarmId: appServicePlan.id,
    siteConfig: {
        linuxFxVersion: "NODE|18-lts"
    }
});

export const appUrl = pulumi.interpolate`https://${webApp.defaultHostName}`;
```

### 3.3 ARM Templates (Azure)

```json
{
  "$schema": "https://schema.management.azure.com/schemas/2019-04-01/deploymentTemplate.json#",
  "contentVersion": "1.0.0.0",
  "parameters": {
    "environment": {
      "type": "string",
      "defaultValue": "prod"
    }
  },
  "resources": [
    {
      "type": "Microsoft.Web/serverfarms",
      "apiVersion": "2022-03-01",
      "name": "[concat('asp-', parameters('environment'))]",
      "location": "[resourceGroup().location]",
      "sku": {
        "name": "P1v2",
        "tier": "PremiumV2"
      },
      "kind": "linux",
      "properties": {
        "reserved": true
      }
    }
  ]
}
```

### 3.4 CloudFormation (AWS)

```yaml
AWSTemplateFormatVersion: '2010-09-09'
Description: 'Web Application Stack'

Parameters:
  Environment:
    Type: String
    Default: prod

Resources:
  WebAppFunction:
    Type: AWS::Lambda::Function
    Properties:
      FunctionName: !Sub 'app-${Environment}'
      Runtime: nodejs18.x
      Handler: index.handler
      Code:
        S3Bucket: my-deployment-bucket
        S3Key: app.zip
      MemorySize: 256
      Timeout: 30

  ApiGateway:
    Type: AWS::ApiGateway::RestApi
    Properties:
      Name: !Sub 'api-${Environment}'

Outputs:
  ApiEndpoint:
    Value: !Sub 'https://${ApiGateway}.execute-api.${AWS::Region}.amazonaws.com'
```

### IaC Tool Comparison

| Feature | Terraform | Pulumi | ARM/Bicep | CloudFormation |
|---------|-----------|--------|-----------|----------------|
| **Language** | HCL | TypeScript, Python, Go | JSON/Bicep | YAML/JSON |
| **Cloud Support** | Multi-cloud | Multi-cloud | Azure only | AWS only |
| **State Management** | Required | Required | Azure-managed | AWS-managed |
| **Learning Curve** | Medium | Low (if you know lang) | Medium | Medium |

---

## 4. Containerization

### 4.1 Docker

**Dockerfile Best Practices**:
```dockerfile
# Use specific version tags
FROM node:18-alpine AS builder

# Set working directory
WORKDIR /app

# Copy package files first (layer caching)
COPY package*.json ./

# Install dependencies
RUN npm ci --only=production

# Copy source code
COPY . .

# Build application
RUN npm run build

# Production stage
FROM node:18-alpine AS production

WORKDIR /app

# Create non-root user
RUN addgroup -g 1001 -S nodejs && \
    adduser -S nodejs -u 1001

# Copy from builder
COPY --from=builder --chown=nodejs:nodejs /app/dist ./dist
COPY --from=builder --chown=nodejs:nodejs /app/node_modules ./node_modules

# Switch to non-root user
USER nodejs

# Expose port
EXPOSE 3000

# Health check
HEALTHCHECK --interval=30s --timeout=3s --start-period=5s --retries=3 \
  CMD wget --no-verbose --tries=1 --spider http://localhost:3000/health || exit 1

# Start application
CMD ["node", "dist/index.js"]
```

**Docker Compose**:
```yaml
# docker-compose.yml
version: '3.8'

services:
  app:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - "3000:3000"
    environment:
      - NODE_ENV=production
      - DATABASE_URL=postgres://db:5432/app
    depends_on:
      - db
      - redis
    healthcheck:
      test: ["CMD", "wget", "-q", "--spider", "http://localhost:3000/health"]
      interval: 30s
      timeout: 10s
      retries: 3

  db:
    image: postgres:15-alpine
    environment:
      POSTGRES_DB: app
      POSTGRES_USER: user
      POSTGRES_PASSWORD: ${DB_PASSWORD}
    volumes:
      - postgres_data:/var/lib/postgresql/data

  redis:
    image: redis:7-alpine
    volumes:
      - redis_data:/data

volumes:
  postgres_data:
  redis_data:
```

### 4.2 Container Security

```
┌─────────────────────────────────────────────────────────┐
│              Container Security Layers                   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │ Image Security                                   │   │
│  │ • Use minimal base images (Alpine, Distroless)  │   │
│  │ • Scan for vulnerabilities (Trivy, Snyk)        │   │
│  │ • Sign and verify images                        │   │
│  │ • Don't run as root                             │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │ Runtime Security                                 │   │
│  │ • Read-only file systems                        │   │
│  │ • Resource limits (CPU, memory)                 │   │
│  │ • Network policies                              │   │
│  │ • Seccomp profiles                              │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │ Registry Security                                │   │
│  │ • Private registries                            │   │
│  │ • Access control                                │   │
│  │ • Vulnerability scanning                        │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

---

## 5. Container Orchestration

### 5.1 Kubernetes

**Architecture**:
```
┌─────────────────────────────────────────────────────────┐
│                   Kubernetes Cluster                     │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │              Control Plane                       │   │
│  │  ┌─────────┐ ┌─────────┐ ┌─────────┐           │   │
│  │  │API Server│ │Scheduler│ │Controller│           │   │
│  │  └─────────┘ └─────────┘ │ Manager  │           │   │
│  │                          └─────────┘            │   │
│  │  ┌─────────────────────────────────────────┐   │   │
│  │  │                 etcd                     │   │   │
│  │  └─────────────────────────────────────────┘   │   │
│  └─────────────────────────────────────────────────┘   │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │              Worker Nodes                        │   │
│  │  ┌─────────────────┐  ┌─────────────────┐      │   │
│  │  │     Node 1      │  │     Node 2      │      │   │
│  │  │ ┌─────┐ ┌─────┐ │  │ ┌─────┐ ┌─────┐ │      │   │
│  │  │ │ Pod │ │ Pod │ │  │ │ Pod │ │ Pod │ │      │   │
│  │  │ └─────┘ └─────┘ │  │ └─────┘ └─────┘ │      │   │
│  │  │    kubelet      │  │    kubelet      │      │   │
│  │  └─────────────────┘  └─────────────────┘      │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

**Kubernetes Manifests**:
```yaml
# deployment.yaml
apiVersion: apps/v1
kind: Deployment
metadata:
  name: web-app
  labels:
    app: web-app
spec:
  replicas: 3
  selector:
    matchLabels:
      app: web-app
  template:
    metadata:
      labels:
        app: web-app
    spec:
      containers:
        - name: web-app
          image: myregistry/web-app:v1.0.0
          ports:
            - containerPort: 3000
          resources:
            requests:
              memory: "128Mi"
              cpu: "100m"
            limits:
              memory: "256Mi"
              cpu: "500m"
          livenessProbe:
            httpGet:
              path: /health
              port: 3000
            initialDelaySeconds: 10
            periodSeconds: 10
          readinessProbe:
            httpGet:
              path: /ready
              port: 3000
            initialDelaySeconds: 5
            periodSeconds: 5
          env:
            - name: NODE_ENV
              value: "production"
            - name: DB_PASSWORD
              valueFrom:
                secretKeyRef:
                  name: db-secrets
                  key: password
---
# service.yaml
apiVersion: v1
kind: Service
metadata:
  name: web-app
spec:
  selector:
    app: web-app
  ports:
    - port: 80
      targetPort: 3000
  type: ClusterIP
---
# ingress.yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: web-app
  annotations:
    kubernetes.io/ingress.class: nginx
    cert-manager.io/cluster-issuer: letsencrypt-prod
spec:
  tls:
    - hosts:
        - app.example.com
      secretName: app-tls
  rules:
    - host: app.example.com
      http:
        paths:
          - path: /
            pathType: Prefix
            backend:
              service:
                name: web-app
                port:
                  number: 80
```

### 5.2 Managed Kubernetes Services

| Service | Provider | Features |
|---------|----------|----------|
| **AKS** | Azure | Azure integration, Azure CNI, AAD integration |
| **EKS** | AWS | AWS integration, Fargate support |
| **GKE** | GCP | Autopilot mode, best-in-class networking |

### 5.3 Azure Container Apps

**Simpler alternative to Kubernetes**:
```yaml
# container-app.yaml
properties:
  configuration:
    ingress:
      external: true
      targetPort: 3000
      traffic:
        - latestRevision: true
          weight: 100
    secrets:
      - name: db-password
        value: ${DB_PASSWORD}
  template:
    containers:
      - name: web-app
        image: myregistry/web-app:v1.0.0
        resources:
          cpu: 0.5
          memory: 1Gi
        env:
          - name: DB_PASSWORD
            secretRef: db-password
    scale:
      minReplicas: 1
      maxReplicas: 10
      rules:
        - name: http-scaling
          http:
            metadata:
              concurrentRequests: 100
```

---

## 6. GitOps

### 6.1 GitOps Principles

```
┌─────────────────────────────────────────────────────────┐
│                     GitOps Flow                          │
│                                                         │
│  ┌──────────┐    ┌──────────┐    ┌──────────────────┐  │
│  │   Git    │───▶│  GitOps  │───▶│   Kubernetes     │  │
│  │  (Source │    │ Operator │    │    Cluster       │  │
│  │  of Truth)    │(ArgoCD)  │    │                  │  │
│  └──────────┘    └──────────┘    └──────────────────┘  │
│       ▲                                    │           │
│       │                                    │           │
│       └────────────────────────────────────┘           │
│              Drift Detection & Sync                    │
└─────────────────────────────────────────────────────────┘

Principles:
1. Declarative: Describe desired state
2. Versioned: Git as single source of truth
3. Automated: Changes applied automatically
4. Auditable: Complete history in Git
```

### 6.2 ArgoCD

**Application Definition**:
```yaml
# argocd-application.yaml
apiVersion: argoproj.io/v1alpha1
kind: Application
metadata:
  name: web-app
  namespace: argocd
spec:
  project: default
  source:
    repoURL: https://github.com/org/manifests.git
    targetRevision: HEAD
    path: apps/web-app
  destination:
    server: https://kubernetes.default.svc
    namespace: production
  syncPolicy:
    automated:
      prune: true
      selfHeal: true
    syncOptions:
      - CreateNamespace=true
```

### 6.3 Flux

**Flux Configuration**:
```yaml
# flux-system/gotk-sync.yaml
apiVersion: source.toolkit.fluxcd.io/v1
kind: GitRepository
metadata:
  name: flux-system
  namespace: flux-system
spec:
  interval: 1m
  url: https://github.com/org/manifests.git
  ref:
    branch: main
---
apiVersion: kustomize.toolkit.fluxcd.io/v1
kind: Kustomization
metadata:
  name: flux-system
  namespace: flux-system
spec:
  interval: 10m
  path: ./clusters/production
  prune: true
  sourceRef:
    kind: GitRepository
    name: flux-system
```

---

## 7. Artifact Management

### 7.1 Container Registries

| Registry | Provider | Features |
|----------|----------|----------|
| **Azure Container Registry** | Azure | Geo-replication, Tasks |
| **Amazon ECR** | AWS | Lifecycle policies |
| **Google Artifact Registry** | GCP | Multi-format support |
| **Docker Hub** | Docker | Public registry |
| **Harbor** | CNCF | Self-hosted, scanning |

### 7.2 Package Managers

| Type | Tools |
|------|-------|
| **npm/Node** | Azure Artifacts, npm registry, Verdaccio |
| **NuGet/.NET** | Azure Artifacts, NuGet.org |
| **Maven/Java** | Nexus, Artifactory |
| **PyPI/Python** | Azure Artifacts, private PyPI |
| **Helm** | Harbor, ChartMuseum |

---

## 8. Platform Engineering

### 8.1 Internal Developer Platform (IDP)

```
┌─────────────────────────────────────────────────────────┐
│              Internal Developer Platform                 │
│                                                         │
│  ┌─────────────────────────────────────────────────┐   │
│  │             Developer Portal                     │   │
│  │  • Service Catalog  • Documentation             │   │
│  │  • Self-Service     • Templates                 │   │
│  └─────────────────────────────────────────────────┘   │
│                          │                             │
│  ┌───────────────────────┼───────────────────────┐    │
│  │                       ▼                        │    │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────┐       │    │
│  │  │  CI/CD  │  │   IaC   │  │ Secrets │       │    │
│  │  │Pipeline │  │Templates│  │  Mgmt   │       │    │
│  │  └─────────┘  └─────────┘  └─────────┘       │    │
│  │                                               │    │
│  │  ┌─────────┐  ┌─────────┐  ┌─────────┐       │    │
│  │  │ Monitor │  │  Logs   │  │ Alerts  │       │    │
│  │  └─────────┘  └─────────┘  └─────────┘       │    │
│  └───────────────────────────────────────────────┘    │
│                          │                             │
│                          ▼                             │
│  ┌─────────────────────────────────────────────────┐   │
│  │            Infrastructure Layer                  │   │
│  │     Kubernetes  |  Cloud Services  |  Network   │   │
│  └─────────────────────────────────────────────────┘   │
└─────────────────────────────────────────────────────────┘
```

### 8.2 Platform Engineering Tools

| Category | Tools |
|----------|-------|
| **Developer Portal** | Backstage, Port, Cortex |
| **Self-Service** | Crossplane, Kratix |
| **Templates** | Cookiecutter, Yeoman, Backstage |
| **Documentation** | Backstage TechDocs, ReadMe |

---

## Practical Exercises

1. **CI/CD Pipeline**: Create a complete pipeline with build, test, and deploy stages
2. **Terraform Module**: Build a reusable Terraform module for a web application
3. **Kubernetes Deployment**: Deploy a multi-tier application to Kubernetes
4. **GitOps Setup**: Implement GitOps with ArgoCD for a sample application

---

## Further Reading

- "The DevOps Handbook" - Gene Kim, et al.
- "Infrastructure as Code" - Kief Morris
- "Kubernetes in Action" - Marko Lukša
- "Team Topologies" - Matthew Skelton & Manuel Pais
- "Platform Engineering on Kubernetes" - Mauricio Salatino

---

## Summary

DevOps and Platform Engineering are essential disciplines for modern software delivery. By automating CI/CD pipelines, managing infrastructure as code, leveraging containers and orchestration, and implementing GitOps practices, organizations can achieve faster, more reliable software delivery. Platform Engineering extends these capabilities by providing self-service platforms that empower developers while maintaining governance and standards.
