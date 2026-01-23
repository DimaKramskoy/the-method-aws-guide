# Installation

This guide walks you through setting up your development environment for building Method-based systems on AWS.

## Prerequisites

- **Node.js** 18.x or 20.x
- **npm** or **yarn**
- **AWS Account** with appropriate permissions
- **AWS CLI** configured with credentials
- **Python 3.x** (for MkDocs documentation)

## Install Development Tools

### 1. Install Node.js and npm

Download and install from [nodejs.org](https://nodejs.org/) or use a version manager:

```bash
# Using nvm (recommended)
nvm install 20
nvm use 20
```

### 2. Install AWS CLI

```bash
# macOS
brew install awscli

# Linux
curl "https://awscli.amazonaws.com/awscli-exe-linux-x86_64.zip" -o "awscliv2.zip"
unzip awscliv2.zip
sudo ./aws/install

# Windows
# Download installer from https://aws.amazon.com/cli/
```

### 3. Configure AWS Credentials

```bash
aws configure
# Enter your AWS Access Key ID
# Enter your AWS Secret Access Key
# Enter your default region (e.g., us-east-1)
# Enter your default output format (json)
```

### 4. Install AWS CDK

```bash
npm install -g aws-cdk
```

### 5. Install MkDocs (for documentation)

```bash
pip install mkdocs mkdocs-material
```

## Project Setup

### 1. Clone the Repository

```bash
git clone https://github.com/your-org/the-method-on-aws.git
cd the-method-on-aws
```

### 2. Install Dependencies

```bash
npm install
```

### 3. Bootstrap CDK (First Time Only)

```bash
cdk bootstrap aws://ACCOUNT-ID/REGION
```

## Verify Installation

### Check Node.js and npm

```bash
node --version  # Should show v18.x or v20.x
npm --version
```

### Check AWS CLI

```bash
aws --version
aws sts get-caller-identity  # Should show your AWS account info
```

### Check CDK

```bash
cdk --version
```

### Check MkDocs

```bash
mkdocs --version
```

## View Documentation Locally

```bash
# Serve documentation with live reload
mkdocs serve

# Open browser to http://127.0.0.1:8000
```

## Next Steps

- **[Quick Start](quick-start.md)** - Build your first example
- **[Core Principles](../guidelines/core-principles.md)** - Learn The Method's principles
