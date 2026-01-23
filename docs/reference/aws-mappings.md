# AWS Service Mappings

Complete reference for mapping The Method components to AWS services.

_This reference will be fully developed in the implementation phase._

## Component → Service Mapping

| Component Type | AWS Services            | Use When               |
| -------------- | ----------------------- | ---------------------- |
| Manager        | Step Functions, Lambda  | Workflow orchestration |
| Engine         | Lambda                  | Business logic         |
| ResourceAccess | Lambda                  | Data access            |
| Resource       | DynamoDB, S3, RDS, SQS  | Storage, queues        |
| Utility        | CloudWatch, X-Ray, SNS  | Cross-cutting concerns |
| Client         | API Gateway, CloudFront | User interfaces        |

## Coming Soon

Detailed mapping guide with decision trees and examples.
