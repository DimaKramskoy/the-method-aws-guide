# Requirements Document: Main Guideline Document

## Introduction

This document defines the requirements for the main guideline document that demonstrates how to apply The Method from "Righting Software" to AWS serverless services. The guideline will teach through 3-5 complete system examples from different domains, showing how proven architectural principles naturally align with modern cloud development.

## Glossary

- **The Method**: Juval Löwy's structured engineering approach to system and project design from "Righting Software"
- **Guideline Document**: The main educational document that walks through system examples
- **System Example**: A complete architectural design for a specific business domain
- **Component**: A building block in The Method's taxonomy (Manager, Engine, ResourceAccess, Resource, Utility, Client)
- **Volatility**: An area of potential change in requirements or implementation
- **AWS Serverless**: AWS services that abstract server management (Lambda, Step Functions, DynamoDB, etc.)
- **Atomic Business Verb**: Indivisible business operation exposed by ResourceAccess components

## Requirements

### Requirement 1: Document Structure and Organization

**User Story:** As a reader, I want a well-organized guideline document, so that I can progressively learn how to apply The Method to AWS services.

#### Acceptance Criteria

1. THE Guideline_Document SHALL contain an introduction section explaining The Method and its relevance to cloud development
2. THE Guideline_Document SHALL include proper attribution to "Righting Software" by Juval Löwy
3. THE Guideline_Document SHALL contain 3-5 complete system examples from different domains
4. THE Guideline_Document SHALL organize examples in order of increasing complexity
5. THE Guideline_Document SHALL include a conclusion section summarizing key patterns and best practices
6. WHEN a reader navigates the document THEN THE Guideline_Document SHALL provide clear section headings and table of contents

### Requirement 2: Introduction and Foundation

**User Story:** As a reader new to The Method, I want a clear introduction, so that I understand the core principles before seeing examples.

#### Acceptance Criteria

1. THE Introduction SHALL explain the core principles of The Method (volatility-based decomposition, component taxonomy, layered architecture)
2. THE Introduction SHALL describe the natural parallels between The Method and AWS serverless services
3. THE Introduction SHALL explain why The Method is relevant to modern cloud development
4. THE Introduction SHALL define the component taxonomy (Manager, Engine, ResourceAccess, Resource, Utility, Client)
5. THE Introduction SHALL include a visual diagram showing the layered architecture
6. THE Introduction SHALL explain the concept of atomic business verbs

### Requirement 3: System Example Structure

**User Story:** As a reader, I want each system example to follow a consistent structure, so that I can easily compare approaches across different domains.

#### Acceptance Criteria

1. WHEN presenting a system example THEN THE Guideline_Document SHALL include a business context section
2. WHEN presenting a system example THEN THE Guideline_Document SHALL include a volatility analysis section
3. WHEN presenting a system example THEN THE Guideline_Document SHALL include an architecture design section with component diagrams
4. WHEN presenting a system example THEN THE Guideline_Document SHALL include an AWS service mapping section
5. WHEN presenting a system example THEN THE Guideline_Document SHALL include use case flows and call chains
6. WHEN presenting a system example THEN THE Guideline_Document SHALL include implementation highlights showing key code patterns
7. WHEN presenting a system example THEN THE Guideline_Document SHALL include a lessons learned section

### Requirement 4: Domain Coverage

**User Story:** As a reader, I want examples from diverse domains, so that I can see how The Method applies to different types of systems.

#### Acceptance Criteria

1. THE Guideline_Document SHALL include at least 3 system examples from different business domains
2. THE Guideline_Document SHALL include at most 5 system examples to maintain focus
3. THE System_Examples SHALL cover different complexity levels (simple, medium, complex)
4. THE System_Examples SHALL demonstrate different AWS service combinations
5. THE System_Examples SHALL showcase different architectural patterns (event-driven, workflow-based, real-time)
6. WHERE a system example is included THEN it SHALL teach at least one unique concept not covered in previous examples

### Requirement 5: Volatility Analysis

**User Story:** As a reader, I want to see detailed volatility analysis for each example, so that I understand how to identify and encapsulate areas of change.

#### Acceptance Criteria

1. WHEN analyzing volatility THEN THE Guideline_Document SHALL identify all major areas of potential change
2. WHEN analyzing volatility THEN THE Guideline_Document SHALL explain why each area is considered volatile
3. WHEN analyzing volatility THEN THE Guideline_Document SHALL show how volatility maps to component types
4. WHEN analyzing volatility THEN THE Guideline_Document SHALL distinguish between volatile and variable aspects
5. THE Volatility_Analysis SHALL use the axes of volatility concept from The Method

### Requirement 6: Architecture Diagrams

**User Story:** As a reader, I want clear architecture diagrams, so that I can visualize the system structure and component relationships.

#### Acceptance Criteria

1. WHEN showing system architecture THEN THE Guideline_Document SHALL include a component diagram showing all components and their types
2. WHEN showing system architecture THEN THE Guideline_Document SHALL include a layer diagram showing the layered structure
3. WHEN showing system architecture THEN THE Guideline_Document SHALL use consistent notation for component types
4. WHEN showing system architecture THEN THE Guideline_Document SHALL clearly indicate layer boundaries
5. THE Architecture_Diagrams SHALL include AWS service icons where appropriate
6. THE Architecture_Diagrams SHALL show both logical architecture and physical AWS deployment

### Requirement 7: AWS Service Mapping

**User Story:** As a reader, I want explicit mapping between The Method components and AWS services, so that I know which services to use for each component type.

#### Acceptance Criteria

1. WHEN mapping to AWS services THEN THE Guideline_Document SHALL specify which AWS service implements each component
2. WHEN mapping to AWS services THEN THE Guideline_Document SHALL explain why that service is appropriate for the component type
3. THE AWS_Mapping SHALL show how Managers map to Step Functions or Lambda
4. THE AWS_Mapping SHALL show how Engines map to Lambda functions
5. THE AWS_Mapping SHALL show how ResourceAccess maps to Lambda with AWS SDK
6. THE AWS_Mapping SHALL show how Resources map to DynamoDB, S3, SQS, etc.
7. THE AWS_Mapping SHALL show how Utilities map to CloudWatch, X-Ray, EventBridge, etc.

### Requirement 8: Use Cases and Call Chains

**User Story:** As a reader, I want to see how use cases flow through the system, so that I understand the runtime behavior and component interactions.

#### Acceptance Criteria

1. WHEN presenting a use case THEN THE Guideline_Document SHALL describe the business scenario
2. WHEN presenting a use case THEN THE Guideline_Document SHALL show the call chain through components
3. WHEN presenting a use case THEN THE Guideline_Document SHALL indicate which layer each component belongs to
4. THE Call_Chains SHALL show the sequence of component invocations
5. THE Call_Chains SHALL demonstrate proper layer communication (no layer skipping)
6. THE Call_Chains SHALL show how Managers orchestrate Engines and ResourceAccess

### Requirement 9: Implementation Highlights

**User Story:** As a reader, I want to see key code patterns, so that I understand how to implement The Method components in TypeScript.

#### Acceptance Criteria

1. WHEN showing implementation THEN THE Guideline_Document SHALL include TypeScript code examples
2. WHEN showing implementation THEN THE Guideline_Document SHALL demonstrate component interface definitions
3. WHEN showing implementation THEN THE Guideline_Document SHALL show atomic business verbs in ResourceAccess
4. WHEN showing implementation THEN THE Guideline_Document SHALL demonstrate proper error handling
5. THE Implementation_Highlights SHALL include CDK infrastructure code examples
6. THE Implementation_Highlights SHALL show Lambda function structure for each component type
7. THE Implementation_Highlights SHALL demonstrate how to avoid common anti-patterns

### Requirement 10: Progressive Learning

**User Story:** As a reader, I want each example to build on previous concepts, so that I can learn progressively without repetition.

#### Acceptance Criteria

1. THE First_Example SHALL introduce foundational concepts (basic component types, simple workflows)
2. THE Second_Example SHALL build on the first by introducing new concepts (client volatility, multiple UIs)
3. THE Third_Example SHALL introduce additional concepts (engine reuse, parallel processing)
4. WHEN introducing a new concept THEN THE Guideline_Document SHALL reference which example first introduced it
5. THE Later_Examples SHALL assume understanding of concepts from earlier examples
6. THE Guideline_Document SHALL avoid repeating explanations already covered in earlier examples

### Requirement 11: Anti-Patterns and Common Mistakes

**User Story:** As a reader, I want to see common mistakes and anti-patterns, so that I can avoid them in my own designs.

#### Acceptance Criteria

1. WHEN discussing a design decision THEN THE Guideline_Document SHALL explain what not to do
2. THE Guideline_Document SHALL include examples of functional decomposition and why it fails
3. THE Guideline_Document SHALL show CRUD-based ResourceAccess as an anti-pattern
4. THE Guideline_Document SHALL demonstrate layer violations and their consequences
5. THE Guideline_Document SHALL explain common mistakes in volatility identification

### Requirement 12: Conclusion and Best Practices

**User Story:** As a reader, I want a summary of key patterns and best practices, so that I have a reference guide for future work.

#### Acceptance Criteria

1. THE Conclusion SHALL summarize the key principles demonstrated across all examples
2. THE Conclusion SHALL provide a checklist for applying The Method to AWS
3. THE Conclusion SHALL list common patterns that emerged across examples
4. THE Conclusion SHALL provide guidance on when to use different AWS services
5. THE Conclusion SHALL include references for further learning

### Requirement 13: Code Samples and Repository

**User Story:** As a reader, I want access to complete code samples, so that I can study working implementations.

#### Acceptance Criteria

1. THE Guideline_Document SHALL reference a code repository with complete examples
2. WHEN referencing code THEN THE Guideline_Document SHALL provide GitHub repository links
3. THE Code_Repository SHALL include working implementations for each system example
4. THE Code_Repository SHALL include CDK infrastructure code
5. THE Code_Repository SHALL include README files with setup instructions

### Requirement 14: Visual Consistency

**User Story:** As a reader, I want consistent visual styling, so that I can easily recognize different types of information.

#### Acceptance Criteria

1. THE Guideline_Document SHALL use consistent formatting for component names
2. THE Guideline_Document SHALL use consistent color coding for component types in diagrams
3. THE Guideline_Document SHALL use consistent code block styling
4. THE Guideline_Document SHALL use consistent heading hierarchy
5. THE Guideline_Document SHALL use consistent notation for AWS services

### Requirement 15: Accessibility and Format

**User Story:** As a reader, I want the guideline in accessible formats, so that I can read it on different devices and platforms.

#### Acceptance Criteria

1. THE Guideline_Document SHALL be available in Markdown format
2. THE Guideline_Document SHALL be convertible to PDF format
3. THE Guideline_Document SHALL be readable on GitHub
4. THE Guideline_Document SHALL include alt text for all diagrams
5. THE Guideline_Document SHALL use semantic heading structure for screen readers
