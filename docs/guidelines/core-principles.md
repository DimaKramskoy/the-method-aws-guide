# Core Principles

Let's talk about the three big ideas that make The Method work. These principles come from Juval Löwy's "Righting Software," and they're the foundation for everything we'll build with AWS.

## 1. Volatility-Based Decomposition

### The Big Idea (From Löwy)

Here's how most people design software: they break it into pieces based on what the system does. User management here, order processing there, payment handling over there. This is called **functional decomposition**, and as Löwy explains, it's the wrong approach.

The Method uses **volatility-based decomposition** instead. You break your system into pieces based on **what changes**, not what it does.

### Why This Matters

Think about it: when your boss comes to you with new requirements (and they always do), what happens?

With functional decomposition, that change ripples through multiple modules. You're touching code everywhere, breaking things, creating bugs.

With volatility-based decomposition, as Löwy describes it, you identify areas of potential change and encapsulate those into services or building blocks. When change comes, you open one "vault," make your changes, close it up, and you're done. The rest of your system doesn't even know anything happened.

### How to Spot Volatility

Löwy teaches us to ask: "What aspects of this system are likely to change?"

Here's what we've found maps well to AWS:

- **Use case sequences** change → Manager component
- **Business rules** change → Engine component
- **Data access patterns** change → ResourceAccess component
- **Resource types** change → Resource component

### Example: E-Commerce Order Processing

Let me show you the difference:

**Functional Decomposition (❌ The Old Way)**:

```
OrderService
├── createOrder()
├── updateOrder()
├── calculatePrice()
├── processPayment()
└── updateInventory()
```

Everything's mixed together. When pricing rules change, you're digging through OrderService. When payment methods change, same thing. It's a mess.

**Volatility-Based Decomposition (✅ The Method's Way)**:

```
OrderProcessingManager (use case workflow changes)
├── calls → PricingEngine (pricing rules change)
├── calls → PaymentEngine (payment methods change)
├── calls → InventoryEngine (inventory rules change)
└── calls → OrderDataAccess (data access changes)
```

Now each area of change is isolated. Pricing rules change? You only touch PricingEngine. Payment methods change? Just PaymentEngine. See the difference?

### How We Map This to AWS

Here's our interpretation for AWS services:

- **Manager** (workflow volatility) → Step Functions or Lambda
- **Engine** (business rule volatility) → Lambda
- **ResourceAccess** (access pattern volatility) → Lambda with AWS SDK
- **Resource** (storage volatility) → DynamoDB, S3, RDS

We'll see why these mappings make sense as we go through examples.

## 2. Component Taxonomy

### The Six Component Types (From Löwy)

The Method defines six types of components. Each has a specific job, and mixing them up causes problems. Here's the taxonomy Löwy established:

| Component          | Responsibility         | Changes When                  |
| ------------------ | ---------------------- | ----------------------------- |
| **Manager**        | Orchestrate use cases  | Use case sequence changes     |
| **Engine**         | Execute business logic | Business rules change         |
| **ResourceAccess** | Access resources       | Access patterns change        |
| **Resource**       | Store data             | Storage requirements change   |
| **Utility**        | Provide infrastructure | Cross-cutting concerns change |
| **Client**         | Present UI             | User interface changes        |

### What Each Component Does

**Managers** orchestrate workflows. They're like conductors—they don't play instruments, they coordinate everyone else. In AWS, we implement these as Step Functions (for complex workflows) or Lambda functions (for simple ones).

**Engines** contain pure business logic. No database calls, no API calls, just rules and calculations. They're stateless and reusable. In AWS, these are Lambda functions that focus solely on business logic.

**ResourceAccess** components talk to databases, queues, and storage. But here's the key insight from Löwy: they expose **atomic business verbs**, not generic CRUD operations. You don't have `updateRecord()`, you have `markOrderAsPaid()` or `cancelOrder()`. This is huge for maintainability.

**Resources** are your actual databases, queues, and storage. In AWS, these are DynamoDB tables, S3 buckets, SQS queues, etc.

**Utilities** provide cross-cutting concerns like logging, monitoring, and security. In AWS, these are CloudWatch, X-Ray, Secrets Manager, etc.

**Clients** are what users interact with—web apps, mobile apps, APIs. In AWS, these might be hosted on S3+CloudFront, Amplify, or external systems.

### How We Implement This in AWS

Here's how we interpret these components for AWS:

```typescript
// Manager → Step Functions (complex workflow)
const orderWorkflow = new sfn.StateMachine(this, "OrderProcessing", {
  definition: validateStep.next(paymentStep).next(inventoryStep),
});

// Manager → Lambda (simple workflow)
const taskManager = new lambda.Function(this, "TaskManager", {
  handler: "task-manager.handler",
});

// Engine → Lambda (business logic)
const pricingEngine = new lambda.Function(this, "PricingEngine", {
  handler: "pricing-engine.handler",
});

// ResourceAccess → Lambda (data access)
const orderDataAccess = new lambda.Function(this, "OrderDataAccess", {
  handler: "order-data-access.handler",
});

// Resource → DynamoDB (data storage)
const ordersTable = new dynamodb.Table(this, "Orders", {
  partitionKey: { name: "orderId", type: dynamodb.AttributeType.STRING },
});
```

## 3. Layered Architecture

### How Components Are Organized (From Löwy)

The Method organizes components into layers with strict communication rules. As Löwy explains, this prevents the tangled mess that most systems become.

Here's the structure:

```
┌─────────────────────────────────────┐
│         Client Layer                │  ← Users interact here
└─────────────────────────────────────┘
              ↓ (calls)
┌─────────────────────────────────────┐
│    Business Logic Layer             │  ← Managers orchestrate
│    (Managers + Engines)             │  ← Engines execute logic
└─────────────────────────────────────┘
              ↓ (calls)
┌─────────────────────────────────────┐
│    Resource Access Layer            │  ← Atomic operations
└─────────────────────────────────────┘
              ↓ (calls)
┌─────────────────────────────────────┐
│         Resource Layer              │  ← Physical storage
└─────────────────────────────────────┘

        Utilities Bar (available to all)
┌─────────────────────────────────────┐
│  Logging, Monitoring, Security      │
└─────────────────────────────────────┘
```

### The Rules

**You can call down, but never up.** Clients call Managers. Managers call Engines and ResourceAccess. ResourceAccess calls Resources. That's it.

**Allowed**:

- Client → Manager ✅
- Manager → Engine ✅
- Manager → ResourceAccess ✅
- Engine → ResourceAccess ✅
- Any layer → Utility ✅

**Forbidden**:

- Client → Engine ❌ (skip Manager)
- Client → ResourceAccess ❌ (skip Manager)
- Manager → Resource ❌ (skip ResourceAccess)
- Engine → Resource ❌ (skip ResourceAccess)
- Reverse dependencies ❌ (lower layer calling higher layer)

### Why These Rules Matter

When you skip layers, you create tight coupling. Your Client becomes dependent on Engine implementation details. When the Engine changes, your Client breaks. The layers protect you from this.

### How This Looks in AWS

Here's our interpretation for AWS:

```typescript
// ✅ CORRECT: Proper layer communication
export class OrderManager {
  async processOrder(orderId: string): Promise<void> {
    // Manager calls Engine
    const price = await this.pricingEngine.calculatePrice(orderId);

    // Manager calls ResourceAccess
    await this.orderDataAccess.updateOrderPrice(orderId, price);
  }
}

// ❌ WRONG: Layer violation
export class OrderManager {
  async processOrder(orderId: string): Promise<void> {
    // Manager calling Resource directly (skipping ResourceAccess)
    await dynamodb.putItem({ TableName: 'Orders', ... });
  }
}
```

See the difference? In the correct version, the Manager doesn't know anything about DynamoDB. It just calls ResourceAccess. If you switch from DynamoDB to RDS tomorrow, the Manager doesn't change at all.

### AWS API Gateway + Lambda Pattern

Here's how we typically set this up in AWS:

```typescript
// Client Layer: API Gateway
const api = new apigateway.RestApi(this, "Api");

// Business Logic Layer: Manager Lambda
const orderManager = new lambda.Function(this, "OrderManager", {
  handler: "order-manager.handler",
  environment: {
    PRICING_ENGINE_ARN: pricingEngine.functionArn,
    ORDER_DATA_ACCESS_ARN: orderDataAccess.functionArn,
  },
});

// API Gateway calls Manager (correct layer communication)
api.root
  .addResource("orders")
  .addMethod("POST", new apigateway.LambdaIntegration(orderManager));
```

## Putting It All Together

### The Design Process

When we apply The Method to AWS, here's the process we follow:

1. **Identify Volatility**: What aspects will change? (Löwy's key question)
2. **Map to Components**: Which component type for each volatility? (Using Löwy's taxonomy)
3. **Organize in Layers**: Which layer does each component belong to? (Following Löwy's architecture)
4. **Select AWS Services**: Which AWS service for each component? (Our interpretation)
5. **Define Interfaces**: What operations does each component expose?

### Quick Example: Task Management System

Let's walk through a simple example:

**Step 1: Identify Volatility**

- Task workflow changes → TaskManager
- Priority calculation changes → PriorityEngine
- Task storage changes → TaskDataAccess

**Step 2: Map to Components**

- TaskManager (Manager)
- PriorityEngine (Engine)
- TaskDataAccess (ResourceAccess)
- Tasks table (Resource)

**Step 3: Organize in Layers**

- Client Layer: Web UI
- Business Logic: TaskManager, PriorityEngine
- Resource Access: TaskDataAccess
- Resource: DynamoDB table

**Step 4: Select AWS Services** (Our interpretation)

- TaskManager → Lambda
- PriorityEngine → Lambda
- TaskDataAccess → Lambda
- Tasks → DynamoDB

**Step 5: Define Interfaces**

```typescript
interface ITaskManager {
  createTask(description: string, dueDate: Date): Promise<string>;
  completeTask(taskId: string): Promise<void>;
}

interface IPriorityEngine {
  calculatePriority(dueDate: Date, createdDate: Date): Promise<number>;
}

interface ITaskDataAccess {
  createTask(task: Task): Promise<string>;
  markTaskAsComplete(taskId: string): Promise<void>;
  // Note: atomic business verbs, not updateTask()
}
```

## What's Next

Now that you understand the core principles, let's dive deeper into each component type and see complete examples.

- **[Component Taxonomy](component-taxonomy.md)** - Detailed guide for each component type
- **[Task Management Example](../examples/task-management.md)** - See these principles in action
- **[AWS Mappings](../reference/aws-mappings.md)** - Complete service mapping reference
