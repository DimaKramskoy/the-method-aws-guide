# Introduction

This guideline demonstrates how to apply The Method—Juval Löwy's structured engineering approach to system design—to AWS serverless services. The Method, introduced in "Righting Software," provides a systematic framework for building maintainable, resilient software systems based on volatility-based decomposition, component taxonomy, and layered architecture.

**Reference**: Löwy, Juval. _Righting Software_. Boston: Addison-Wesley Professional, 2019.

## Attribution and Scope

All core architectural principles presented in this guide—volatility-based decomposition, component taxonomy, and layered architecture—originate from Juval Löwy's work. These principles represent decades of refinement in software engineering practice.

Our contribution is demonstrating how these principles map to AWS serverless services. Throughout this guide, we distinguish between:

- **The Method's principles** (attributed to Löwy): Fundamental design concepts and architectural patterns
- **AWS interpretation** (our contribution): Specific mappings to AWS services and implementation patterns

When we reference "As Löwy explains..." or "The Method teaches us...", we are citing his original work. When we state "We interpret this for AWS as..." or "In our AWS implementation...", we are presenting our application of his principles to cloud services.

## Why The Method Matters for Cloud Development

Software projects frequently fail due to architectural decisions that cannot accommodate changing requirements. As Löwy demonstrates in "Righting Software," the root cause is typically not technical incompetence but rather the application of functional decomposition—organizing systems by what they do rather than by what changes.

The Method addresses this through volatility-based decomposition, which identifies areas of potential change and encapsulates them into discrete system components. This approach, grounded in engineering principles proven across multiple disciplines, provides a foundation for building systems that remain stable despite evolving requirements.

The Method's principles demonstrate natural alignment with AWS serverless services. The component taxonomy maps directly to AWS service capabilities, the layered architecture corresponds to AWS service boundaries, and the emphasis on encapsulation aligns with serverless design patterns.

## What You'll Learn

Through this guide, you'll see how to:

- **Decompose systems based on volatility** (what changes) rather than function (what the system does)
- **Use a clear component taxonomy** that maps naturally to AWS services
- **Build layered architectures** that are maintainable and resilient to change
- **Apply atomic business verbs** instead of generic CRUD operations
- **Design systems that actually survive requirement changes**

We'll walk through complete system examples from different domains—task management, e-commerce, trading systems, and content management. Each example builds on the previous one, introducing new concepts progressively.

## How This Guide Works

Each system example follows the same structure:

1. **Business Context** - What problem are we solving?
2. **Volatility Analysis** - What's likely to change? (This is Löwy's key insight)
3. **Architecture Design** - How do we encapsulate those changes?
4. **AWS Service Mapping** - Which AWS services implement our design? (This is our interpretation)
5. **Use Case Flows** - How does it work at runtime?
6. **Implementation Highlights** - What does the code look like?
7. **Lessons Learned** - What did we discover?

## A Note on Attribution

Everything you'll learn about volatility-based decomposition, component taxonomy, and layered architecture comes from Juval Löwy's "Righting Software." These are his principles, refined over decades of experience.

Our contribution is showing how these principles apply to AWS serverless services. When we say "As Löwy explains..." or "The Method teaches us...", we're referring to his work. When we say "We interpret this for AWS as..." or "In our experience with AWS...", that's our application of his principles to the cloud.

## The Core Principle

The Method's fundamental directive, as stated by Löwy:

> "Decompose based on volatility."

This principle represents a paradigm shift from functional decomposition (organizing by what the system does) to volatility-based decomposition (organizing by what changes). As Löwy explains, volatility-based decomposition identifies areas of potential change and encapsulates those into services or system building blocks. The required system behavior emerges from the interaction between these encapsulated areas of volatility.

The significance of this approach lies in change containment. When requirements evolve, modifications remain isolated within specific components. Löwy uses the metaphor of vaults: changes are contained within the appropriate vault, leaving the remainder of the system unaffected. This encapsulation provides resilience against requirement volatility and feature creep during development.

Throughout the examples in this guide, we demonstrate how this principle manifests in AWS serverless architectures.

## Approach and Methodology

This guide differs from typical AWS documentation in its focus. Rather than explaining how to use individual AWS services, we demonstrate how to design systems using proven architectural principles, with AWS services serving as the implementation medium.

The emphasis is on architectural thinking—understanding volatility analysis, component responsibility assignment, and layer communication patterns. AWS service selection becomes a natural consequence of sound architectural decisions rather than the starting point of design.

Next: **[Core Principles](core-principles.md)** - The foundation of The Method
