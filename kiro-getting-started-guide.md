# Getting Started with Kiro CLI and MCP Servers

## Overview

Kiro is an AI assistant built by AWS that runs locally via CLI and can be extended with Model Context Protocol (MCP) servers. MCP is an open protocol that allows AI assistants to connect to external tools and data sources.

This guide covers:
- Installing Kiro CLI
- Setting up AWS Documentation and AWS Knowledge MCP servers
- Setting up Brave Search MCP server
- Basic usage patterns

## Prerequisites

- macOS, Linux, or Windows with WSL2
- Python 3.8+ (for MCP servers)
- Node.js 18+ (for some MCP servers)
- AWS Account (for AWS-specific features)
- Brave Search API key (for web search)

## Part 1: Install Kiro CLI

### Step 1: Install Kiro

```bash
# Download and install Kiro CLI
# Visit: https://aws.amazon.com/kiro/ for latest installation instructions

# For macOS (Homebrew):
brew install kiro-cli

# For Linux:
curl -fsSL https://kiro.aws.dev/install.sh | sh

# Verify installation
kiro-cli --version
```

### Step 2: Initial Configuration

```bash
# Configure AWS credentials (if not already done)
aws configure

# Start Kiro for the first time
kiro-cli chat
```

Type `/quit` to exit when testing.

## Part 2: Install MCP Servers

### AWS Documentation MCP Server

This server provides access to AWS documentation, search capabilities, and regional availability information.

```bash
# Create a directory for MCP servers
mkdir -p ~/.mcp-servers
cd ~/.mcp-servers

# Clone the AWS Documentation MCP server
git clone https://github.com/aws/aws-documentation-mcp-server.git
cd aws-documentation-mcp-server

# Install dependencies
pip install -e .

# Test the server
python -m aws_documentation_mcp_server
```

### AWS Knowledge MCP Server

This server provides AWS-specific knowledge and best practices.

```bash
cd ~/.mcp-servers

# Clone the AWS Knowledge MCP server
git clone https://github.com/aws/aws-knowledge-mcp-server.git
cd aws-knowledge-mcp-server

# Install dependencies
pip install -e .
```

### Brave Search MCP Server

This server enables web search capabilities.

```bash
cd ~/.mcp-servers

# Install via npm
npm install -g @modelcontextprotocol/server-brave-search

# Get your Brave Search API key
# Visit: https://brave.com/search/api/
# Sign up for a free account and generate an API key
```

## Part 3: Configure Kiro to Use MCP Servers

### Step 1: Create Kiro Configuration File

```bash
# Create config directory
mkdir -p ~/.kiro

# Create or edit the MCP configuration file
nano ~/.kiro/mcp-config.json
```

### Step 2: Add MCP Server Configurations

Add this configuration to `~/.kiro/mcp-config.json`:

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

**Important**: Replace `YOUR_BRAVE_API_KEY_HERE` with your actual Brave Search API key.

### Step 3: Verify MCP Server Configuration

```bash
# Start Kiro and check available tools
kiro-cli chat

# In the Kiro chat, type:
# "What tools do you have access to?"
```

## Part 4: Using Kiro CLI

### Basic Commands

```bash
# Start interactive chat
kiro-cli chat

# Start chat with a specific prompt
kiro-cli chat "Explain AWS Lambda"

# Get help
kiro-cli --help
```

### In-Chat Commands

While in a Kiro chat session:

- `/quit` - Exit the chat
- `/save` - Save conversation
- `/load` - Load a previous conversation
- `/context` - Manage context
- `/clear` - Clear conversation history

### Example Usage Patterns

#### 1. AWS Architecture Questions

```
You: "I need to build a serverless API. What AWS services should I use?"

Kiro will search AWS documentation and provide recommendations for:
- API Gateway for REST/HTTP APIs
- Lambda for compute
- DynamoDB for database
- Cognito for authentication
```

#### 2. AWS Documentation Search

```
You: "Show me how to configure Lambda environment variables"

Kiro will use the AWS Documentation MCP server to find relevant docs
and provide step-by-step instructions.
```

#### 3. Regional Availability

```
You: "Is AWS Bedrock available in eu-west-1?"

Kiro will check regional availability using the AWS Knowledge MCP server.
```

#### 4. Web Search for Latest Information

```
You: "What are the latest AWS announcements from re:Invent 2024?"

Kiro will use Brave Search to find current information.
```

#### 5. Code Generation

```
You: "Write a Python Lambda function that processes S3 events"

Kiro will generate code with proper error handling and best practices.
```

#### 6. AWS CLI Commands

```
You: "How do I list all EC2 instances in my account?"

Kiro can provide the AWS CLI command and even execute it if you approve.
```

## Part 5: Advanced Features

### Working with Files

```bash
# Kiro can read and write files in your current directory
cd ~/my-project
kiro-cli chat

# In chat:
"Read the README.md file and suggest improvements"
"Create a new Python script that implements the architecture we discussed"
```

### AWS Resource Management

```
"List all my S3 buckets"
"Create a new Lambda function named 'data-processor'"
"Show me the CloudFormation stack status for my-stack"
```

### Multi-Step Tasks

```
"Help me deploy a containerized application to ECS:
1. Review my Dockerfile
2. Create an ECR repository
3. Build and push the image
4. Create an ECS task definition
5. Deploy to ECS Fargate"
```

## Part 6: Best Practices

### 1. Be Specific with Questions

❌ "Tell me about databases"
✅ "Compare DynamoDB and Aurora PostgreSQL for a high-traffic e-commerce application"

### 2. Provide Context

```
"I'm building a real-time analytics pipeline that processes 10,000 events/second.
What AWS services should I use?"
```

### 3. Iterate on Solutions

```
"The Lambda function you created is timing out. How can I optimize it?"
```

### 4. Use MCP Servers Effectively

- AWS Documentation server: For official AWS docs and API references
- AWS Knowledge server: For best practices and architecture patterns
- Brave Search: For latest news, blog posts, and community content

### 5. Security Considerations

- Never share AWS credentials in chat
- Review generated IAM policies before applying
- Use least-privilege access principles
- Kiro will substitute PII with placeholders automatically

## Part 7: Troubleshooting

### MCP Servers Not Working

```bash
# Check if MCP servers are configured
cat ~/.kiro/mcp-config.json

# Test individual MCP server
python -m aws_documentation_mcp_server

# Check Kiro logs
kiro-cli chat --verbose
```

### AWS Credentials Issues

```bash
# Verify AWS credentials
aws sts get-caller-identity

# Reconfigure if needed
aws configure
```

### Brave Search Not Working

- Verify API key is correct in `~/.kiro/mcp-config.json`
- Check API quota at https://brave.com/search/api/
- Ensure the key is not expired

## Part 8: Common Use Cases

### Architecture Design

```
"Design a highly available, multi-region architecture for a SaaS application
with these requirements:
- 99.99% uptime SLA
- Global user base
- GDPR compliance
- Real-time data sync"
```

### Cost Optimization

```
"Analyze my current Lambda usage and suggest cost optimizations"
"Compare costs between EC2 and Fargate for my workload"
```

### Security Review

```
"Review this IAM policy and identify security issues"
"What security best practices should I follow for an S3 bucket storing customer data?"
```

### Migration Planning

```
"I'm migrating from on-premises to AWS. Help me plan the migration for:
- 50 VMs
- 10TB database
- Legacy .NET applications"
```

### Troubleshooting

```
"My CloudFormation stack failed with error: 'Resource creation cancelled'.
Help me debug this."
```

## Part 9: Learning Resources

### AWS Service Categories

**Compute**: Lambda, EC2, ECS, EKS, Fargate
**Storage**: S3, EBS, EFS, FSx
**Database**: RDS, DynamoDB, Aurora, Redshift
**Networking**: VPC, CloudFront, Route 53, API Gateway
**Security**: IAM, Secrets Manager, KMS, WAF
**AI/ML**: Bedrock, SageMaker, Rekognition
**Analytics**: Athena, Glue, EMR, Kinesis
**Developer Tools**: CodePipeline, CodeBuild, CodeDeploy

### Ask Kiro to Explain

```
"Explain AWS Lambda in simple terms"
"What's the difference between ECS and EKS?"
"When should I use DynamoDB vs RDS?"
"Explain VPC networking concepts"
```

## Part 10: Next Steps

1. **Explore AWS Services**: Ask Kiro about different AWS services relevant to your use case
2. **Build a Project**: Start with a simple project (e.g., serverless API, static website)
3. **Learn Infrastructure as Code**: Ask about CloudFormation or CDK
4. **Understand Pricing**: Use Kiro to understand AWS pricing models
5. **Security First**: Learn about IAM, security groups, and encryption
6. **Join AWS Community**: Ask Kiro about AWS user groups and resources

## Quick Reference Card

```bash
# Start Kiro
kiro-cli chat

# Common in-chat commands
/quit          # Exit
/save          # Save conversation
/load          # Load conversation
/help          # Show help

# Example prompts
"Search AWS docs for [topic]"
"Is [service] available in [region]?"
"Generate [language] code for [task]"
"Explain [AWS concept]"
"Compare [service A] vs [service B]"
"Review this [code/config]"
"Create a diagram for [architecture]"
```

## Support and Resources

- **Kiro Documentation**: https://aws.amazon.com/kiro/
- **AWS Documentation**: https://docs.aws.amazon.com/
- **MCP Protocol**: https://modelcontextprotocol.io/
- **AWS Support**: https://aws.amazon.com/support/

## Conclusion

You now have Kiro CLI configured with AWS Documentation, AWS Knowledge, and Brave Search MCP servers. Start by asking simple questions and gradually explore more complex use cases. Kiro learns from context, so the more specific you are, the better the responses.

**First prompt to try:**

```
"I'm new to AWS. Help me understand the core services I should learn first
for building a modern web application."
```

Happy building! 🚀
