# Product Overview

A comprehensive guideline and reference implementation for applying Juval Löwy's "The Method" architectural principles to AWS serverless services.

## Attribution

This project is based on **"Righting Software"** by Juval Löwy (Addison-Wesley, 2019), which introduces **The Method** - a structured engineering approach to system and project design. This guideline adapts The Method's principles for modern cloud development, specifically viewing them through the lens of AWS serverless services.

**Key Reference**: Löwy, Juval. _Righting Software_. Boston: Addison-Wesley Professional, 2019.

We fully acknowledge and attribute all core architectural principles, component taxonomy, and design concepts to Juval Löwy and his work. This project's contribution is demonstrating how these proven principles apply naturally to AWS cloud services.

## Purpose

This project serves as a **practical guide** that brings The Method to modern cloud development. It demonstrates how to build systems "the right way" using proven architectural principles with AWS services.

**The Goal**: Bridge the gap between traditional software engineering principles (from "Righting Software") and modern serverless cloud development on AWS.

The project provides:

- **Architectural guidelines** for mapping The Method's component taxonomy to AWS serverless services
- **Reference implementations** showing concrete examples of well-designed systems
- **System design patterns** that leverage AWS services while maintaining The Method's principles
- **Best practices** for volatility-based decomposition in cloud-native architectures

The focus is on **serverless services** where natural parallels exist between The Method's concepts and AWS's service offerings.

## Product Deliverables

### 1. Guidelines Documentation

- How to map Managers, Engines, and ResourceAccess to AWS Lambda, Step Functions, etc.
- Patterns for maintaining volatility encapsulation in serverless architectures
- Best practices for atomic business verbs in AWS service interfaces

### 2. Reference System Designs

- Complete architectural diagrams following The Method
- Use case examples with call chains
- Volatility analysis for common business domains

### 3. Implementation Examples

- Working code samples demonstrating proper component design
- Infrastructure as Code (IaC) templates following The Method's structure
- Integration patterns between AWS services

## Key Principles

### Volatility-Based Decomposition

Systems are decomposed based on areas of change (volatility) rather than functional decomposition. Changes are encapsulated within components, minimizing system-wide impact.

### Component Taxonomy

The Method defines specific component types:

- **Managers**: Encapsulate volatility in use case sequences/workflows
- **Engines**: Encapsulate volatility in business activities and rules
- **ResourceAccess**: Encapsulate volatility in accessing resources (databases, storage, etc.)
- **Resources**: Actual physical resources (databases, queues, storage)
- **Utilities**: Common infrastructure services (security, logging, messaging)
- **Clients**: User-facing applications and interfaces

### Layered Architecture

Systems are organized in layers:

1. Client Layer - Various client applications
2. Business Logic Layer - Managers and Engines
3. Resource Access Layer - ResourceAccess components
4. Resource Layer - Physical resources
5. Utilities Bar - Cross-cutting infrastructure

## Natural Parallels with AWS Serverless

The Method's architecture naturally aligns with AWS serverless services:

- **Managers** → Step Functions (workflow orchestration) or Lambda (simple workflows)
- **Engines** → Lambda functions (business logic execution)
- **ResourceAccess** → Lambda functions with AWS SDK (atomic business operations)
- **Resources** → DynamoDB, S3, SQS, etc. (managed services)
- **Utilities** → CloudWatch, X-Ray, EventBridge (cross-cutting concerns)

## Target Users

- Software architects designing cloud-native systems on AWS
- Development teams wanting to apply proven architectural principles to serverless
- Organizations seeking maintainable, extensible cloud architectures
- Anyone learning how to bring engineering discipline to modern cloud development
- Readers of "Righting Software" looking for cloud-specific applications

## Project Mission

Demonstrate that The Method's principles from "Righting Software" are not only relevant but naturally aligned with modern AWS serverless services, enabling developers to build cloud systems with the same engineering rigor as traditional software systems.
