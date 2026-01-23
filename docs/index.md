# The Method on AWS

Welcome to the comprehensive guideline for applying **The Method** from Juval Löwy's "Righting Software" to AWS serverless services.

## What is This?

This guideline demonstrates how to build cloud-native systems using proven architectural principles from "Righting Software" with AWS serverless services. Through detailed system examples, you'll learn how volatility-based decomposition, component taxonomy, and layered architecture naturally align with modern cloud development.

## Attribution

This project is based on **"Righting Software"** by Juval Löwy (Addison-Wesley, 2019). All core architectural principles, component taxonomy, and design concepts are attributed to Juval Löwy's work. Our contribution is demonstrating how these principles apply to AWS cloud services.

**Reference**: Löwy, Juval. _Righting Software_. Boston: Addison-Wesley Professional, 2019.

## Quick Navigation

- **[Getting Started](getting-started/overview.md)** - Learn about The Method and AWS parallels
- **[Guidelines](guidelines/introduction.md)** - Core principles and component taxonomy
- **[Examples](examples/task-management.md)** - Complete system examples from different domains
- **[Reference](reference/aws-mappings.md)** - AWS service mappings and best practices

## Key Concepts

### Volatility-Based Decomposition

Systems are decomposed based on areas of change (volatility) rather than functional decomposition. Changes are encapsulated within components, minimizing system-wide impact.

### Component Taxonomy

The Method defines specific component types:

- **Managers**: Encapsulate volatility in use case sequences/workflows
- **Engines**: Encapsulate volatility in business activities and rules
- **ResourceAccess**: Encapsulate volatility in accessing resources
- **Resources**: Actual physical resources (databases, queues, storage)
- **Utilities**: Common infrastructure services
- **Clients**: User-facing applications and interfaces

### AWS Serverless Parallels

The Method's architecture naturally aligns with AWS serverless services:

- **Managers** → Step Functions or Lambda
- **Engines** → Lambda functions
- **ResourceAccess** → Lambda with AWS SDK
- **Resources** → DynamoDB, S3, SQS, etc.
- **Utilities** → CloudWatch, X-Ray, EventBridge

## System Examples

This guideline walks through 4 complete system examples:

1. **[Task Management System](examples/task-management.md)** - Simple system introducing foundational concepts
2. **[E-Commerce Platform](examples/e-commerce.md)** - Medium complexity with workflow orchestration
3. **[Trading System](examples/trading-system.md)** - Complex event-driven architecture
4. **[Content Management](examples/content-management.md)** - Enterprise patterns and multi-tenancy

## Get Started

Begin with the [Overview](getting-started/overview.md) to understand The Method's core principles and how they map to AWS services.
