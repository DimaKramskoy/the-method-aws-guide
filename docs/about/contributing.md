# Contributing

We welcome contributions to improve this guideline and add more examples!

## How to Contribute

### Reporting Issues

If you find errors, unclear explanations, or have suggestions:

1. Open an issue on GitHub
2. Describe the problem or suggestion clearly
3. Include relevant section references

### Contributing Examples

We're especially interested in:

- New system examples from different domains
- Real-world case studies
- Additional AWS service mappings
- Code samples and implementations

### Contributing Documentation

To contribute documentation improvements:

1. Fork the repository
2. Make your changes in the `docs/` directory
3. Test locally with `mkdocs serve`
4. Submit a pull request

## Guidelines for Contributions

### Maintain Attribution

All contributions must maintain proper attribution to Juval Löwy and "Righting Software" for The Method's principles.

### Follow The Method

All examples and documentation must correctly apply The Method's principles:

- Use volatility-based decomposition
- Follow component taxonomy
- Respect layer communication rules
- Use atomic business verbs in ResourceAccess

### Code Quality

Code contributions should:

- Follow TypeScript best practices
- Include proper type definitions
- Use AWS CDK for infrastructure
- Include tests
- Follow naming conventions from the guideline

### Documentation Quality

Documentation contributions should:

- Be clear and concise
- Include code examples
- Use consistent formatting
- Include diagrams where helpful
- Follow the existing structure

## Development Setup

```bash
# Clone the repository
git clone https://github.com/your-org/the-method-on-aws.git
cd the-method-on-aws

# Install dependencies
npm install
pip install mkdocs mkdocs-material

# Serve documentation locally
mkdocs serve

# Build documentation
mkdocs build
```

## Pull Request Process

1. Create a feature branch
2. Make your changes
3. Test locally
4. Update documentation if needed
5. Submit pull request with clear description
6. Address review feedback

## Code of Conduct

Be respectful, constructive, and professional in all interactions.

## Questions?

Open an issue or discussion on GitHub if you have questions about contributing.
