# Technology Stack

## Platform

**AWS (Amazon Web Services)** - Cloud platform for deploying The Method architecture

## Build System

**AWS CDK (Cloud Development Kit)** with TypeScript for infrastructure as code

## Languages & Frameworks

**Primary Language: TypeScript with Node.js**

TypeScript/Node.js is the chosen implementation language because:

- **Native AWS Integration**: Excellent AWS SDK support with strong typing
- **Lambda Runtime**: First-class support in AWS Lambda
- **CDK Native**: AWS CDK is built with TypeScript, providing the best developer experience
- **Type Safety**: Strong typing helps enforce The Method's component contracts
- **Ecosystem**: Rich ecosystem for serverless development
- **Readability**: Clear, accessible code for guideline examples

### Key Dependencies

- **AWS SDK v3**: For interacting with AWS services (@aws-sdk/\*)
- **AWS CDK**: Infrastructure as code (aws-cdk-lib)
- **TypeScript**: Type-safe JavaScript (^5.0.0)
- **Node.js**: Runtime environment (18.x or 20.x)
- **MkDocs**: Documentation site generator for Markdown files

## Key AWS Services

### Compute

- **Lambda**: Serverless functions for Managers, Engines, ResourceAccess
- **Step Functions**: Workflow orchestration (Manager layer)
- **ECS/Fargate**: Container-based compute (if needed for long-running processes)

### Storage & Databases

- **DynamoDB**: NoSQL database (Resource layer)
- **RDS/Aurora**: Relational databases (Resource layer, if needed)
- **S3**: Object storage (Resource layer)

### Messaging & Integration

- **SQS**: Message queues (Resource layer)
- **SNS**: Pub/Sub messaging (Utility layer)
- **EventBridge**: Event bus (Utility layer)
- **API Gateway**: API management (Client layer)

### Utilities

- **CloudWatch**: Logging and monitoring
- **X-Ray**: Distributed tracing
- **Secrets Manager**: Secure configuration
- **IAM**: Security and access control
- **CloudFormation/CDK**: Infrastructure as code

## Common Commands

### Development

```bash
# Install dependencies
npm install

# Run local tests
npm test

# Type check
npm run type-check

# Build TypeScript
npm run build

# Deploy to dev environment
npm run cdk:deploy -- --profile dev
```

### Testing

```bash
# Run unit tests
npm run test:unit

# Run integration tests
npm run test:integration

# Run tests with coverage
npm run test:coverage

# Test specific component
npm test -- managers/OrderManager.test.ts
```

### CDK Commands

```bash
# Synthesize CloudFormation template
npm run cdk:synth

# Show differences
npm run cdk:diff

# Deploy infrastructure
npm run cdk:deploy

# Destroy infrastructure
npm run cdk:destroy
```

### Deployment

```bash
# Deploy to dev
npm run deploy:dev

# Deploy to staging
npm run deploy:staging

# Deploy to production
npm run deploy:prod
```

### Documentation

```bash
# Install MkDocs (one-time setup)
pip install mkdocs

# Install Material theme (recommended, optional)
pip install mkdocs-material

# Serve documentation locally with live reload
# This creates a web UI at http://127.0.0.1:8000
mkdocs serve

# Build static documentation site
# Generates HTML files in site/ directory
mkdocs build

# Deploy documentation to GitHub Pages
mkdocs gh-deploy
```

**MkDocs Setup**:

- MkDocs converts Markdown files into a browsable documentation website
- Requires a `mkdocs.yml` configuration file at project root
- Markdown files remain as `.md` in the project
- `mkdocs serve` provides live preview with auto-reload
- Access documentation UI at http://127.0.0.1:8000 when serving locally

## Architecture Mapping

### The Method → AWS Services

**Client Layer**

- Web/Mobile apps → CloudFront + S3 (static hosting)
- APIs → API Gateway (REST/HTTP APIs)
- Admin tools → Amplify or custom apps

**Business Logic Layer (Managers & Engines)**

- Managers → Step Functions (complex workflows) or Lambda (simple workflows)
- Engines → Lambda functions (business logic execution)
- Communication → Direct invocation or EventBridge

**Resource Access Layer**

- ResourceAccess → Lambda functions with AWS SDK
- Expose atomic business verbs, not CRUD operations
- Example: `creditAccount()` not `updateRecord()`

**Resource Layer**

- Databases → DynamoDB (primary), RDS/Aurora (if relational needed)
- Storage → S3 (documents, files)
- Queues → SQS (async processing)
- Caches → ElastiCache (if needed)

**Utilities Bar**

- Logging → CloudWatch Logs
- Monitoring → CloudWatch Metrics
- Tracing → X-Ray
- Security → IAM, Secrets Manager
- Messaging → SNS, EventBridge
- Configuration → Systems Manager Parameter Store

## Project Structure

```
project-root/
├── bin/
│   └── app.ts                 # CDK app entry point
├── lib/
│   ├── stacks/                # CDK stack definitions
│   └── constructs/            # Reusable CDK constructs
├── src/
│   ├── managers/              # Manager Lambda functions
│   ├── engines/               # Engine Lambda functions
│   ├── resource-access/       # ResourceAccess Lambda functions
│   ├── utilities/             # Utility functions
│   └── shared/                # Shared types and utilities
├── test/
│   ├── unit/                  # Unit tests
│   └── integration/           # Integration tests
├── docs/                      # Documentation (Markdown files)
│   ├── guidelines/            # Architectural guidelines
│   ├── examples/              # System examples
│   └── reference/             # Reference materials
├── mkdocs.yml                 # MkDocs configuration
├── cdk.json                   # CDK configuration
├── tsconfig.json              # TypeScript configuration
└── package.json               # Node.js dependencies
```

## Code Standards

### TypeScript Configuration

- Strict mode enabled
- ES2020 target
- CommonJS modules for Lambda
- Source maps for debugging

### Naming Conventions

- Lambda functions: `[ComponentName][ComponentType]` (e.g., `OrderManagerFunction`)
- CDK constructs: `[ComponentName][ComponentType]Construct`
- Interfaces: `I[Name]` (e.g., `IOrderManager`)
- Types: `[Name]Type` (e.g., `OrderType`)

### Component Contracts

Use TypeScript interfaces to define component contracts:

```typescript
// Manager contract
interface IOrderManager {
  processOrder(orderId: string): Promise<OrderResult>;
}

// Engine contract
interface IPricingEngine {
  calculatePrice(items: Item[]): Promise<Price>;
}

// ResourceAccess contract (atomic business verbs)
interface IOrderDataAccess {
  createOrder(order: Order): Promise<string>;
  getOrder(orderId: string): Promise<Order>;
  // NOT: updateRecord(), deleteRecord()
}
```
