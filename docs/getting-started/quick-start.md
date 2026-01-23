# Quick Start

This quick start guide walks you through building a simple Task Management System using The Method on AWS.

## What You'll Build

A simple task management system with:

- Create and complete tasks
- Task priority calculation
- Task persistence in DynamoDB
- Notifications via SNS

## Architecture Overview

```
Client (Web/Mobile)
    ↓
TaskManager (Lambda)
    ↓
PriorityEngine (Lambda) + TaskDataAccess (Lambda)
    ↓
Tasks Table (DynamoDB)
```

## Step 1: Create Project Structure

```bash
mkdir task-management-system
cd task-management-system
cdk init app --language typescript
```

## Step 2: Define Components

### Manager: TaskManager

```typescript
// src/managers/task-manager.ts
export interface ITaskManager {
  createTask(description: string, dueDate: Date): Promise<string>;
  completeTask(taskId: string): Promise<void>;
}
```

### Engine: PriorityEngine

```typescript
// src/engines/priority-engine.ts
export interface IPriorityEngine {
  calculatePriority(dueDate: Date, createdDate: Date): Promise<number>;
}
```

### ResourceAccess: TaskDataAccess

```typescript
// src/resource-access/task-data-access.ts
export interface ITaskDataAccess {
  createTask(task: Task): Promise<string>;
  getTask(taskId: string): Promise<Task>;
  markTaskAsComplete(taskId: string): Promise<void>;
  // NOT: updateTask(), deleteTask()
}
```

## Step 3: Implement Components

### TaskManager Implementation

```typescript
// src/managers/task-manager.ts
import { PriorityEngine } from "../engines/priority-engine";
import { TaskDataAccess } from "../resource-access/task-data-access";

export class TaskManager implements ITaskManager {
  constructor(
    private priorityEngine: PriorityEngine,
    private taskDataAccess: TaskDataAccess,
  ) {}

  async createTask(description: string, dueDate: Date): Promise<string> {
    const createdDate = new Date();
    const priority = await this.priorityEngine.calculatePriority(
      dueDate,
      createdDate,
    );

    const task = {
      taskId: generateId(),
      description,
      dueDate,
      createdDate,
      priority,
      status: "pending",
    };

    return await this.taskDataAccess.createTask(task);
  }

  async completeTask(taskId: string): Promise<void> {
    await this.taskDataAccess.markTaskAsComplete(taskId);
  }
}
```

## Step 4: Define Infrastructure

```typescript
// lib/task-management-stack.ts
import * as cdk from "aws-cdk-lib";
import * as lambda from "aws-cdk-lib/aws-lambda";
import * as dynamodb from "aws-cdk-lib/aws-dynamodb";
import * as apigateway from "aws-cdk-lib/aws-apigateway";

export class TaskManagementStack extends cdk.Stack {
  constructor(scope: cdk.App, id: string, props?: cdk.StackProps) {
    super(scope, id, props);

    // Resource: Tasks Table
    const tasksTable = new dynamodb.Table(this, "TasksTable", {
      partitionKey: { name: "taskId", type: dynamodb.AttributeType.STRING },
      billingMode: dynamodb.BillingMode.PAY_PER_REQUEST,
    });

    // Engine: Priority Engine
    const priorityEngine = new lambda.Function(this, "PriorityEngine", {
      runtime: lambda.Runtime.NODEJS_18_X,
      handler: "priority-engine.handler",
      code: lambda.Code.fromAsset("dist/engines"),
    });

    // ResourceAccess: Task Data Access
    const taskDataAccess = new lambda.Function(this, "TaskDataAccess", {
      runtime: lambda.Runtime.NODEJS_18_X,
      handler: "task-data-access.handler",
      code: lambda.Code.fromAsset("dist/resource-access"),
      environment: {
        TASKS_TABLE: tasksTable.tableName,
      },
    });
    tasksTable.grantReadWriteData(taskDataAccess);

    // Manager: Task Manager
    const taskManager = new lambda.Function(this, "TaskManager", {
      runtime: lambda.Runtime.NODEJS_18_X,
      handler: "task-manager.handler",
      code: lambda.Code.fromAsset("dist/managers"),
      environment: {
        PRIORITY_ENGINE_ARN: priorityEngine.functionArn,
        TASK_DATA_ACCESS_ARN: taskDataAccess.functionArn,
      },
    });
    priorityEngine.grantInvoke(taskManager);
    taskDataAccess.grantInvoke(taskManager);

    // Client: API Gateway
    const api = new apigateway.RestApi(this, "TaskApi");
    const tasks = api.root.addResource("tasks");
    tasks.addMethod("POST", new apigateway.LambdaIntegration(taskManager));
  }
}
```

## Step 5: Deploy

```bash
# Build TypeScript
npm run build

# Deploy to AWS
cdk deploy
```

## Step 6: Test

```bash
# Create a task
curl -X POST https://YOUR-API-ID.execute-api.REGION.amazonaws.com/prod/tasks \
  -H "Content-Type: application/json" \
  -d '{"description": "Learn The Method", "dueDate": "2024-12-31"}'
```

## Key Takeaways

1. **Volatility Encapsulation**: Each component encapsulates a specific type of change
   - TaskManager: Use case workflow
   - PriorityEngine: Priority calculation rules
   - TaskDataAccess: Data access patterns

2. **Atomic Business Verbs**: TaskDataAccess uses `createTask()` and `markTaskAsComplete()`, not generic CRUD

3. **Layer Communication**: Client → Manager → Engine/ResourceAccess → Resource

4. **AWS Service Mapping**:
   - Manager → Lambda
   - Engine → Lambda
   - ResourceAccess → Lambda
   - Resource → DynamoDB

## Next Steps

- **[Task Management Example](../examples/task-management.md)** - Complete system design
- **[Core Principles](../guidelines/core-principles.md)** - Deep dive into The Method
- **[E-Commerce Example](../examples/e-commerce.md)** - More complex system
