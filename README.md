# auto-spend-guard
Auto-Spend-Guard is an AI-powered FinOps system that monitors cloud and SaaS spend, detects anomalies early, explains root causes, and automates remediation before costs spiral out of control.

# Auto-Spend Guard: FinOps Multi-Agent System with FOCUS Data Structure

## Overview

Auto-Spend Guard is a multi-tenant, multi-agent system designed to deliver outcome-based FinOps services using the FOCUS (FinOps Open Cost and Usage Specification) data standard. This intelligent platform automatically guards your cloud spending through specialized agents that optimize costs, detect anomalies, forecast usage, and ensure compliance across vendor, cloud, and SaaS spending categories.

## Table of Contents

- [What is FinOps?](#what-is-finops)
- [FOCUS Data Structure](#focus-data-structure)
- [Auto-Spend Guard Architecture](#auto-spend-guard-architecture)
- [MVP Development Plan](#mvp-development-plan)
- [Agent Examples](#agent-examples)
- [Getting Started](#getting-started)
- [Contributing](#contributing)

## What is FinOps?

FinOps (Financial Operations) is a discipline that combines financial management, operations, and technology to maximize business value from cloud investments. It enables cross-functional teams to make data-driven decisions about cloud spending by creating a culture of financial accountability.

### Core Principles
- **Everyone takes ownership** of their cloud usage
- **Teams collaborate** on cost optimization decisions
- **Data-driven decisions** replace cost estimation guesswork
- **Continuous improvement** through iterative optimization

## FOCUS Data Structure

The FinOps Open Cost and Usage Specification (FOCUS) normalizes cloud billing data across providers, enabling consistent analysis and optimization regardless of whether you're using AWS, Azure, Google Cloud, or multiple providers.

### Key FOCUS Components

#### Standardized Dimensions
```json
{
  "Provider": "AWS|Azure|GCP",
  "ServiceName": "Compute|Storage|Network",
  "ResourceId": "i-1234567890abcdef0",
  "ResourceType": "Virtual Machine",
  "Region": "us-east-1",
  "AvailabilityZone": "us-east-1a",
  "PricingCategory": "On-Demand|Reserved|Spot",
  "CommitmentDiscountType": "Savings Plan|Reserved Instance"
}
```

#### Standardized Metrics
```json
{
  "BilledCost": 100.50,
  "EffectiveCost": 85.25,
  "ListCost": 120.00,
  "UsageQuantity": 744,
  "UsageUnit": "Hours"
}
```

#### Time Dimensions
```json
{
  "BillingPeriodStart": "2024-01-01T00:00:00Z",
  "BillingPeriodEnd": "2024-01-31T23:59:59Z",
  "ServicePeriodStart": "2024-01-15T10:30:00Z",
  "ServicePeriodEnd": "2024-01-15T11:30:00Z"
}
```

## Auto-Spend Guard Architecture

Auto-Spend Guard's intelligent architecture evolves through three iterations, from basic routing to advanced multi-agent orchestration:

### Core Agent Types

1. **Router Agent** - Classifies queries into spending categories
2. **Workflow Agent** - Orchestrates data retrieval and response generation
3. **Anomaly Detection Agent** - Identifies spending irregularities using traditional ML
4. **Root Cause Analysis Agent** - Provides LLM-powered explanations
5. **Vendor Intelligence Agent** - Handles vendor-related queries and recommendations

### Multi-Tenant Design Benefits

- **Standardized Data**: Single agent codebase works across all tenants
- **Consistent Allocation**: FOCUS account dimensions enable accurate cost attribution
- **Measurable Outcomes**: Standardized metrics for SLA tracking
- **Data Segregation**: Robust multi-tenant guardrails prevent data leakage

## MVP Development Plan

### Iteration 1: Foundation & Intelligent Routing
**Core Components:**
- **Router Agent**: Classifies incoming questions into 3 categories:
  - **Vendor-related** queries (pricing, features, comparisons)
  - **Cloud Spend-related** queries (AWS, Azure, GCP costs)
  - **SaaS Spend** queries (subscription management, usage optimization)
- **Workflow Agent**: After routing, pulls relevant data from datastore and enables LLM response generation
- **Evaluation Focus**: Key success metric is whether the system responds with something **meaningful and useful** to the user

**Technical Implementation:**
- **Retrieval/Data Storage**:
  - **Time-series datastore** for cost/spend-related data
  - **Relational table** for structured/vendor data
- **Guardrails**: Ensure **customer data is properly segregated** in multi-tenant settings to prevent leakage across clients

**Deliverables:**
- [ ] Router agent with classification accuracy >85%
- [ ] Workflow agent with data retrieval pipeline
- [ ] Multi-tenant data isolation framework
- [ ] Basic response quality evaluation system

### Iteration 2: Intelligent Anomaly Detection
**Core Components:**
- **Anomaly Detection**: 
  - Use **traditional ML model** (not LLM) to flag anomalies in spend - optimized for cost efficiency
  - LLM used only for **Root Cause Analysis (RCA)** and explanation generation
- **Evaluation Focus**: 
  - **Anomaly detection accuracy** (precision, recall, false positives)
  - **Usefulness of RCA explanations** from users

**Technical Implementation:**
- Statistical/ML-based anomaly detection algorithms
- LLM-powered RCA explanation engine
- Feedback loop for continuous model improvement

**Deliverables:**
- [ ] Traditional ML anomaly detection with >90% accuracy
- [ ] LLM-based RCA explanation system
- [ ] User feedback collection for RCA quality
- [ ] Performance benchmarking dashboard

### Iteration 3: Advanced Multi-Agent Orchestration
**Core Components:**
- **Multi-Agent Design**:
  - Move to **hierarchical agent structure** with orchestrator agent coordinating specialized sub-agents
  - Specialized agents covering:
    - Anomaly detection and monitoring
    - Root cause analysis and investigation
    - Vendor intelligence and recommendations
    - Automated remediation and optimization
- **Data Connectivity**: Explore **MCP (Model Context Protocol)** for connecting to multiple data sources

**Technical Implementation:**
- Hierarchical agent communication protocols
- Advanced orchestration engine
- Multi-source data integration via MCP
- Automated remediation workflows

**Deliverables:**
- [ ] Orchestrator agent with sub-agent coordination
- [ ] MCP integration for multi-source data access
- [ ] Automated remediation capabilities
- [ ] Advanced agent performance analytics
- [ ] End-to-end outcome tracking system

## Agent Examples

### Router Agent (Iteration 1)

**Input Query:**
```json
{
  "userQuery": "Why did our AWS compute costs spike 300% last week?",
  "tenantId": "tenant-123",
  "timestamp": "2024-01-31T15:30:00Z",
  "userId": "user-456"
}
```

**Agent Output:**
```json
{
  "agentType": "Router",
  "tenantId": "tenant-123",
  "timestamp": "2024-01-31T15:30:01Z",
  "classification": {
    "category": "Cloud Spend-related",
    "subCategory": "Cost Anomaly",
    "confidence": 0.92,
    "provider": "AWS",
    "serviceType": "Compute"
  },
  "routingDecision": {
    "nextAgent": "WorkflowAgent",
    "dataSourcesRequired": ["time-series", "resource-metadata"],
    "priority": "HIGH"
  },
  "extractedEntities": {
    "timeRange": "last week",
    "costIncrease": "300%",
    "provider": "AWS",
    "service": "compute"
  }
}
```

### Workflow Agent (Iteration 1)

**Input from Router:**
```json
{
  "classification": "Cloud Spend-related",
  "extractedEntities": {
    "timeRange": "last week",
    "provider": "AWS",
    "service": "compute"
  },
  "tenantId": "tenant-123"
}
```

**Agent Output:**
```json
{
  "agentType": "Workflow",
  "tenantId": "tenant-123",
  "timestamp": "2024-01-31T15:30:05Z",
  "dataRetrieved": {
    "costData": {
      "currentWeek": 15000.00,
      "previousWeek": 5000.00,
      "percentageIncrease": 200.0,
      "affectedResources": ["i-1234567890abcdef0", "i-0987654321fedcba0"]
    },
    "contextualInfo": {
      "instanceTypeChanges": 2,
      "newInstancesLaunched": 5,
      "configurationChanges": 3
    }
  },
  "generatedResponse": {
    "summary": "Your AWS compute costs increased by 200% last week due to 5 new large instances being launched and 2 existing instances being resized.",
    "actionItems": [
      "Review the necessity of newly launched instances",
      "Consider right-sizing recently modified instances",
      "Set up automated cost alerts for future spikes"
    ],
    "confidenceScore": 0.87
  }
}
```

### Anomaly Detection Agent (Iteration 2)

**Input FOCUS Data:**
```json
{
  "Provider": "Azure",
  "ServiceName": "Virtual Machines",
  "ResourceId": "vm-anomaly-example",
  "BilledCost": 2500.00,
  "EffectiveCost": 2500.00,
  "UsageQuantity": 744,
  "UsageUnit": "Hours",
  "BillingPeriodStart": "2024-01-01T00:00:00Z",
  "BillingPeriodEnd": "2024-01-31T23:59:59Z"
}
```

**Agent Output:**
```json
{
  "agentType": "AnomalyDetection",
  "tenantId": "tenant-456",
  "timestamp": "2024-01-31T12:00:00Z",
  "anomalyDetection": {
    "isAnomaly": true,
    "anomalyScore": 0.95,
    "detectionMethod": "Isolation Forest",
    "baseline": {
      "expectedCost": 400.00,
      "confidenceInterval": [350.00, 450.00]
    },
    "actualCost": 2500.00,
    "deviationSigmas": 4.2
  },
  "anomalyDetails": {
    "type": "COST_SPIKE",
    "severity": "CRITICAL",
    "duration": "15 days",
    "affectedMetrics": ["BilledCost", "UsageQuantity"],
    "seasonalityAdjusted": true
  },
  "modelPerformance": {
    "precision": 0.92,
    "recall": 0.88,
    "falsePositiveRate": 0.05
  },
  "triggerRCA": true
}
```

### Root Cause Analysis Agent (Iteration 2)

**Input from Anomaly Detection:**
```json
{
  "anomalyDetected": true,
  "resourceId": "vm-anomaly-example",
  "costIncrease": 525.0,
  "timeRange": "2024-01-15 to 2024-01-31"
}
```

**Agent Output:**
```json
{
  "agentType": "RootCauseAnalysis",
  "tenantId": "tenant-456",
  "timestamp": "2024-01-31T12:05:00Z",
  "rootCauseAnalysis": {
    "primaryCause": "Instance Type Upgrade",
    "confidence": 0.89,
    "explanation": "The virtual machine was upgraded from Standard_B2s to Standard_D8s_v3 on January 15th, increasing compute capacity by 8x and costs proportionally.",
    "contributingFactors": [
      {
        "factor": "Extended Runtime",
        "impact": "15% cost increase",
        "description": "VM running 24/7 instead of previous 12-hour schedule"
      },
      {
        "factor": "Premium Storage Addition", 
        "impact": "10% cost increase",
        "description": "Added 1TB premium SSD on January 20th"
      }
    ]
  },
  "recommendations": [
    {
      "action": "RIGHT_SIZE",
      "description": "Analyze actual CPU/memory utilization to determine if D8s_v3 is necessary",
      "potentialSavings": 1800.00,
      "implementationEffort": "LOW"
    },
    {
      "action": "SCHEDULE_OPTIMIZATION",
      "description": "Implement automated start/stop if 24/7 operation isn't required",
      "potentialSavings": 375.00,
      "implementationEffort": "MEDIUM"
    }
  ],
  "userFeedback": {
    "helpfulnessScore": null,
    "feedbackRequested": true
  }
}
```

### Orchestrator Agent (Iteration 3)

**Input Multi-Agent Coordination:**
```json
{
  "userQuery": "Optimize our multi-cloud spending and provide automated remediation",
  "tenantId": "tenant-789",
  "complexity": "HIGH",
  "requiresCoordination": true
}
```

**Agent Output:**
```json
{
  "agentType": "Orchestrator",
  "tenantId": "tenant-789",
  "timestamp": "2024-01-31T16:00:00Z",
  "orchestrationPlan": {
    "taskDecomposition": [
      {
        "subtask": "Anomaly Detection",
        "assignedAgent": "AnomalyDetectionAgent",
        "priority": 1,
        "estimatedDuration": "2 minutes"
      },
      {
        "subtask": "Vendor Intelligence",
        "assignedAgent": "VendorIntelligenceAgent", 
        "priority": 2,
        "estimatedDuration": "3 minutes"
      },
      {
        "subtask": "Automated Remediation",
        "assignedAgent": "RemediationAgent",
        "priority": 3,
        "estimatedDuration": "5 minutes"
      }
    ],
    "coordinationStrategy": "SEQUENTIAL_WITH_FEEDBACK",
    "totalEstimatedTime": "10 minutes"
  },
  "agentResults": {
    "anomaliesFound": 3,
    "optimizationOpportunities": 7,
    "automatedActionsExecuted": 2,
    "manualReviewRequired": 1
  },
  "consolidatedRecommendations": [
    {
      "type": "MULTI_CLOUD_OPTIMIZATION",
      "description": "Migrate 3 workloads from AWS to GCP for 40% cost savings",
      "estimatedSavings": 12000.00,
      "automationStatus": "PENDING_APPROVAL"
    }
  ],
  "mcpConnections": {
    "dataSources": ["AWS Cost Explorer", "Azure Cost Management", "GCP Billing API"],
    "connectionsActive": 3,
    "dataFreshness": "< 1 hour"
  }
}
```

## Getting Started

### Prerequisites
- Node.js 18+ or Python 3.9+
- Docker and Docker Compose
- Cloud provider credentials (AWS/Azure/GCP)
- Time-series database (InfluxDB or TimescaleDB)
- Relational database (PostgreSQL)

### Installation

```bash
git clone https://github.com/your-org/auto-spend-guard.git
cd auto-spend-guard
npm install # or pip install -r requirements.txt
```

### Configuration

1. Copy environment template:
```bash
cp .env.example .env
```

2. Configure your settings in `.env`:
```bash
# Database Configuration
TIMESERIES_DB_URL=influxdb://localhost:8086
RELATIONAL_DB_URL=postgresql://localhost:5432/autospendguard

# Cloud Provider APIs
AWS_ACCESS_KEY_ID=your_key
AZURE_SUBSCRIPTION_ID=your_subscription
GCP_PROJECT_ID=your_project

# Multi-tenant Settings
TENANT_ISOLATION_MODE=strict
DATA_ENCRYPTION_KEY=your_encryption_key

# Agent Configuration
ROUTER_MODEL=gpt-4
ANOMALY_MODEL=isolation_forest
MCP_ENABLED=false
```

3. Initialize the databases:
```bash
npm run db:migrate # or python manage.py migrate
npm run db:seed    # Load sample FOCUS data
```

### Running Auto-Spend Guard

```bash
# Start all Auto-Spend Guard services
docker-compose up -d

# Run individual agents (Iteration 1)
npm run agent:router
npm run agent:workflow

# Run ML-based agents (Iteration 2)  
npm run agent:anomaly-detection
npm run agent:rca

# Run orchestrated system (Iteration 3)
npm run agent:orchestrator
```

### API Usage

```bash
# Query the Router Agent
curl -X POST http://localhost:8080/api/v1/query \
  -H "Content-Type: application/json" \
  -H "X-Tenant-ID: tenant-123" \
  -d '{"query": "Why are our AWS costs so high this month?"}'

# Get anomaly detection results
curl -X GET http://localhost:8080/api/v1/anomalies/tenant-123 \
  -H "Authorization: Bearer your_token"
```

## Contributing

1. Fork the Auto-Spend Guard repository
2. Create a feature branch (`git checkout -b feature/amazing-feature`)
3. Commit your changes (`git commit -m 'Add amazing Auto-Spend Guard feature'`)
4. Push to the branch (`git push origin feature/amazing-feature`)
5. Open a Pull Request

### Development Guidelines

- Follow FOCUS specification for all data transformations
- Ensure multi-tenant data isolation in all components
- Include unit tests for all agent logic with >80% coverage
- Document agent decision-making processes and evaluation metrics
- Test ML model performance with baseline metrics
- Add integration tests for cross-agent interactions
- Follow semantic versioning for releases

### Code Structure
```
auto-spend-guard/
├── agents/
│   ├── router/           # Iteration 1: Query classification
│   ├── workflow/         # Iteration 1: Data retrieval & LLM
│   ├── anomaly/          # Iteration 2: ML-based detection
│   ├── rca/              # Iteration 2: LLM explanations
│   └── orchestrator/     # Iteration 3: Multi-agent coordination
├── data/
│   ├── focus/            # FOCUS data schemas and validation
│   ├── storage/          # Time-series and relational adapters
│   └── mcp/              # Model Context Protocol integrations
├── evaluation/           # Agent performance and feedback systems
├── multi-tenant/         # Tenant isolation and security
└── tests/               # Comprehensive test coverage
```

## Evaluation & Monitoring

### Iteration 1 Metrics
- **Router Classification Accuracy**: >85% correct categorization
- **Response Usefulness**: User satisfaction scores >4.0/5.0
- **Data Retrieval Latency**: <2 seconds average
- **Multi-tenant Isolation**: 0 data leakage incidents

### Iteration 2 Metrics  
- **Anomaly Detection Precision**: >90%
- **Anomaly Detection Recall**: >85%
- **False Positive Rate**: <5%
- **RCA Explanation Quality**: User helpfulness >4.2/5.0

### Iteration 3 Metrics
- **End-to-end Outcome Achievement**: >80% of optimization targets met
- **Agent Coordination Efficiency**: <10 minute average resolution time
- **Automated Remediation Success**: >75% success rate
- **Cost Savings Delivered**: Measurable ROI tracking

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.

## Support

- Documentation: [Auto-Spend Guard Wiki](https://github.com/your-org/auto-spend-guard/wiki)
- Issues: [GitHub Issues](https://github.com/your-org/auto-spend-guard/issues)
- Discussions: [GitHub Discussions](https://github.com/your-org/auto-spend-guard/discussions)
- Slack: [#auto-spend-guard](https://your-workspace.slack.com/channels/auto-spend-guard)

