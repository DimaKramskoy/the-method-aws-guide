# Design Document: Main Guideline Document

## Overview

This design specifies the structure, content, and organization of the main guideline document that teaches readers how to apply The Method from "Righting Software" to AWS serverless services. The guideline uses a progressive learning approach through 3-5 complete system examples from different business domains, each demonstrating key architectural principles and AWS service mappings.

The document serves as both an educational resource and a practical reference, showing how volatility-based decomposition, component taxonomy, and layered architecture naturally align with AWS serverless services.

## Architecture

### Document Structure

The guideline document follows a progressive learning architecture with three main sections:

1. **Foundation Section**: Introduction, core principles, and component taxonomy
2. **Examples Section**: 3-5 complete system examples with increasing complexity
3. **Synthesis Section**: Conclusion, patterns, best practices, and reference materials

### Information Architecture

```
Main Guideline Document
│
├── Front Matter
│   ├── Title and Attribution
│   ├── Table of Contents
│   └── How to Use This Guide
│
├── Part 1: Foundation
│   ├── Introduction to The Method
│   ├── Core Principles
│   ├── Component Taxonomy
│   ├── Layered Architecture
│   └── AWS Serverless Parallels
│
├── Part 2: System Examples (3-5 examples)
│   ├── Example 1: Simple System (Foundation)
│   ├── Example 2: Medium System (Building Complexity)
│   ├── Example 3: Complex System (Advanced Patterns)
│   ├── [Optional] Example 4: Specialized Pattern
│   └── [Optional] Example 5: Enterprise Scale
│
└── Part 3: Synthesis
    ├── Cross-Cutting Patterns
    ├── Best Practices Checklist
    ├── AWS Service Selection Guide
    ├── Common Anti-Patterns
    └── Further Resources
```

### Example Selection Strategy

The design specifies **4 system examples** (3 required + 1 optional) to balance comprehensive coverage with maintainability:

1. **Example 1 - Task Management System** (Simple, Foundation)
   - Domain: Productivity/Collaboration
   - Complexity: Low
   - Focus: Basic component types, simple workflows, foundational patterns
   - AWS Services: API Gateway, Lambda, DynamoDB, S3

2. **Example 2 - E-Commerce Order Processing** (Medium, Multi-Channel)
   - Domain: Retail/Commerce
   - Complexity: Medium
   - Focus: Client volatility, multiple UIs, workflow orchestration
   - AWS Services: Step Functions, Lambda, DynamoDB, SQS, SNS

3. **Example 3 - Real-Time Trading System** (Complex, High-Performance)
   - Domain: Financial Services
   - Complexity: High
   - Focus: Engine reuse, parallel processing, event-driven architecture
   - AWS Services: EventBridge, Lambda, DynamoDB Streams, Kinesis

4. **Example 4 - Content Management Platform** (Optional, Enterprise)
   - Domain: Media/Publishing
   - Complexity: Medium-High
   - Focus: Resource volatility, multi-tenancy, caching strategies
   - AWS Services: CloudFront, S3, Lambda@Edge, ElastiCache, RDS

### Progressive Learning Path

Each example introduces new concepts while building on previous knowledge:

- **Example 1**: Introduces all component types, basic volatility analysis, simple Manager-Engine-ResourceAccess patterns
- **Example 2**: Adds workflow orchestration (Step Functions), client volatility, async messaging
- **Example 3**: Introduces event-driven patterns, engine reuse, parallel processing, performance considerations
- **Example 4**: Covers resource volatility, caching, multi-tenancy, enterprise patterns

## Components and Interfaces

### Document Components

#### 1. Foundation Components

**Introduction Module**

- Purpose: Establish context and motivation
- Content: The Method overview, AWS relevance, document roadmap
- Length: 2-3 pages

**Core Principles Module**

- Purpose: Teach fundamental concepts
- Content: Volatility-based decomposition, component taxonomy, layered architecture
- Length: 3-4 pages
- Diagrams: Layered architecture diagram, component type icons

**Component Taxonomy Module**

- Purpose: Define each component type
- Content: Manager, Engine, ResourceAccess, Resource, Utility, Client definitions
- Length: 2-3 pages
- Format: Definition, purpose, characteristics, AWS mapping for each type

#### 2. Example Components

Each system example follows this structure:

**Business Context Section**

- Domain introduction
- Business requirements
- Key stakeholders
- Success criteria

**Volatility Analysis Section**

- Identification of volatile areas
- Axes of volatility
- Mapping volatility to component types
- Rationale for decomposition decisions

**Architecture Design Section**

- Component diagram (logical view)
- Layer diagram
- Component descriptions
- Interface definitions

**AWS Service Mapping Section**

- Physical deployment diagram
- Service selection rationale
- Configuration considerations
- Cost and performance implications

**Use Case Flows Section**

- 2-3 key use cases
- Call chain diagrams
- Sequence of operations
- Layer communication patterns

**Implementation Highlights Section**

- TypeScript interface definitions
- Lambda function structure
- CDK infrastructure code
- Atomic business verb examples

**Lessons Learned Section**

- Key takeaways
- Design decisions and trade-offs
- Anti-patterns avoided
- Concepts introduced

#### 3. Synthesis Components

**Cross-Cutting Patterns Module**

- Common patterns across examples
- When to apply each pattern
- Pattern variations

**Best Practices Checklist Module**

- Step-by-step design process
- Verification checklist
- Common pitfalls

**AWS Service Selection Guide Module**

- Decision tree for service selection
- Service comparison matrix
- Cost considerations

### Component Interfaces

#### Example Template Interface

```typescript
interface ISystemExample {
  // Metadata
  title: string;
  domain: string;
  complexity: "simple" | "medium" | "complex";

  // Content sections
  businessContext: BusinessContext;
  volatilityAnalysis: VolatilityAnalysis;
  architecture: ArchitectureDesign;
  awsMapping: AWSServiceMapping;
  useCases: UseCase[];
  implementation: ImplementationHighlights;
  lessonsLearned: LessonsLearned;
}

interface BusinessContext {
  overview: string;
  requirements: string[];
  stakeholders: string[];
  successCriteria: string[];
}

interface VolatilityAnalysis {
  volatileAreas: VolatileArea[];
  decompositionRationale: string;
}

interface VolatileArea {
  name: string;
  description: string;
  changeDrivers: string[];
  componentType: ComponentType;
  encapsulationStrategy: string;
}

interface ArchitectureDesign {
  components: Component[];
  layers: Layer[];
  diagrams: Diagram[];
}

interface Component {
  name: string;
  type: ComponentType;
  layer: string;
  responsibilities: string[];
  interfaces: InterfaceDefinition[];
}

interface AWSServiceMapping {
  mappings: ServiceMapping[];
  deploymentDiagram: Diagram;
  rationale: string;
}

interface ServiceMapping {
  component: string;
  awsService: string;
  configuration: string;
  justification: string;
}

interface UseCase {
  name: string;
  scenario: string;
  callChain: CallChainStep[];
  diagram: Diagram;
}

interface CallChainStep {
  sequence: number;
  component: string;
  operation: string;
  layer: string;
}
```

## Data Models

### Example Metadata Model

```typescript
type ComponentType =
  | "Manager"
  | "Engine"
  | "ResourceAccess"
  | "Resource"
  | "Utility"
  | "Client";

type ComplexityLevel = "simple" | "medium" | "complex";

type AWSService =
  | "Lambda"
  | "Step Functions"
  | "API Gateway"
  | "DynamoDB"
  | "S3"
  | "SQS"
  | "SNS"
  | "EventBridge"
  | "CloudWatch"
  | "X-Ray"
  | "Kinesis"
  | "CloudFront"
  | "ElastiCache"
  | "RDS";

interface ExampleMetadata {
  id: string;
  title: string;
  domain: string;
  complexity: ComplexityLevel;
  order: number;
  conceptsIntroduced: string[];
  prerequisiteExamples: string[];
  awsServices: AWSService[];
  estimatedReadingTime: number; // minutes
}
```

### Diagram Specification Model

```typescript
interface DiagramSpec {
  type: "component" | "layer" | "deployment" | "sequence" | "call-chain";
  format: "mermaid" | "png" | "svg";
  title: string;
  description: string;
  elements: DiagramElement[];
}

interface DiagramElement {
  id: string;
  type: string;
  label: string;
  properties: Record<string, any>;
  connections: Connection[];
}

interface Connection {
  from: string;
  to: string;
  label?: string;
  type: "call" | "data-flow" | "dependency" | "layer-boundary";
}
```

### Content Organization Model

```typescript
interface DocumentSection {
  id: string;
  title: string;
  level: number; // heading level 1-6
  content: string; // markdown content
  subsections: DocumentSection[];
  diagrams: DiagramSpec[];
  codeBlocks: CodeBlock[];
}

interface CodeBlock {
  language: string;
  code: string;
  caption?: string;
  highlightLines?: number[];
  filename?: string;
}
```

## Detailed Example Specifications

### Example 1: Task Management System

**Domain**: Productivity/Collaboration
**Complexity**: Simple
**Purpose**: Introduce foundational concepts

**Business Context**:

- Users create, update, and complete tasks
- Tasks have descriptions, due dates, priorities
- Users can organize tasks into projects
- Simple notification system for due dates

**Volatility Analysis**:

- **Use Case Volatility**: Task workflow (create → assign → complete) → TaskManager
- **Business Rules Volatility**: Priority calculation, due date logic → PriorityEngine
- **Data Access Volatility**: Task storage operations → TaskDataAccess
- **Notification Volatility**: Notification delivery → NotificationUtility

**Components**:

- **Managers**: TaskManager (orchestrates task operations)
- **Engines**: PriorityEngine (calculates task priority), ValidationEngine (validates task data)
- **ResourceAccess**: TaskDataAccess (atomic task operations), ProjectDataAccess
- **Resources**: Tasks DynamoDB table, Projects DynamoDB table
- **Utilities**: NotificationUtility (SNS), LoggingUtility (CloudWatch)
- **Clients**: Web UI (React), Mobile App (React Native)

**AWS Services**:

- API Gateway → Client entry point
- Lambda → TaskManager, PriorityEngine, ValidationEngine, TaskDataAccess
- DynamoDB → Tasks and Projects tables
- SNS → Notifications
- CloudWatch → Logging

**Key Use Cases**:

1. Create Task: Client → API Gateway → TaskManager → ValidationEngine → TaskDataAccess → DynamoDB
2. Complete Task: Client → API Gateway → TaskManager → TaskDataAccess → DynamoDB → NotificationUtility → SNS

**Concepts Introduced**:

- Basic component types
- Simple Manager orchestration
- Atomic business verbs (createTask, completeTask, not updateRecord)
- Layer communication rules
- Basic AWS service mapping

### Example 2: E-Commerce Order Processing

**Domain**: Retail/Commerce
**Complexity**: Medium
**Purpose**: Introduce workflow orchestration and client volatility

**Business Context**:

- Customers place orders through web, mobile, or API
- Orders go through: validation → payment → inventory → fulfillment
- Multiple payment methods (credit card, PayPal, gift card)
- Inventory management across warehouses
- Order status notifications

**Volatility Analysis**:

- **Client Volatility**: Multiple UIs (web, mobile, partner API) → Separate client implementations
- **Workflow Volatility**: Order processing sequence → OrderProcessingManager (Step Functions)
- **Payment Volatility**: Payment method logic → PaymentEngine
- **Inventory Volatility**: Inventory allocation rules → InventoryEngine
- **Data Access Volatility**: Order operations → OrderDataAccess

**Components**:

- **Managers**: OrderProcessingManager (Step Functions workflow), InventoryManager
- **Engines**: PaymentEngine, InventoryEngine, PricingEngine, ValidationEngine
- **ResourceAccess**: OrderDataAccess, InventoryDataAccess, CustomerDataAccess
- **Resources**: Orders table, Inventory table, Customers table, Order queue (SQS)
- **Utilities**: PaymentGatewayUtility, NotificationUtility, LoggingUtility
- **Clients**: Web storefront, Mobile app, Partner API

**AWS Services**:

- API Gateway → Multiple client entry points
- Step Functions → OrderProcessingManager workflow
- Lambda → All Engines and ResourceAccess components
- DynamoDB → Orders, Inventory, Customers tables
- SQS → Order processing queue
- SNS → Order notifications
- Secrets Manager → Payment gateway credentials

**Key Use Cases**:

1. Place Order: Client → API Gateway → OrderProcessingManager (Step Functions) → ValidationEngine → PaymentEngine → InventoryEngine → OrderDataAccess
2. Check Inventory: InventoryManager → InventoryEngine → InventoryDataAccess → DynamoDB

**Concepts Introduced**:

- Step Functions for complex workflows
- Client volatility and multiple UIs
- Engine reuse (ValidationEngine used by multiple Managers)
- Async messaging with SQS
- External service integration (payment gateway)
- Secrets management

### Example 3: Real-Time Trading System

**Domain**: Financial Services
**Complexity**: Complex
**Purpose**: Introduce event-driven architecture and high-performance patterns

**Business Context**:

- Traders submit buy/sell orders
- Real-time market data feeds
- Order matching engine
- Risk management and compliance checks
- Portfolio tracking and P&L calculation
- Audit trail for all transactions

**Volatility Analysis**:

- **Event Volatility**: Market data events, order events → Event-driven architecture
- **Matching Volatility**: Order matching algorithm → MatchingEngine
- **Risk Volatility**: Risk calculation rules → RiskEngine
- **Pricing Volatility**: P&L calculation → PricingEngine
- **Compliance Volatility**: Regulatory rules → ComplianceEngine

**Components**:

- **Managers**: TradeManager, PortfolioManager, RiskManager
- **Engines**: MatchingEngine, RiskEngine, PricingEngine, ComplianceEngine
- **ResourceAccess**: OrderDataAccess, PositionDataAccess, MarketDataAccess
- **Resources**: Orders table (DynamoDB), Positions table, Market data stream (Kinesis)
- **Utilities**: AuditUtility, MonitoringUtility (X-Ray), AlertingUtility
- **Clients**: Trading terminal, Risk dashboard, Compliance portal

**AWS Services**:

- EventBridge → Event routing and filtering
- Kinesis → Market data streaming
- Lambda → All Managers, Engines, ResourceAccess
- DynamoDB → Orders, Positions tables with streams
- DynamoDB Streams → Change data capture
- Step Functions → Complex trade workflows
- X-Ray → Distributed tracing
- CloudWatch → Real-time monitoring

**Key Use Cases**:

1. Submit Order: Client → TradeManager → ComplianceEngine → RiskEngine → MatchingEngine → OrderDataAccess → EventBridge
2. Process Market Data: Kinesis → MarketDataAccess → PricingEngine → PositionDataAccess → EventBridge
3. Calculate Risk: DynamoDB Stream → RiskManager → RiskEngine → PositionDataAccess

**Concepts Introduced**:

- Event-driven architecture with EventBridge
- Stream processing with Kinesis and DynamoDB Streams
- Engine reuse across multiple Managers
- Parallel processing patterns
- High-performance considerations
- Distributed tracing with X-Ray
- Change data capture patterns

### Example 4: Content Management Platform (Optional)

**Domain**: Media/Publishing
**Complexity**: Medium-High
**Purpose**: Introduce resource volatility and enterprise patterns

**Business Context**:

- Multi-tenant content platform
- Content creation, editing, publishing workflow
- Multiple content types (articles, videos, images)
- Content delivery with CDN
- Search and discovery
- User permissions and access control

**Volatility Analysis**:

- **Resource Volatility**: Different storage for different content types → Multiple ResourceAccess components
- **Workflow Volatility**: Publishing workflow → PublishingManager
- **Search Volatility**: Search algorithm → SearchEngine
- **Permission Volatility**: Access control rules → PermissionEngine
- **Delivery Volatility**: CDN configuration → DeliveryUtility

**Components**:

- **Managers**: ContentManager, PublishingManager, UserManager
- **Engines**: SearchEngine, PermissionEngine, ValidationEngine, TransformationEngine
- **ResourceAccess**: ArticleDataAccess, MediaDataAccess, UserDataAccess, SearchDataAccess
- **Resources**: Articles table (DynamoDB), Media bucket (S3), User table, Search index (OpenSearch)
- **Utilities**: CDNUtility (CloudFront), CacheUtility (ElastiCache), TranscodingUtility (MediaConvert)
- **Clients**: CMS admin panel, Public website, Mobile app

**AWS Services**:

- CloudFront → Content delivery
- S3 → Media storage
- Lambda@Edge → Edge computing for personalization
- Lambda → All Managers, Engines, ResourceAccess
- DynamoDB → Articles, Users tables
- RDS/Aurora → Relational data (if needed)
- OpenSearch → Full-text search
- ElastiCache → Caching layer
- MediaConvert → Video transcoding

**Key Use Cases**:

1. Publish Article: CMS → ContentManager → ValidationEngine → TransformationEngine → ArticleDataAccess → DynamoDB → CDNUtility → CloudFront
2. Search Content: Client → SearchEngine → SearchDataAccess → OpenSearch → CacheUtility
3. Deliver Media: Client → CloudFront → Lambda@Edge → PermissionEngine → S3

**Concepts Introduced**:

- Resource volatility and multiple storage types
- Multi-tenancy patterns
- Caching strategies with ElastiCache
- CDN integration with CloudFront
- Edge computing with Lambda@Edge
- Full-text search with OpenSearch
- Media processing pipelines

## Diagram Specifications

### Standard Diagram Types

#### 1. Component Diagram

- **Purpose**: Show all components and their types
- **Format**: Mermaid graph
- **Elements**: Rectangles for components, color-coded by type
- **Connections**: Solid lines for calls, dashed for data flow
- **Layers**: Horizontal swim lanes

#### 2. Layer Diagram

- **Purpose**: Show layered architecture
- **Format**: Mermaid graph or custom SVG
- **Elements**: Horizontal layers with components inside
- **Annotations**: Layer names, communication rules

#### 3. Deployment Diagram

- **Purpose**: Show AWS service mapping
- **Format**: Mermaid graph with AWS icons
- **Elements**: AWS service boxes, Lambda functions, databases
- **Connections**: API calls, event flows, data flows

#### 4. Call Chain Diagram

- **Purpose**: Show use case execution flow
- **Format**: Mermaid sequence diagram
- **Elements**: Components as participants, operations as messages
- **Annotations**: Layer boundaries, return values

#### 5. Volatility Map

- **Purpose**: Show volatile areas and their encapsulation
- **Format**: Custom diagram
- **Elements**: Volatile areas as bubbles, components as containers
- **Annotations**: Change drivers, volatility axes

### Diagram Style Guide

**Color Coding**:

- Managers: Blue (#4A90E2)
- Engines: Green (#7ED321)
- ResourceAccess: Orange (#F5A623)
- Resources: Purple (#BD10E0)
- Utilities: Gray (#9B9B9B)
- Clients: Red (#D0021B)

**Typography**:

- Component names: Bold, 12pt
- Operation names: Regular, 10pt
- Annotations: Italic, 9pt

**Layout**:

- Top-to-bottom for layer diagrams
- Left-to-right for call chains
- Hierarchical for component diagrams

## Implementation Highlights Structure

### Code Example Categories

#### 1. Interface Definitions

Show TypeScript interfaces for each component type:

```typescript
// Manager interface
interface IOrderProcessingManager {
  processOrder(orderId: string): Promise<OrderResult>;
  cancelOrder(orderId: string): Promise<void>;
}

// Engine interface
interface IPricingEngine {
  calculatePrice(items: Item[], customer: Customer): Promise<Price>;
  applyDiscount(price: Price, discountCode: string): Promise<Price>;
}

// ResourceAccess interface (atomic business verbs)
interface IOrderDataAccess {
  createOrder(order: Order): Promise<string>;
  getOrder(orderId: string): Promise<Order>;
  markOrderAsPaid(orderId: string): Promise<void>;
  markOrderAsShipped(orderId: string, trackingNumber: string): Promise<void>;
  // NOT: updateOrder(), deleteOrder()
}
```

#### 2. Lambda Function Structure

Show standard structure for each component type:

```typescript
// Manager Lambda
export const handler = async (
  event: APIGatewayEvent,
): Promise<APIGatewayResponse> => {
  const orderId = event.pathParameters?.orderId;

  // Orchestrate engines and resource access
  const validationResult = await validationEngine.validate(orderId);
  const paymentResult = await paymentEngine.processPayment(orderId);
  const inventoryResult = await inventoryEngine.allocate(orderId);

  await orderDataAccess.markOrderAsPaid(orderId);

  return {
    statusCode: 200,
    body: JSON.stringify({ orderId, status: "processed" }),
  };
};

// Engine Lambda
export const handler = async (event: EngineEvent): Promise<EngineResult> => {
  // Pure business logic, no I/O
  const price = calculateBasePrice(event.items);
  const tax = calculateTax(price, event.location);
  const discount = applyBusinessRules(price, event.customer);

  return {
    finalPrice: price + tax - discount,
    breakdown: { price, tax, discount },
  };
};

// ResourceAccess Lambda
export const handler = async (
  event: DataAccessEvent,
): Promise<DataAccessResult> => {
  const dynamodb = new DynamoDBClient({});

  // Atomic business operation
  await dynamodb.send(
    new PutItemCommand({
      TableName: "Orders",
      Item: marshall(event.order),
      ConditionExpression: "attribute_not_exists(orderId)",
    }),
  );

  return { orderId: event.order.orderId };
};
```

#### 3. CDK Infrastructure Code

Show how to define infrastructure for each example:

```typescript
// Manager as Step Functions
const orderProcessingStateMachine = new sfn.StateMachine(
  this,
  "OrderProcessing",
  {
    definition: sfn.Chain.start(
      new tasks.LambdaInvoke(this, "Validate", {
        lambdaFunction: validationEngine,
      }),
    )
      .next(
        new tasks.LambdaInvoke(this, "ProcessPayment", {
          lambdaFunction: paymentEngine,
        }),
      )
      .next(
        new tasks.LambdaInvoke(this, "AllocateInventory", {
          lambdaFunction: inventoryEngine,
        }),
      ),
  },
);

// Engine as Lambda
const pricingEngine = new lambda.Function(this, "PricingEngine", {
  runtime: lambda.Runtime.NODEJS_18_X,
  handler: "pricing-engine.handler",
  code: lambda.Code.fromAsset("dist/engines"),
  environment: {
    TAX_RATE_TABLE: taxRateTable.tableName,
  },
});

// ResourceAccess as Lambda with DynamoDB access
const orderDataAccess = new lambda.Function(this, "OrderDataAccess", {
  runtime: lambda.Runtime.NODEJS_18_X,
  handler: "order-data-access.handler",
  code: lambda.Code.fromAsset("dist/resource-access"),
  environment: {
    ORDERS_TABLE: ordersTable.tableName,
  },
});

ordersTable.grantReadWriteData(orderDataAccess);
```

#### 4. Anti-Pattern Examples

Show what NOT to do:

```typescript
// ❌ WRONG: CRUD-based ResourceAccess
interface IOrderDataAccess {
  create(order: Order): Promise<string>;
  read(orderId: string): Promise<Order>;
  update(orderId: string, updates: Partial<Order>): Promise<void>;
  delete(orderId: string): Promise<void>;
}

// ✅ CORRECT: Atomic business verbs
interface IOrderDataAccess {
  createOrder(order: Order): Promise<string>;
  getOrder(orderId: string): Promise<Order>;
  markOrderAsPaid(orderId: string): Promise<void>;
  markOrderAsShipped(orderId: string, trackingNumber: string): Promise<void>;
  cancelOrder(orderId: string, reason: string): Promise<void>;
}

// ❌ WRONG: Layer violation (Client calling Engine directly)
const price = await pricingEngine.calculatePrice(items);

// ✅ CORRECT: Client calls Manager, Manager calls Engine
const orderResult = await orderManager.processOrder(orderId);

// ❌ WRONG: Functional decomposition
class OrderService {
  createOrder() {}
  updateOrder() {}
  deleteOrder() {}
}

// ✅ CORRECT: Volatility-based decomposition
class OrderManager {
  /* orchestrates workflow */
}
class PricingEngine {
  /* pricing rules */
}
class OrderDataAccess {
  /* atomic operations */
}
```

## Content Writing Guidelines

### Writing Style

- **Tone**: Friendly and accessible, technically correct but not overly complex
- **Voice**: Active voice, conversational ("let's explore...", "we'll see how...")
- **Tense**: Present tense for principles, past tense for examples
- **Clarity**: Simple language that resonates with the author's intent, short sentences, clear terminology
- **Attribution**: Consistently acknowledge Juval Löwy and "Righting Software" as the source of The Method's principles
- **Perspective**: Clearly distinguish between:
  - The Method's principles (attributed to Juval Löwy)
  - Our interpretation and application to AWS (our contribution)
  - Use phrases like "As Löwy explains...", "The Method teaches us...", "We interpret this for AWS as..."

### Section Length Guidelines

- Introduction: 2-3 pages
- Core Principles: 3-4 pages
- Each Example: 8-12 pages
  - Business Context: 1 page
  - Volatility Analysis: 2 pages
  - Architecture Design: 2-3 pages
  - AWS Mapping: 1-2 pages
  - Use Cases: 2 pages
  - Implementation: 1-2 pages
  - Lessons Learned: 1 page
- Conclusion: 2-3 pages
- **Total**: 40-60 pages

### Code Example Guidelines

- Maximum 30 lines per code block
- Include comments explaining The Method principles
- Show both interface and implementation
- Highlight key lines
- Provide context before code
- Explain after code

### Diagram Guidelines

- One diagram per concept
- Maximum 10 components per diagram
- Clear labels and legends
- Consistent notation
- Alt text for accessibility

## Error Handling

### Content Validation

**Missing Content Detection**:

- Each example must have all required sections
- Each section must meet minimum length requirements
- All diagrams must have alt text
- All code blocks must have language specified

**Consistency Validation**:

- Component names consistent across diagrams and text
- AWS service names match official AWS documentation
- Color coding consistent across all diagrams
- Terminology consistent with "Righting Software"

**Quality Validation**:

- No broken internal links
- All external links valid
- All code examples syntactically correct
- All diagrams render correctly

### Error Recovery

**Missing Section**:

- Generate placeholder with TODO marker
- Log warning for review
- Continue with other sections

**Invalid Diagram**:

- Fall back to text description
- Log error for manual diagram creation
- Include diagram specification for later rendering

**Broken Link**:

- Mark link as broken with annotation
- Log for correction
- Continue document generation

## Testing Strategy

### Unit Testing

**Content Structure Tests**:

- Verify all required sections present
- Verify section ordering correct
- Verify heading hierarchy valid
- Verify table of contents matches sections

**Example Completeness Tests**:

- Each example has all required subsections
- Each example introduces unique concepts
- Examples ordered by complexity
- No concept repetition across examples

**Code Example Tests**:

- All TypeScript code compiles
- All CDK code synthesizes
- All interfaces follow naming conventions
- All ResourceAccess uses atomic business verbs

**Diagram Tests**:

- All Mermaid diagrams render
- All diagrams have required elements
- Color coding consistent
- Alt text present

### Integration Testing

**Cross-Reference Tests**:

- Internal links resolve correctly
- Concept references valid
- Example prerequisites satisfied
- Forward references valid

**Progressive Learning Tests**:

- Concepts introduced before used
- Examples build on previous examples
- No circular dependencies
- Clear learning path

**AWS Mapping Tests**:

- All components mapped to AWS services
- Service selections justified
- No unmapped components
- Service combinations valid

## Correctness Properties

_A property is a characteristic or behavior that should hold true across all valid executions of a system—essentially, a formal statement about what the system should do. Properties serve as the bridge between human-readable specifications and machine-verifiable correctness guarantees._

For the guideline document, properties validate the structural integrity, content completeness, and consistency of the generated documentation. These properties ensure that the document meets all requirements and provides a high-quality learning experience.

### Property 1: Example Count Bounds

_For any_ generated guideline document, the number of system examples shall be at least 3 and at most 5.

**Validates: Requirements 4.1, 4.2**

### Property 2: Example Complexity Ordering

_For any_ generated guideline document with multiple examples, each example's complexity level shall be greater than or equal to the previous example's complexity level (simple < medium < complex).

**Validates: Requirements 1.4**

### Property 3: Domain Uniqueness

_For any_ generated guideline document, all system examples shall be from different business domains (no two examples share the same domain).

**Validates: Requirements 4.1**

### Property 4: Example Structure Completeness

_For any_ system example in the guideline document, it shall contain all seven required sections: business context, volatility analysis, architecture design, AWS service mapping, use case flows, implementation highlights, and lessons learned.

**Validates: Requirements 3.1, 3.2, 3.3, 3.4, 3.5, 3.6, 3.7**

### Property 5: Unique Concept Introduction

_For any_ system example in the guideline document (except the first), it shall introduce at least one concept that was not covered in any previous example.

**Validates: Requirements 4.6**

### Property 6: Volatility Mapping Completeness

_For any_ volatile area identified in a volatility analysis section, there shall be an explanation of why it's volatile and a mapping to a specific component type.

**Validates: Requirements 5.2, 5.3**

### Property 7: Architecture Diagram Completeness

_For any_ system example's architecture section, it shall include both a component diagram showing all components and their types, and a layer diagram showing the layered structure.

**Validates: Requirements 6.1, 6.2**

### Property 8: Diagram Notation Consistency

_For any_ two diagrams in the guideline document that show the same component type, they shall use the same visual notation (color, shape, icon).

**Validates: Requirements 6.3, 14.2**

### Property 9: Layer Boundary Indication

_For any_ architecture diagram in the guideline document, it shall clearly indicate layer boundaries between Client, Business Logic, Resource Access, Resource, and Utilities layers.

**Validates: Requirements 6.4**

### Property 10: AWS Service Mapping Completeness

_For any_ component in a system example, the AWS service mapping section shall specify which AWS service implements that component and explain why that service is appropriate.

**Validates: Requirements 7.1, 7.2**

### Property 11: Use Case Completeness

_For any_ use case presented in the guideline document, it shall include a business scenario description, a call chain showing component invocations, and layer identification for each component.

**Validates: Requirements 8.1, 8.2, 8.3**

### Property 12: Call Chain Layer Compliance

_For any_ call chain in the guideline document, no component shall invoke another component that is more than one layer away (no layer skipping).

**Validates: Requirements 8.5**

### Property 13: Call Chain Orchestration Pattern

_For any_ call chain that includes a Manager component, the Manager shall be shown orchestrating one or more Engine or ResourceAccess components (not calling Resources directly).

**Validates: Requirements 8.6**

### Property 14: Call Chain Sequence Clarity

_For any_ call chain in the guideline document, each component invocation shall have a clear sequence number or ordering indicator.

**Validates: Requirements 8.4**

### Property 15: Implementation Section Completeness

_For any_ system example's implementation highlights section, it shall include TypeScript code examples, component interface definitions, atomic business verbs in ResourceAccess, proper error handling, and Lambda function structure for each component type used.

**Validates: Requirements 9.1, 9.2, 9.3, 9.4, 9.6**

### Property 16: Atomic Business Verb Usage

_For any_ ResourceAccess interface shown in implementation examples, all method names shall be atomic business verbs (e.g., createOrder, markAsPaid) and not generic CRUD operations (e.g., update, delete).

**Validates: Requirements 9.3**

### Property 17: Concept Reference Traceability

_For any_ concept introduced in a later example that references an earlier concept, the document shall explicitly state which example first introduced that concept.

**Validates: Requirements 10.4**

### Property 18: Design Decision Anti-Pattern Inclusion

_For any_ design decision discussion in the guideline document, it shall include an explanation of what not to do (anti-pattern) alongside the correct approach.

**Validates: Requirements 11.1**

### Property 19: Code Repository Link Presence

_For any_ code reference in the guideline document, it shall include a GitHub repository link to the complete implementation.

**Validates: Requirements 13.2**

### Property 20: Formatting Consistency

_For any_ two occurrences of the same type of element (component names, code blocks, headings, AWS service names), they shall use the same formatting style throughout the document.

**Validates: Requirements 14.1, 14.3, 14.4, 14.5**

### Property 21: Diagram Accessibility

_For any_ diagram in the guideline document, it shall include alt text describing the diagram's content for screen readers.

**Validates: Requirements 15.4**

### Property 22: Heading Hierarchy Validity

_For any_ sequence of headings in the guideline document, no heading shall skip a level (e.g., H2 followed by H4 without an H3 in between).

**Validates: Requirements 15.5**

### Property 23: AWS Service Variety

_For any_ set of system examples in the guideline document, they shall collectively demonstrate different AWS service combinations (no two examples use the exact same set of services).

**Validates: Requirements 4.4**

### Property 24: Architectural Pattern Variety

_For any_ set of system examples in the guideline document, they shall showcase different architectural patterns (e.g., event-driven, workflow-based, real-time).

**Validates: Requirements 4.5**

### Property 25: Complexity Level Coverage

_For any_ generated guideline document, the system examples shall collectively cover at least two different complexity levels (simple, medium, or complex).

**Validates: Requirements 4.3**

## Testing Strategy

The guideline document will be validated using a dual testing approach: unit tests for specific structural requirements and property-based tests for universal correctness properties.

### Unit Testing Approach

Unit tests will verify specific examples and edge cases:

**Document Structure Tests**:

- Verify introduction section exists and contains required content (attribution, core principles, component taxonomy)
- Verify conclusion section exists and contains summary, checklist, patterns, and references
- Verify table of contents is present and matches document structure
- Verify all required sections are present in correct order

**Specific Content Tests**:

- Verify introduction explains The Method's core principles (volatility-based decomposition, component taxonomy, layered architecture)
- Verify introduction includes layered architecture diagram
- Verify introduction explains atomic business verbs
- Verify AWS mapping sections cover all component types (Manager→Step Functions/Lambda, Engine→Lambda, etc.)
- Verify conclusion includes checklist for applying The Method to AWS
- Verify anti-pattern examples are present (functional decomposition, CRUD-based ResourceAccess, layer violations)
- Verify document is in Markdown format
- Verify document is GitHub-compatible (no unsupported syntax)

**Example-Specific Tests**:

- Verify first example introduces foundational concepts
- Verify second example builds on first with new concepts
- Verify third example introduces additional concepts beyond first two
- Verify Example 1 (Task Management) uses specified AWS services
- Verify Example 2 (E-Commerce) demonstrates Step Functions workflow
- Verify Example 3 (Trading System) demonstrates event-driven architecture

**Code Example Tests**:

- Verify all TypeScript code examples compile successfully
- Verify all CDK code examples synthesize without errors
- Verify ResourceAccess interfaces use atomic business verbs (not CRUD)
- Verify implementation examples include error handling
- Verify Lambda function examples follow standard structure

**Diagram Tests**:

- Verify all Mermaid diagrams render correctly
- Verify diagrams use consistent color coding (Managers=Blue, Engines=Green, etc.)
- Verify both logical and physical deployment diagrams are present

### Property-Based Testing Approach

Property-based tests will verify universal properties across all generated content using a minimum of 100 iterations per test:

**Structural Properties** (Properties 1-4, 7, 11, 15, 21, 22):

- Generate variations of guideline documents with different example counts, orderings, and structures
- Verify example count bounds (3-5 examples)
- Verify example structure completeness (all 7 required sections)
- Verify architecture diagram completeness (component + layer diagrams)
- Verify use case completeness (scenario + call chain + layers)
- Verify implementation section completeness (TypeScript + interfaces + atomic verbs + error handling + Lambda structure)
- Verify all diagrams have alt text
- Verify heading hierarchy never skips levels

**Ordering and Uniqueness Properties** (Properties 2, 3, 5, 23, 24, 25):

- Generate documents with various example orderings and domain selections
- Verify complexity ordering (each example >= previous)
- Verify domain uniqueness (no duplicate domains)
- Verify unique concept introduction (each example adds new concepts)
- Verify AWS service variety (different service combinations)
- Verify architectural pattern variety (different patterns)
- Verify complexity level coverage (at least 2 different levels)

**Completeness Properties** (Properties 6, 10, 17, 19):

- Generate documents with various volatility analyses and mappings
- Verify volatility mapping completeness (explanation + component type for each volatile area)
- Verify AWS service mapping completeness (service + justification for each component)
- Verify concept reference traceability (later concepts reference where first introduced)
- Verify code repository links present for all code references

**Consistency Properties** (Properties 8, 9, 20):

- Generate documents with multiple diagrams and repeated elements
- Verify diagram notation consistency (same component type = same notation)
- Verify layer boundary indication in all architecture diagrams
- Verify formatting consistency (same element type = same formatting)

**Correctness Properties** (Properties 12, 13, 14, 16, 18):

- Generate documents with various call chains and implementations
- Verify call chain layer compliance (no layer skipping)
- Verify call chain orchestration pattern (Managers orchestrate Engines/ResourceAccess)
- Verify call chain sequence clarity (all invocations have sequence numbers)
- Verify atomic business verb usage (ResourceAccess uses business verbs, not CRUD)
- Verify design decision anti-pattern inclusion (all decisions show what not to do)

### Property Test Configuration

Each property-based test will:

- Run a minimum of 100 iterations with randomized inputs
- Include a comment tag referencing the design property
- Tag format: `// Feature: guideline-document, Property {number}: {property_text}`
- Generate diverse test cases covering edge cases (minimum examples, maximum examples, various complexity orderings)

### Test Data Generation

For property-based testing, generators will create:

- **Document structures**: Varying numbers of examples (1-7), different section orderings
- **Example metadata**: Different domains, complexity levels, AWS service combinations
- **Diagrams**: Various component counts, layer configurations, notation styles
- **Call chains**: Different component sequences, layer transitions
- **Code examples**: Various interface definitions, method names, error handling patterns
- **Formatting variations**: Different heading levels, code block styles, component name formats

### Integration Testing

Integration tests will verify cross-cutting concerns:

- **Internal link resolution**: All internal references resolve correctly
- **Concept dependency graph**: Concepts are introduced before they're referenced
- **Example prerequisite satisfaction**: Later examples can reference earlier concepts
- **Progressive learning path**: Clear learning progression from simple to complex
- **Cross-reference validity**: All forward and backward references are valid

### Validation Workflow

The testing workflow will follow this sequence:

1. **Structure Validation**: Run unit tests to verify basic document structure
2. **Content Validation**: Run unit tests to verify specific required content
3. **Property Validation**: Run property-based tests to verify universal properties
4. **Integration Validation**: Run integration tests to verify cross-cutting concerns
5. **Manual Review**: Human review of generated content for quality and clarity

### Test Coverage Goals

- **Structural coverage**: 100% of required sections and subsections
- **Property coverage**: All 25 correctness properties tested
- **Code coverage**: All code examples compile and follow conventions
- **Diagram coverage**: All diagrams render and follow style guide
- **Accessibility coverage**: All diagrams have alt text, all headings follow hierarchy

### Continuous Validation

As the guideline document is updated:

- Run structure and content unit tests on every change
- Run property-based tests on major updates
- Run integration tests before publishing
- Maintain test suite alongside document content
- Update tests when requirements change

This dual testing approach ensures both specific requirements are met (unit tests) and universal properties hold across all variations (property-based tests), providing comprehensive validation of the guideline document's correctness and quality.
