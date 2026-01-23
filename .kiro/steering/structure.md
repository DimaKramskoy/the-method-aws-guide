# Project Structure

## About This Project

This project demonstrates how to apply **The Method** from Juval Löwy's "Righting Software" to modern AWS serverless development. All architectural principles, component taxonomy, and design concepts are attributed to Juval Löwy's work. Our contribution is showing how these principles naturally map to AWS cloud services.

**Reference**: Löwy, Juval. _Righting Software_. Boston: Addison-Wesley Professional, 2019.

## Directory Organization

```
.
├── .kiro/
│   └── steering/          # AI assistant steering rules
├── docs/
│   ├── guidelines/        # Architectural guidelines and patterns
│   ├── mappings/          # The Method → AWS service mappings
│   └── best-practices/    # Best practices documentation
├── examples/
│   ├── e-commerce/        # E-commerce system example
│   ├── trading-system/    # Trading system example (from the book)
│   └── [other-domains]/   # Additional domain examples
├── reference-designs/
│   ├── architectures/     # System architecture diagrams
│   ├── use-cases/         # Use case documentation
│   └── call-chains/       # Call chain diagrams
└── implementations/
    ├── infrastructure/    # IaC templates (CDK/Terraform)
    └── code-samples/      # Working code examples
```

## Content Organization

### Guidelines Documentation

Each guideline document should cover:

- The Method principle being applied
- AWS service mapping
- Code examples
- Anti-patterns to avoid

### Reference Designs

Each reference design should include:

- Business domain overview
- Volatility analysis
- Component diagram
- Layer structure
- Use case flows
- Call chains

### Implementation Examples

Each implementation should demonstrate:

- Proper component naming
- Volatility encapsulation
- Atomic business verbs
- Layer separation
- AWS service integration

## Architectural Conventions

### Component Naming

Follow The Method's taxonomy in component names:

- **Managers**: `[Domain]Manager` (e.g., `OrderManager`, `InventoryManager`)
- **Engines**: `[Activity]Engine` (e.g., `PricingEngine`, `ValidationEngine`)
- **ResourceAccess**: `[Resource]Access` (e.g., `OrderDataAccess`, `ProductDataAccess`)
- **Resources**: Actual AWS services (DynamoDB tables, S3 buckets, SQS queues)
- **Utilities**: `[Function]Utility` (e.g., `LoggingUtility`, `SecurityUtility`)

### AWS Service Naming

When defining AWS resources, use descriptive names that reflect their role:

- Lambda functions: `[ComponentName]-[ComponentType]` (e.g., `OrderManager-Manager`, `PricingEngine-Engine`)
- DynamoDB tables: `[Domain]-[Entity]` (e.g., `Orders-Table`, `Products-Table`)
- Step Functions: `[Workflow]Manager-StateMachine` (e.g., `OrderProcessingManager-StateMachine`)

### Code Organization for Examples

Organize example code by component type and layer:

```
examples/[domain]/
├── architecture/
│   ├── diagram.png        # System architecture
│   ├── volatility.md      # Volatility analysis
│   └── use-cases.md       # Use case documentation
├── infrastructure/
│   └── cdk/               # CDK infrastructure code
│       ├── clients/       # Client layer resources
│       ├── managers/      # Manager Lambda functions
│       ├── engines/       # Engine Lambda functions
│       ├── resource-access/ # ResourceAccess Lambda functions
│       ├── resources/     # DynamoDB, S3, etc.
│       └── utilities/     # CloudWatch, X-Ray, etc.
└── src/
    ├── managers/          # Manager implementation
    ├── engines/           # Engine implementation
    ├── resource-access/   # ResourceAccess implementation
    └── utilities/         # Utility implementation
```

## Architecture Patterns

### Volatility Encapsulation

- Identify areas of change in requirements
- Encapsulate each volatility in a dedicated component
- Use appropriate component type based on what changes
- Document the volatility analysis for each example

### Atomic Business Verbs

- ResourceAccess components expose business operations, not I/O operations
- Example: `CreditAccount()` and `DebitAccount()` instead of `Update()` or `Write()`
- Document the business verbs for each domain

### Layer Communication

- Clients call Managers (via API Gateway)
- Managers orchestrate Engines and ResourceAccess
- Engines may call ResourceAccess
- All layers can use Utilities
- No layer skipping or reverse dependencies
- Show clear call chains in diagrams

## Documentation Standards

### Architecture Diagrams

- Use consistent notation for component types
- Show layer boundaries clearly
- Include AWS service icons where appropriate
- Provide both logical and physical views

### Code Examples

- Include inline comments explaining The Method principles
- Show both correct and incorrect approaches
- Provide complete, runnable examples
- Include tests demonstrating component isolation

### Guidelines

- Start with the principle from The Method
- Explain the AWS service mapping
- Provide concrete examples
- Include common pitfalls and how to avoid them
