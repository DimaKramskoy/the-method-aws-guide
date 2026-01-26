# Component Taxonomy

The Method, as defined by Juval Löwy in "Righting Software," establishes a precise taxonomy of six component types. Each type serves a specific purpose in encapsulating different forms of volatility. This section provides detailed guidance for each component type, including their characteristics, responsibilities, AWS service mappings, and implementation patterns.

## Overview

Löwy's component taxonomy addresses a fundamental challenge in software design: how to organize system building blocks to maximize resilience to change. The taxonomy defines six distinct component types, each designed to encapsulate a specific form of volatility:

- **Managers** - Encapsulate use case sequence volatility
- **Engines** - Encapsulate business rule volatility
- **ResourceAccess** - Encapsulate data access pattern volatility
- **Resources** - Physical storage and infrastructure
- **Utilities** - Cross-cutting infrastructure services
- **Clients** - User-facing interfaces and applications

The precision of this taxonomy is critical. Mixing component responsibilities—such as placing business logic in a Manager or data access in an Engine—undermines the volatility encapsulation that makes systems maintainable.

## Manager Components

### Purpose and Volatility

As Löwy explains, Managers encapsulate volatility in use case sequences and workflows. When the steps of a business process change—adding new validation, reordering operations, introducing conditional logic—these changes remain isolated within the Manager component.

Managers orchestrate but do not execute. They coordinate Engines and ResourceAccess components to fulfill use cases, but contain no business logic themselves. This separation ensures that changes to business rules (Engine volatility) or data access patterns (ResourceAccess volatility) do not affect workflow orchestration.

### Characteristics

- Orchestrate business operations by coordinating other components
- Stateless—maintain no state between invocations
- Contain no business logic—delegate to Engines
- Perform no I/O operations—delegate to ResourceAccess
- One Manager per use case or workflow

### AWS Service Mapping

In our AWS interpretation:

- **Step Functions** for complex workflows requiring multiple steps, error handling, and state management
- **Lambda** for simple workflows with straightforward orchestration

The choice between Step Functions and Lambda depends on workflow complexity. Step Functions provides visual workflow definition, built-in error handling, and state management. Lambda suffices for simple orchestration where these features are unnecessary.

### Example

```typescript
// Manager interface
interface IOrderProcessingManager {
  processOrder(orderId: string): Promise<OrderResult>;
}

// Manager implementation
export class OrderProcessingManager implements IOrderProcessingManager {
  constructor(
    private validationEngine: IValidationEngine,
    private pricingEngine: IPricingEngine,
    private paymentEngine: IPaymentEngine,
    private orderDataAccess: IOrderDataAccess,
  ) {}

  async processOrder(orderId: string): Promise<OrderResult> {
    // Orchestrate workflow
    await this.validationEngine.validateOrder(orderId);
    const price = await this.pricingEngine.calculatePrice(orderId);
    await this.paymentEngine.processPayment(orderId, price);
    await this.orderDataAccess.markOrderAsPaid(orderId);

    return { orderId, status: "processed" };
  }
}
```

## Engine Components

### Purpose and Volatility

Engines encapsulate volatility in business activities and rules. As Löwy emphasizes, business rules change frequently—pricing algorithms evolve, validation criteria adjust, calculation methods refine. Engines isolate these changes, ensuring that rule modifications do not ripple through the system.

The defining characteristic of an Engine is purity: no I/O operations, no external dependencies, only business logic. This purity enables testing, reusability, and parallel execution. An Engine receives inputs, applies business rules, and returns outputs—nothing more.

### Characteristics

- Pure business logic with no side effects
- Stateless—deterministic outputs for given inputs
- Reusable across multiple Managers
- No I/O operations—no database calls, no API calls, no file access
- Testable in isolation without mocks or stubs

### AWS Service Mapping

In our AWS interpretation, Engines are implemented as **Lambda functions** focused exclusively on business logic. The Lambda execution model aligns well with Engine characteristics: stateless, event-driven, and independently scalable.

### Example

```typescript
// Engine interface
interface IPricingEngine {
  calculatePrice(items: Item[], customer: Customer): Promise<Price>;
}

// Engine implementation
export class PricingEngine implements IPricingEngine {
  async calculatePrice(items: Item[], customer: Customer): Promise<Price> {
    // Pure business logic
    const basePrice = items.reduce((sum, item) => sum + item.price, 0);
    const tax = this.calculateTax(basePrice, customer.location);
    const discount = this.applyCustomerDiscount(basePrice, customer.tier);

    return {
      base: basePrice,
      tax,
      discount,
      final: basePrice + tax - discount,
    };
  }

  private calculateTax(amount: number, location: string): number {
    // Tax calculation logic
  }

  private applyCustomerDiscount(amount: number, tier: string): number {
    // Discount logic
  }
}
```

## ResourceAccess Components

### Purpose and Volatility

ResourceAccess components encapsulate volatility in data access patterns. As Löwy explains, how you access data changes independently of what data you access. Database schemas evolve, query patterns optimize, storage technologies migrate. ResourceAccess components isolate these changes from business logic.

The critical insight Löwy provides is the concept of **atomic business verbs**. ResourceAccess components do not expose generic CRUD operations (create, read, update, delete). Instead, they expose operations that reflect business intent: `markOrderAsPaid()`, `cancelSubscription()`, `approveTimesheet()`. This approach encapsulates not just the data access mechanism but also the business semantics of data modifications.

### Characteristics

- Expose atomic business verbs, not generic CRUD
- One ResourceAccess component per resource type
- Encapsulate all data access patterns for a resource
- Translate business operations into storage operations
- Isolate business logic from storage implementation details

### AWS Service Mapping

In our AWS interpretation, ResourceAccess components are implemented as **Lambda functions** that use the AWS SDK to interact with storage services. Each ResourceAccess Lambda encapsulates all access patterns for a specific resource (e.g., OrderDataAccess for the Orders table).

### The Atomic Business Verb Principle

This principle, central to The Method, deserves emphasis. Consider the difference:

❌ **WRONG: Generic CRUD**

```typescript
interface IOrderDataAccess {
  create(order: Order): Promise<string>;
  read(orderId: string): Promise<Order>;
  update(orderId: string, data: Partial<Order>): Promise<void>;
  delete(orderId: string): Promise<void>;
}
```

✅ **CORRECT: Atomic Business Verbs**

```typescript
interface IOrderDataAccess {
  createOrder(order: Order): Promise<string>;
  getOrder(orderId: string): Promise<Order>;
  markOrderAsPaid(orderId: string): Promise<void>;
  markOrderAsShipped(orderId: string, trackingNumber: string): Promise<void>;
  cancelOrder(orderId: string, reason: string): Promise<void>;
}
```

### Example Implementation

```typescript
export class OrderDataAccess implements IOrderDataAccess {
  constructor(private dynamodb: DynamoDBClient) {}

  async createOrder(order: Order): Promise<string> {
    await this.dynamodb.send(
      new PutItemCommand({
        TableName: "Orders",
        Item: marshall(order),
        ConditionExpression: "attribute_not_exists(orderId)",
      }),
    );
    return order.orderId;
  }

  async markOrderAsPaid(orderId: string): Promise<void> {
    await this.dynamodb.send(
      new UpdateItemCommand({
        TableName: "Orders",
        Key: marshall({ orderId }),
        UpdateExpression: "SET #status = :paid, paidAt = :timestamp",
        ExpressionAttributeNames: { "#status": "status" },
        ExpressionAttributeValues: marshall({
          ":paid": "paid",
          ":timestamp": new Date().toISOString(),
        }),
      }),
    );
  }
}
```

## Resource Components

### Purpose and Nature

Resources represent the physical infrastructure—databases, storage, queues, and other managed services. Unlike the other component types, Resources contain no custom code. They are purely configuration, defining the storage and messaging infrastructure that ResourceAccess components interact with.

As Löwy explains, Resources encapsulate storage volatility. When storage requirements change—migrating from one database technology to another, adjusting capacity, modifying retention policies—these changes remain isolated within Resource configuration. The ResourceAccess components that interact with Resources remain unchanged, as they expose business operations rather than storage-specific operations.

### Characteristics

- Pure infrastructure—no custom code
- Managed by cloud provider
- Configuration-only definitions
- Encapsulate storage technology choices
- Accessed exclusively through ResourceAccess components

### AWS Service Mapping

In our AWS interpretation, Resources map to managed storage and messaging services:

- **DynamoDB** - NoSQL database for high-scale, key-value access patterns
- **RDS/Aurora** - Relational databases for complex queries and transactions
- **S3** - Object storage for files, documents, and large binary data
- **SQS** - Message queues for asynchronous processing
- **SNS** - Pub/sub messaging for event distribution
- **Kinesis** - Stream processing for real-time data flows

The choice of Resource type depends on access patterns, consistency requirements, and scale characteristics—not on functional decomposition.

### Example

```typescript
// DynamoDB table
const ordersTable = new dynamodb.Table(this, "OrdersTable", {
  partitionKey: { name: "orderId", type: dynamodb.AttributeType.STRING },
  sortKey: { name: "createdAt", type: dynamodb.AttributeType.STRING },
  billingMode: dynamodb.BillingMode.PAY_PER_REQUEST,
  pointInTimeRecovery: true,
});

// S3 bucket
const documentsBucket = new s3.Bucket(this, "DocumentsBucket", {
  encryption: s3.BucketEncryption.S3_MANAGED,
  versioned: true,
});

// SQS queue
const orderQueue = new sqs.Queue(this, "OrderQueue", {
  visibilityTimeout: cdk.Duration.seconds(300),
  retentionPeriod: cdk.Duration.days(14),
});
```

## Utility Components

### Purpose and Cross-Cutting Concerns

Utilities provide infrastructure services that span all layers—logging, monitoring, security, messaging, and configuration. As Löwy explains, Utilities represent cross-cutting concerns that every component may need but that don't belong to any specific layer.

The key characteristic of Utilities is their universal availability. Any component—Manager, Engine, ResourceAccess—can use Utilities without violating layer communication rules. This exception to the layered architecture reflects the reality that logging, monitoring, and security are pervasive concerns that cannot be confined to a single layer.

### Characteristics

- Available to all layers without restriction
- Provide infrastructure services, not business logic
- Stateless and reusable
- No dependencies on business components
- Examples: logging, monitoring, security, messaging, configuration

### AWS Service Mapping

In our AWS interpretation, Utilities map to AWS infrastructure services:

- **CloudWatch Logs** - Centralized logging for all components
- **CloudWatch Metrics** - Performance monitoring and alerting
- **X-Ray** - Distributed tracing across service boundaries
- **SNS** - Notification delivery for alerts and events
- **EventBridge** - Event routing and filtering
- **Secrets Manager** - Secure credential and configuration storage
- **IAM** - Authentication and authorization
- **Systems Manager Parameter Store** - Configuration management

These services provide the infrastructure foundation that enables the business logic components to function.

### Example

```typescript
// Logging utility
export class LoggingUtility {
  log(level: string, message: string, context: any): void {
    console.log(
      JSON.stringify({
        timestamp: new Date().toISOString(),
        level,
        message,
        context,
      }),
    );
  }
}

// Notification utility
export class NotificationUtility {
  constructor(private sns: SNSClient) {}

  async sendNotification(topic: string, message: string): Promise<void> {
    await this.sns.send(
      new PublishCommand({
        TopicArn: topic,
        Message: message,
      }),
    );
  }
}
```

## Client Components

### Purpose and User Interface Volatility

Clients represent user-facing applications and interfaces—web applications, mobile apps, command-line tools, and external APIs. As Löwy explains, Clients encapsulate user interface volatility. When presentation requirements change—adding a mobile app alongside a web interface, introducing a partner API, redesigning the user experience—these changes remain isolated within Client components.

The critical architectural rule for Clients is that they call only Managers, never Engines or ResourceAccess directly. This constraint ensures that business logic and data access remain encapsulated, preventing UI concerns from leaking into business logic layers.

### Characteristics

- Present user interfaces and APIs
- Call only Managers—never Engines or ResourceAccess
- Encapsulate presentation logic and user interaction
- Multiple Clients can coexist (web, mobile, API)
- No business logic—delegate to Managers

### AWS Service Mapping

In our AWS interpretation, Clients map to various AWS frontend and API services:

- **CloudFront + S3** - Static web applications (React, Vue, Angular)
- **Amplify** - Full-stack web and mobile applications with backend integration
- **API Gateway** - REST and HTTP APIs for external consumers
- **AppSync** - GraphQL APIs with real-time capabilities
- **Elastic Beanstalk** - Server-rendered web applications

The choice depends on client requirements: static vs. dynamic, web vs. mobile, REST vs. GraphQL.

### Example

```typescript
// API Gateway as client entry point
const api = new apigateway.RestApi(this, "ClientApi", {
  restApiName: "Task Management API",
});

// Client calls Manager
const tasks = api.root.addResource("tasks");
tasks.addMethod("POST", new apigateway.LambdaIntegration(taskManager));
tasks.addMethod("GET", new apigateway.LambdaIntegration(taskManager));

const task = tasks.addResource("{taskId}");
task.addMethod("PUT", new apigateway.LambdaIntegration(taskManager));
```

## Component Selection Guide

Selecting the appropriate component type is fundamental to applying The Method correctly. Use this decision process:

**1. Does it orchestrate a workflow or use case?**

- Yes → **Manager**
- No → Continue

**2. Does it contain business rules or calculations?**

- Yes → **Engine**
- No → Continue

**3. Does it access a database, queue, or storage?**

- Yes → **ResourceAccess**
- No → Continue

**4. Is it physical storage or infrastructure?**

- Yes → **Resource**
- No → Continue

**5. Is it a cross-cutting infrastructure service?**

- Yes → **Utility**
- No → **Client**

### Common Mistakes

**Mixing Manager and Engine responsibilities**: Placing business logic in a Manager violates the separation of concerns. Managers orchestrate; Engines execute logic.

**Generic CRUD in ResourceAccess**: Exposing `update()` or `delete()` instead of atomic business verbs like `markOrderAsPaid()` or `cancelSubscription()` loses business semantics.

**Clients calling Engines directly**: Skipping the Manager layer creates tight coupling between UI and business logic, making both harder to change.

**Business logic in Utilities**: Utilities provide infrastructure, not business logic. If it contains business rules, it belongs in an Engine.

## Next Steps

- **[Examples](../examples/task-management.md)** - See components in complete systems
- **[AWS Mappings](../reference/aws-mappings.md)** - Detailed service mapping guide
- **[Best Practices](../reference/best-practices.md)** - Component design best practices
