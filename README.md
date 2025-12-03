# Getting Started with Kiro CLI and MCP Servers

[![AWS](https://img.shields.io/badge/AWS-Kiro%20CLI-FF9900?style=flat&logo=amazon-aws)](https://aws.amazon.com/kiro/)
[![MCP](https://img.shields.io/badge/MCP-Protocol-blue?style=flat)](https://modelcontextprotocol.io/)
[![License](https://img.shields.io/badge/License-MIT-green.svg)](LICENSE)

> A comprehensive guide to installing and using Kiro CLI with AWS Documentation, AWS Knowledge, and Brave Search MCP servers.

## 📋 Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Installation](#installation)
  - [Install Kiro CLI](#install-kiro-cli)
  - [Install MCP Servers](#install-mcp-servers)
- [Configuration](#configuration)
- [Usage](#usage)
  - [Basic Commands](#basic-commands)
  - [Example Use Cases](#example-use-cases)
- [Best Practices](#best-practices)
- [Troubleshooting](#troubleshooting)
- [Resources](#resources)
- [Contributing](#contributing)

## Overview

**Kiro** is an AI assistant built by AWS that runs locally via CLI and can be extended with Model Context Protocol (MCP) servers. MCP is an open protocol that allows AI assistants to connect to external tools and data sources.

### What You'll Learn

- ✅ Install and configure Kiro CLI
- ✅ Set up AWS Documentation and AWS Knowledge MCP servers
- ✅ Integrate Brave Search for web queries
- ✅ Use Kiro effectively for AWS architecture and development

## Prerequisites

- **OS**: macOS, Linux, or Windows with WSL2
- **Python**: 3.8 or higher
- **Node.js**: 18 or higher
- **AWS Account**: For AWS-specific features
- **Brave Search API Key**: [Get one here](https://brave.com/search/api/) (free tier available)

## Installation

### Install Kiro CLI

#### macOS (Homebrew)

```bash
brew install kiro-cli
```

#### Linux

```bash
curl -fsSL https://kiro.aws.dev/install.sh | sh
```

#### Verify Installation

```bash
kiro-cli --version
```

### Install MCP Servers

#### 1. AWS Documentation MCP Server

```bash
# Create directory for MCP servers
mkdir -p ~/.mcp-servers
cd ~/.mcp-servers

# Clone and install
git clone https://github.com/aws/aws-documentation-mcp-server.git
cd aws-documentation-mcp-server
pip install -e .
```

#### 2. AWS Knowledge MCP Server

```bash
cd ~/.mcp-servers
git clone https://github.com/aws/aws-knowledge-mcp-server.git
cd aws-knowledge-mcp-server
pip install -e .
```

#### 3. Brave Search MCP Server

```bash
npm install -g @modelcontextprotocol/server-brave-search
```

Get your Brave Search API key at [brave.com/search/api](https://brave.com/search/api/)

## Configuration

### Create MCP Configuration File

```bash
mkdir -p ~/.kiro
nano ~/.kiro/mcp-config.json
```

### Add Configuration

```json
{
  "mcpServers": {
    "aws-documentation": {
      "command": "python",
      "args": ["-m", "aws_documentation_mcp_server"],
      "env": {}
    },
    "aws-knowledge": {
      "command": "python",
      "args": ["-m", "aws_knowledge_mcp_server"],
      "env": {}
    },
    "brave-search": {
      "command": "npx",
      "args": ["-y", "@modelcontextprotocol/server-brave-search"],
      "env": {
        "BRAVE_API_KEY": "YOUR_BRAVE_API_KEY_HERE"
      }
    }
  }
}
```

**⚠️ Important**: Replace `YOUR_BRAVE_API_KEY_HERE` with your actual API key.

### Configure AWS Credentials

```bash
aws configure
```

## Usage

### Basic Commands

```bash
# Start interactive chat
kiro-cli chat

# Start with a prompt
kiro-cli chat "Explain AWS Lambda"

# Get help
kiro-cli --help
```

### In-Chat Commands

| Command | Description |
|---------|-------------|
| `/quit` | Exit the chat |
| `/save` | Save conversation |
| `/load` | Load previous conversation |
| `/context` | Manage context |
| `/clear` | Clear conversation history |

### Example Use Cases

#### 🏗️ Architecture Design

```
"Design a highly available, multi-region architecture for a SaaS application with:
- 99.99% uptime SLA
- Global user base
- GDPR compliance"
```

#### 📚 AWS Documentation Search

```
"Show me how to configure Lambda environment variables"
```

#### 🌍 Regional Availability

```
"Is AWS Bedrock available in eu-west-1?"
```

#### 💻 Code Generation

```
"Write a Python Lambda function that processes S3 events with error handling"
```

#### 🔍 Web Search

```
"What are the latest AWS announcements from re:Invent 2024?"
```

#### 💰 Cost Optimization

```
"Compare costs between EC2 and Fargate for a containerized web application"
```

#### 🔒 Security Review

```
"Review this IAM policy and identify security issues"
```

## Best Practices

### ✅ Do's

- **Be specific**: "Compare DynamoDB and Aurora for high-traffic e-commerce" vs "Tell me about databases"
- **Provide context**: Include requirements, scale, and constraints
- **Iterate**: Refine solutions based on responses
- **Use appropriate MCP servers**: AWS docs for official info, Brave for latest news

### ❌ Don'ts

- Don't share AWS credentials in chat
- Don't apply generated IAM policies without review
- Don't use vague questions
- Don't skip security considerations

## Troubleshooting

### MCP Servers Not Working

```bash
# Verify configuration
cat ~/.kiro/mcp-config.json

# Test individual server
python -m aws_documentation_mcp_server

# Check logs
kiro-cli chat --verbose
```

### AWS Credentials Issues

```bash
# Verify credentials
aws sts get-caller-identity

# Reconfigure
aws configure
```

### Brave Search Issues

- Verify API key in `~/.kiro/mcp-config.json`
- Check quota at [brave.com/search/api](https://brave.com/search/api/)
- Ensure key is not expired

## AWS Service Quick Reference

| Category | Services |
|----------|----------|
| **Compute** | Lambda, EC2, ECS, EKS, Fargate |
| **Storage** | S3, EBS, EFS, FSx |
| **Database** | RDS, DynamoDB, Aurora, Redshift |
| **Networking** | VPC, CloudFront, Route 53, API Gateway |
| **Security** | IAM, Secrets Manager, KMS, WAF |
| **AI/ML** | Bedrock, SageMaker, Rekognition |
| **Analytics** | Athena, Glue, EMR, Kinesis |
| **DevOps** | CodePipeline, CodeBuild, CodeDeploy |

## Resources

- 📖 [Kiro Documentation](https://aws.amazon.com/kiro/)
- 📖 [AWS Documentation](https://docs.aws.amazon.com/)
- 🔗 [MCP Protocol](https://modelcontextprotocol.io/)
- 💬 [AWS Support](https://aws.amazon.com/support/)
- 💰 [AWS Pricing Calculator](https://calculator.aws.amazon.com/)

## Quick Start Example

```bash
# Start Kiro
kiro-cli chat

# Try this first prompt:
"I'm new to AWS. Help me understand the core services I should learn first
for building a modern web application."
```

## Contributing

Contributions are welcome! Please feel free to submit a Pull Request.

1. Fork the repository
2. Create your feature branch (`git checkout -b feature/AmazingFeature`)
3. Commit your changes (`git commit -m 'Add some AmazingFeature'`)
4. Push to the branch (`git push origin feature/AmazingFeature`)
5. Open a Pull Request

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Acknowledgments

- AWS for creating Kiro CLI
- The MCP community for the protocol specification
- Brave Search for API access

---

**Made with ❤️ for the AWS community**

*For questions or issues, please open an issue on GitHub.*
