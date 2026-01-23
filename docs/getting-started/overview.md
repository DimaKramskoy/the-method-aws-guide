# Overview

## Introduction to The Method

**The Method** is a structured engineering approach to system and project design introduced by Juval Löwy in "Righting Software" (Addison-Wesley, 2019). It provides a systematic way to decompose systems based on volatility rather than functionality.

## Why The Method Matters for Cloud Development

Traditional software engineering principles remain relevant in the cloud era. The Method's focus on volatility-based decomposition and clear component boundaries aligns naturally with AWS serverless services, which are designed around similar principles of separation of concerns and managed infrastructure.

## Core Principles

### 1. Volatility-Based Decomposition

Instead of decomposing systems by function (e.g., "user management", "order processing"), The Method decomposes by **what changes**:

- **Use case volatility** → Managers
- **Business rule volatility** → Engines
- **Data access volatility** → ResourceAccess
- **Resource volatility** → Resources

### 2. Component Taxonomy

The Method defines six component types, each encapsulating a specific type of volatility:

| Component Type     | Encapsulates                  | AWS Mapping            |
| ------------------ | ----------------------------- | ---------------------- |
| **Manager**        | Use case sequences/workflows  | Step Functions, Lambda |
| **Engine**         | Business activities and rules | Lambda                 |
| **ResourceAccess** | Resource access patterns      | Lambda with AWS SDK    |
| **Resource**       | Physical resources            | DynamoDB, S3, SQS      |
| **Utility**        | Cross-cutting concerns        | CloudWatch, X-Ray, SNS |
| **Client**         | User interfaces               | Web/Mobile apps, APIs  |

### 3. Layered Architecture

Systems are organized in layers with clear communication rules:

```
┌─────────────────────────────────────┐
│         Client Layer                │
│  (Web, Mobile, Partner APIs)        │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│    Business Logic Layer             │
│  (Managers orchestrate Engines)     │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│    Resource Access Layer            │
│  (Atomic business operations)       │
└─────────────────────────────────────┘
              ↓
┌─────────────────────────────────────┐
│         Resource Layer              │
│  (Databases, Storage, Queues)       │
└─────────────────────────────────────┘

┌─────────────────────────────────────┐
│         Utilities Bar               │
│  (Logging, Monitoring, Security)    │
│  (Available to all layers)          │
└─────────────────────────────────────┘
```

## Natural Parallels with AWS

The Method's architecture maps naturally to AWS serverless services:

### Managers → Step Functions or Lambda

Managers orchestrate workflows and use cases. AWS Step Functions provide visual workflow orchestration, while Lambda can handle simpler workflows.

### Engines → Lambda Functions

Engines contain business logic and rules. Lambda functions are perfect for encapsulating stateless business logic.

### ResourceAccess → Lambda with AWS SDK

ResourceAccess components expose atomic business operations. Lambda functions with AWS SDK provide the perfect abstraction layer.

### Resources → Managed Services

Resources are actual data stores and queues. AWS provides fully managed services like DynamoDB, S3, and SQS.

### Utilities → Cross-Cutting Services

Utilities provide infrastructure services. AWS offers CloudWatch for logging, X-Ray for tracing, and EventBridge for messaging.

## What You'll Learn

This guideline teaches you how to:

1. **Identify volatility** in business requirements
2. **Map volatility to components** using The Method's taxonomy
3. **Design layered architectures** that minimize coupling
4. **Select appropriate AWS services** for each component type
5. **Implement atomic business verbs** instead of CRUD operations
6. **Avoid common anti-patterns** in cloud architecture

## Next Steps

- **[Installation](installation.md)** - Set up your development environment
- **[Quick Start](quick-start.md)** - Build your first Method-based system on AWS
- **[Core Principles](../guidelines/core-principles.md)** - Deep dive into The Method's principles
