# Component Taxonomy

Detailed guide for each component type in The Method.

## Overview

The Method defines six component types. This page provides detailed guidance for each type, including characteristics, responsibilities, AWS mappings, and code examples.

## Manager Components

### Purpose

Managers encapsulate volatility in use case sequences and workflows.

### Characteristics

- Orchestrate business operations
- Call Engines and ResourceAccess
- Stateless
- No business logic
- One per use case or workflow

### AWS Mapping

- **Step Functions** for complex workflows with multiple steps
- **Lambda** for simple workflows

### When to Use

- Use case sequence might change
- Multiple steps need orchestration
- Workflow involves multiple Engines or ResourceAccess

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

### Purpose

Engines encapsulate volatility in business activities and rules.

### Characteristics

- Pure business logic
- Stateless
- Reusable across Managers
- No I/O operations
- Deterministic

### AWS Mapping

- **Lambda** functions

### When to Use

- Business rules might change
- Logic needs to be reused
- Calculation or validation required

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

### Purpose

ResourceAccess components encapsulate volatility in accessing resources.

### Characteristics

- Expose atomic business verbs
- One per resource type
- Encapsulate data access patterns
- NOT generic CRUD

### AWS Mapping

- **Lambda** functions with AWS SDK

### When to Use

- Data access patterns might change
- Need abstraction over storage
- Multiple Managers/Engines access same resource

### Atomic Business Verbs

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

### Purpose

Resources are the actual physical storage and infrastructure.

### Characteristics

- Databases, queues, storage buckets
- Managed by cloud provider
- No custom code
- Configuration only

### AWS Mapping

- **DynamoDB** for NoSQL databases
- **RDS/Aurora** for relational databases
- **S3** for object storage
- **SQS** for message queues
- **SNS** for pub/sub messaging

### When to Use

- Need to store data
- Need message queues
- Need file storage

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

### Purpose

Utilities provide cross-cutting infrastructure services.

### Characteristics

- Available to all layers
- Logging, monitoring, security
- Messaging, configuration
- No business logic

### AWS Mapping

- **CloudWatch** for logging and monitoring
- **X-Ray** for distributed tracing
- **SNS** for notifications
- **EventBridge** for event routing
- **Secrets Manager** for secrets
- **IAM** for security

### When to Use

- Need logging or monitoring
- Need to send notifications
- Need distributed tracing
- Need secrets management

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

### Purpose

Clients are user-facing applications and interfaces.

### Characteristics

- Present UI to users
- Call Managers (not Engines or ResourceAccess)
- Web, mobile, CLI, API

### AWS Mapping

- **CloudFront + S3** for static web apps
- **Amplify** for full-stack apps
- **API Gateway** for REST/HTTP APIs

### When to Use

- Need user interface
- Need external API
- Need partner integration

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

Use this decision tree to select the right component type:

```
Does it orchestrate a workflow?
├─ Yes → Manager
└─ No
    ├─ Does it contain business logic?
    │   ├─ Yes → Engine
    │   └─ No
    │       ├─ Does it access a resource?
    │       │   ├─ Yes → ResourceAccess
    │       │   └─ No
    │       │       ├─ Is it storage/queue/database?
    │       │       │   ├─ Yes → Resource
    │       │       │   └─ No
    │       │       │       ├─ Is it cross-cutting infrastructure?
    │       │       │       │   ├─ Yes → Utility
    │       │       │       │   └─ No → Client
```

## Next Steps

- **[Examples](../examples/task-management.md)** - See components in complete systems
- **[AWS Mappings](../reference/aws-mappings.md)** - Detailed service mapping guide
- **[Best Practices](../reference/best-practices.md)** - Component design best practices
