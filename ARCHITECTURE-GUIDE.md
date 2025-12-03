# Building AWS Architectures with Kiro CLI: A Practical Guide

## Overview

This guide demonstrates how to use Kiro CLI effectively to explore, design, and build AWS architectures. Using a real-world example (CloudWatch to Kafka integration), you'll learn how to craft effective prompts, leverage AWS documentation, and build solutions step-by-step with Kiro's assistance.

## Table of Contents

- [Core Principles](#core-principles)
- [Effective Prompt Patterns](#effective-prompt-patterns)
- [Example 1: Initial Architecture Exploration](#example-1-initial-architecture-exploration)
- [Example 2: Deep Dive into Specific Services](#example-2-deep-dive-into-specific-services)
- [Example 3: Building Implementation Code](#example-3-building-implementation-code)
- [Example 4: Comparing Architecture Options](#example-4-comparing-architecture-options)
- [Example 5: Security and Best Practices Review](#example-5-security-and-best-practices-review)
- [Example 6: Cost Optimization](#example-6-cost-optimization)
- [Advanced Techniques](#advanced-techniques)

## Core Principles

### 1. Always Request Clarification First

Before Kiro builds anything, ask it to clarify your requirements. This ensures the solution matches your actual needs.

**Pattern:**
```
"Ask me up to six questions to clarify my request before building anything."
```

### 2. Use AWS Documentation as Source of Truth

Always direct Kiro to use official AWS documentation rather than general knowledge.

**Pattern:**
```
"Use only AWS documentation to answer this question."
```

### 3. Build Incrementally

Start with architecture, then move to implementation details, then code. Don't try to do everything at once.

### 4. Be Specific About Context

Provide constraints, requirements, and context upfront to get better recommendations.

## Effective Prompt Patterns

### Pattern 1: Discovery and Exploration

```
"Use only AWS documentation. I need to [goal]. Ask me up to six questions 
to clarify my requirements before suggesting any solutions."
```

### Pattern 2: Architecture Comparison

```
"Use only AWS documentation. Compare [Option A] vs [Option B] for [use case].
Focus on: scalability, cost, operational complexity, and security."
```

### Pattern 3: Implementation Guidance

```
"Use only AWS documentation. Show me how to implement [specific component].
Include: IAM permissions, configuration examples, and error handling."
```

### Pattern 4: Best Practices Review

```
"Use only AWS documentation. Review this [architecture/code] against AWS 
best practices for: security, reliability, performance, and cost optimization."
```

### Pattern 5: Troubleshooting

```
"Use only AWS documentation. I'm experiencing [problem]. Help me debug this
by asking clarifying questions first, then provide solutions."
```

## Example 1: Initial Architecture Exploration

### Scenario
You need to send CloudWatch alarms to a Kafka topic but aren't sure of the best approach.

### Effective Prompt

```
Use only AWS documentation. I need to send CloudWatch alarm notifications 
to an Apache Kafka topic (Amazon MSK). Ask me up to six questions to 
clarify my requirements before suggesting any architecture options.
```

### What Kiro Will Ask

Kiro should ask questions like:
1. What is your expected alarm volume (alarms per minute/hour)?
2. Do you need to send alarms to multiple destinations besides Kafka?
3. Do you require any transformation of alarm data before sending to Kafka?
4. What are your latency requirements for alarm delivery?
5. Do you need to filter or route different alarm types to different topics?
6. Are you using Amazon MSK or self-managed Kafka?

### Your Responses

```
1. Low volume - approximately 10-50 alarms per hour
2. Yes, we also need email notifications for critical alarms
3. Minimal transformation - just need to format as JSON
4. Near real-time is fine - within 30 seconds is acceptable
5. All alarms go to a single topic for now
6. Amazon MSK
```

### Follow-Up Prompt

```
Use only AWS documentation. Based on my answers, recommend 2-3 architecture 
options with pros and cons for each. Focus on simplicity, cost-effectiveness, 
and maintainability.
```

### Expected Output

Kiro will provide architecture options like:
- **Option A**: CloudWatch Alarms → SNS → Lambda → MSK
- **Option B**: CloudWatch Alarms → EventBridge → Lambda → MSK

With detailed pros/cons for each based on your requirements.

## Example 2: Deep Dive into Specific Services

### Scenario
You've chosen the SNS → Lambda → MSK approach and need to understand SNS configuration.

### Effective Prompt

```
Use only AWS documentation. I'm implementing CloudWatch Alarms → SNS → Lambda → MSK.
Explain how to configure the SNS topic and subscription for this use case. Include:

1. Required IAM permissions
2. SNS topic configuration options
3. Lambda subscription setup
4. Message filtering capabilities
5. Retry and dead-letter queue configuration
```

### Follow-Up for Specific Details

```
Use only AWS documentation. Show me the exact IAM policy needed for:
1. CloudWatch to publish to SNS
2. SNS to invoke Lambda
3. Lambda to write to MSK

Provide the JSON policy documents.
```

## Example 3: Building Implementation Code

### Scenario
You need to write the Lambda function that receives SNS messages and publishes to Kafka.

### Effective Prompt

```
Use only AWS documentation. I need to create a Lambda function that:
1. Receives CloudWatch alarm notifications from SNS
2. Transforms the alarm data to JSON format
3. Publishes to an Amazon MSK topic

Ask me up to six questions about my specific requirements before writing any code.
```

### What Kiro Will Ask

1. What programming language do you prefer (Python, Node.js, Java, etc.)?
2. What authentication method are you using for MSK (IAM, SASL/SCRAM, mTLS)?
3. Do you need to handle batch processing or single messages?
4. What should happen if the Kafka write fails?
5. Do you need any specific logging or monitoring?
6. What is your MSK cluster configuration (bootstrap servers, topic name)?

### Your Responses

```
1. Python 3.11
2. IAM authentication
3. Single message processing is fine
4. Log the error and send to a dead-letter queue
5. Yes, log all operations to CloudWatch Logs
6. Bootstrap servers: b-1.mycluster.kafka.us-east-1.amazonaws.com:9098
   Topic: cloudwatch-alarms
```

### Follow-Up Prompt

```
Use only AWS documentation. Now write the Lambda function code with:
1. Proper error handling
2. Idempotent message processing
3. CloudWatch Logs integration
4. IAM authentication for MSK
5. Environment variables for configuration
```

### Expected Output

Kiro will provide:
- Complete Lambda function code
- requirements.txt or package.json
- Environment variable configuration
- IAM role policy
- Deployment instructions

## Example 4: Comparing Architecture Options

### Scenario
You want to understand the trade-offs between SNS and EventBridge approaches.

### Effective Prompt

```
Use only AWS documentation. Compare these two architectures for sending 
CloudWatch alarms to Kafka:

Option A: CloudWatch → SNS → Lambda → MSK
Option B: CloudWatch → EventBridge → Lambda → MSK

Create a detailed comparison covering:
1. Setup complexity
2. Message filtering capabilities
3. Fan-out to multiple destinations
4. Cost at 100 alarms/hour
5. Operational overhead
6. Failure handling and retries
7. Use cases where one is clearly better than the other
```

### Follow-Up for Decision Making

```
Use only AWS documentation. Based on this comparison, which option would 
you recommend for:

Scenario 1: Simple alarm routing, need email + Kafka, low volume
Scenario 2: Complex routing rules, 50+ different alarm types, high volume
Scenario 3: Need to integrate with Step Functions and other AWS services
```

## Example 5: Security and Best Practices Review

### Scenario
You have a draft architecture and want to ensure it follows AWS best practices.

### Effective Prompt

```
Use only AWS documentation. Review this architecture against AWS Well-Architected 
Framework best practices:

Architecture:
- CloudWatch custom metrics from IoT devices
- CloudWatch Alarms with SNS notifications
- Lambda function processing SNS messages
- Lambda writing to Amazon MSK
- MSK cluster in private subnets

Focus on:
1. Security (IAM, encryption, network isolation)
2. Reliability (fault tolerance, retries, monitoring)
3. Performance (latency, throughput optimization)
4. Cost optimization
5. Operational excellence

Ask clarifying questions if you need more details about the implementation.
```

### Follow-Up for Specific Concerns

```
Use only AWS documentation. I'm concerned about security. Provide specific 
recommendations for:

1. Encrypting data in transit between all components
2. Least-privilege IAM policies for each component
3. Network security (VPC, security groups, NACLs)
4. Secrets management for any credentials
5. Audit logging and compliance
```

## Example 6: Cost Optimization

### Scenario
You want to understand and optimize costs for your architecture.

### Effective Prompt

```
Use only AWS documentation. Help me estimate and optimize costs for this architecture:

Components:
- CloudWatch: 500 custom metrics, 1-minute resolution
- CloudWatch Alarms: 100 alarms
- SNS: 100 notifications/hour
- Lambda: 100 invocations/hour, 512MB memory, 2-second average duration
- MSK: 2-broker cluster, kafka.m5.large instances

Ask me questions about usage patterns that would affect cost, then provide:
1. Estimated monthly cost breakdown
2. Cost optimization recommendations
3. Alternative approaches that might be more cost-effective
```

### Follow-Up for Optimization

```
Use only AWS documentation. The Lambda costs seem high. Show me how to:
1. Optimize Lambda memory and timeout settings
2. Implement batching to reduce invocations
3. Use Lambda reserved concurrency if beneficial
4. Consider alternative compute options (Fargate, ECS)
```

## Advanced Techniques

### Technique 1: Iterative Refinement

Start broad, then narrow down:

```
# Step 1: High-level architecture
"Use only AWS documentation. Design a serverless event-driven architecture 
for processing IoT sensor data. Ask clarifying questions first."

# Step 2: Focus on specific component
"Use only AWS documentation. Deep dive into the data ingestion layer. 
Show me options for handling 10,000 events/second."

# Step 3: Implementation details
"Use only AWS documentation. Show me how to implement backpressure handling 
in the ingestion layer."
```

### Technique 2: Multi-Perspective Analysis

Ask Kiro to analyze from different angles:

```
"Use only AWS documentation. Analyze this architecture from three perspectives:

1. DevOps Engineer: Deployment, monitoring, troubleshooting
2. Security Engineer: Threat model, compliance, access control
3. FinOps Analyst: Cost drivers, optimization opportunities, budget forecasting"
```

### Technique 3: Scenario-Based Testing

```
"Use only AWS documentation. Walk through these failure scenarios for my 
architecture and explain what happens:

1. Lambda function times out
2. MSK cluster is unavailable
3. SNS delivery fails
4. CloudWatch API is throttled
5. Lambda hits concurrency limits

For each scenario, explain the failure mode and recovery mechanism."
```

### Technique 4: Documentation Deep Dive

```
"Use only AWS documentation. I need to understand Amazon MSK's IAM authentication 
in depth. Explain:

1. How IAM authentication works with Kafka protocol
2. Required IAM policies for producers and consumers
3. How to configure Kafka clients for IAM auth
4. Common troubleshooting issues
5. Performance implications

Provide code examples for Python Kafka clients."
```

### Technique 5: Migration Planning

```
"Use only AWS documentation. I'm migrating from self-managed Kafka to Amazon MSK. 
Ask me questions about my current setup, then create a migration plan covering:

1. Pre-migration assessment
2. MSK cluster sizing and configuration
3. Data migration strategy
4. Application migration approach
5. Rollback plan
6. Post-migration validation"
```

## Real-World Example: Complete Workflow

### Starting Point

```
Use only AWS documentation. I need to build a system that:
- Collects metrics from 200 IoT devices in remote locations
- Stores metrics in CloudWatch
- Creates alarms for anomalies
- Sends alarm notifications to a Kafka topic for downstream processing
- Needs to be cost-effective and reliable

Ask me up to six questions to clarify requirements before suggesting anything.
```

### After Clarification, Architecture Phase

```
Use only AWS documentation. Based on my answers, design a complete architecture.
Include:
1. Data flow diagram (describe in text)
2. AWS services used and why
3. Networking and security considerations
4. Estimated costs
5. Deployment approach
```

### Implementation Phase - Component 1

```
Use only AWS documentation. Let's implement the metrics ingestion component.
Show me:
1. How IoT devices should send metrics to CloudWatch
2. Python code example for PutMetricData API
3. Batching strategy for efficiency
4. Error handling and retries
5. IAM permissions needed
```

### Implementation Phase - Component 2

```
Use only AWS documentation. Now let's implement the alarm-to-Kafka integration.
I've decided on SNS → Lambda → MSK approach. Provide:
1. CloudFormation or CDK template for infrastructure
2. Lambda function code
3. IAM roles and policies
4. Testing strategy
```

### Validation Phase

```
Use only AWS documentation. Review the complete implementation against:
1. AWS Well-Architected Framework
2. Security best practices
3. Cost optimization opportunities
4. Operational excellence

Provide specific, actionable recommendations for improvements.
```

## Best Practices Summary

### Do's ✅

1. **Always use "Use only AWS documentation"** - Ensures accurate, up-to-date information
2. **Ask Kiro to ask questions** - Gets better, more tailored solutions
3. **Build incrementally** - Architecture → Components → Code → Testing
4. **Be specific about constraints** - Budget, latency, scale, compliance requirements
5. **Request multiple options** - Compare trade-offs before committing
6. **Ask for IAM policies explicitly** - Security is critical
7. **Request error handling** - Production code needs robust error handling
8. **Validate against Well-Architected** - Ensures best practices

### Don'ts ❌

1. **Don't ask vague questions** - "How do I use Lambda?" vs "How do I configure Lambda to process SNS messages and write to MSK?"
2. **Don't skip the clarification step** - You'll get generic solutions
3. **Don't try to build everything at once** - Break it down
4. **Don't ignore security** - Always ask about IAM, encryption, network isolation
5. **Don't forget about costs** - Ask for cost estimates and optimization
6. **Don't skip testing strategy** - Ask how to test each component
7. **Don't ignore operational concerns** - Monitoring, logging, alerting matter

## Prompt Templates

### Template 1: New Architecture

```
Use only AWS documentation. I need to build [description of system].

Requirements:
- [Requirement 1]
- [Requirement 2]
- [Requirement 3]

Constraints:
- [Constraint 1]
- [Constraint 2]

Ask me up to six questions to clarify my requirements before suggesting 
any architecture options.
```

### Template 2: Service Deep Dive

```
Use only AWS documentation. Explain [AWS Service] for [specific use case].

Include:
1. How it works
2. Configuration options relevant to my use case
3. IAM permissions required
4. Integration with [other services]
5. Best practices
6. Common pitfalls to avoid
```

### Template 3: Code Implementation

```
Use only AWS documentation. Write [component description] that:
- [Requirement 1]
- [Requirement 2]
- [Requirement 3]

Ask me clarifying questions about:
- Programming language preference
- Error handling requirements
- Logging and monitoring needs
- Configuration management approach

Then provide complete, production-ready code.
```

### Template 4: Architecture Review

```
Use only AWS documentation. Review this architecture:

[Describe architecture]

Against these criteria:
1. Security
2. Reliability
3. Performance
4. Cost
5. Operational complexity

Provide specific, actionable recommendations for each area.
```

### Template 5: Troubleshooting

```
Use only AWS documentation. I'm experiencing this issue:

Problem: [Description]
Expected behavior: [What should happen]
Actual behavior: [What is happening]
Architecture: [Brief description]

Ask me diagnostic questions, then help me troubleshoot and resolve this.
```

## Conclusion

Effective use of Kiro CLI for AWS architecture work comes down to:

1. **Clear, specific prompts** with context and constraints
2. **Requesting clarification** before solutions are proposed
3. **Using AWS documentation** as the authoritative source
4. **Building incrementally** from architecture to implementation
5. **Validating against best practices** throughout the process

By following these patterns and examples, you'll get more accurate, relevant, and production-ready solutions from Kiro CLI.

## Additional Resources

- [AWS Well-Architected Framework](https://aws.amazon.com/architecture/well-architected/)
- [AWS Architecture Center](https://aws.amazon.com/architecture/)
- [AWS Documentation](https://docs.aws.amazon.com/)

---

*This guide uses the [CloudWatch-Kafka Integration Options](https://github.com/rushealy-aws/cloudwatch-kafka-integration-options) repository as a reference example.*
