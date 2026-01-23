# The Method AWS Guide

A comprehensive guide demonstrating how to apply Juval Löwy's "The Method" from "Righting Software" to AWS serverless architectures. This guide shows how volatility-based decomposition, component taxonomy, and layered architecture naturally align with AWS services.

## About This Project

All core architectural principles—volatility-based decomposition, component taxonomy, and layered architecture—originate from Juval Löwy's "Righting Software" (Addison-Wesley, 2019). This guide's contribution is demonstrating how these proven principles map to AWS serverless services through complete system examples.

**Reference**: Löwy, Juval. _Righting Software_. Boston: Addison-Wesley Professional, 2019.

## What You'll Find

- **Core Principles**: Volatility-based decomposition and component taxonomy applied to AWS
- **System Examples**: Complete architectures for task management, e-commerce, trading systems, and content management
- **AWS Service Mappings**: How Managers, Engines, and ResourceAccess map to Step Functions, Lambda, and other AWS services
- **Implementation Patterns**: TypeScript code examples and CDK infrastructure templates
- **Best Practices**: Design patterns and anti-patterns for cloud-native systems

## Documentation

The complete guide is built with MkDocs and includes:

- **[Introduction](docs/guidelines/introduction.md)** - Overview of The Method and its relevance to cloud development
- **[Core Principles](docs/guidelines/core-principles.md)** - Volatility-based decomposition, component taxonomy, and layered architecture
- **[Component Taxonomy](docs/guidelines/component-taxonomy.md)** - Detailed explanation of each component type
- **[System Examples](docs/examples/)** - Complete architectural designs from different domains

## Getting Started

### Prerequisites

- Python 3.8+
- pip or uv package manager

### Local Development

1. Clone the repository:

```bash
git clone https://github.com/DimaKramskoy/the-method-aws-guide.git
cd the-method-aws-guide
```

2. Install MkDocs and dependencies:

```bash
pip install mkdocs-material
```

3. Run the local development server:

```bash
mkdocs serve
```

4. Open your browser to `http://127.0.0.1:8000/`

## Project Structure

```
.
├── docs/                      # Documentation content
│   ├── guidelines/           # Core principles and component taxonomy
│   ├── examples/             # Complete system examples
│   ├── reference/            # AWS mappings and best practices
│   └── getting-started/      # Quick start guides
├── .kiro/                    # Project specifications
│   ├── specs/                # Feature specifications
│   └── steering/             # AI assistant guidance
├── mkdocs.yml                # MkDocs configuration
└── README.md                 # This file
```

## System Examples

The guide includes four complete system examples with increasing complexity:

1. **Task Management System** (Simple) - Introduces foundational concepts
2. **E-Commerce Order Processing** (Medium) - Demonstrates workflow orchestration
3. **Real-Time Trading System** (Complex) - Shows event-driven architecture
4. **Content Management Platform** (Enterprise) - Covers multi-tenancy and caching

Each example includes:

- Business context and requirements
- Volatility analysis
- Architecture design with diagrams
- AWS service mapping
- Use case flows and call chains
- Implementation highlights with TypeScript/CDK code
- Lessons learned

## Technology Stack

- **Documentation**: MkDocs with Material theme
- **Language**: TypeScript
- **Infrastructure**: AWS CDK
- **AWS Services**: Lambda, Step Functions, DynamoDB, S3, EventBridge, and more

## Attribution

This project is based on **"Righting Software"** by Juval Löwy (Addison-Wesley, 2019). All core architectural principles, component taxonomy, and design concepts are attributed to Juval Löwy's work. This project's contribution is demonstrating how these principles apply to AWS cloud services.

## Contributing

Contributions are welcome! Please see [CONTRIBUTING.md](docs/about/contributing.md) for guidelines.

## License

This project is licensed under the MIT License - see the LICENSE file for details.

## Acknowledgments

- Juval Löwy for "Righting Software" and The Method
- The AWS community for serverless best practices
- All contributors to this guide

## Contact

For questions or feedback, please open an issue on GitHub.
