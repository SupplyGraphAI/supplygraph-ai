# Due Diligence Agent


## Overview

The **Due Diligence Agent** generates a comprehensive, structured, and continually updated intelligence report for any company worldwide.

It consolidates fragmented public data — including corporate registration, legal records, subsidiaries, IP assets, compliance, financial disclosures, workforce, and regulatory signals — into a **standardized, enterprise-grade due diligence dossier**.

This agent is designed for:

- M&A screening
- Supplier onboarding
- Investment research
- Risk and compliance reviews
- Strategic partner evaluation


## Pain

Traditional due diligence is slow, fragmented, and expensive.

Analysts often spend **10+ hours per company** collecting unstructured information from dozens of sources — and by the time the report is finished, much of the data is already outdated.

Key challenges include:

- Manual data aggregation
- Inconsistent formats
- Incomplete coverage
- Lack of continuous monitoring
- High research costs

This creates blind spots in strategic decision-making and risk management.


## Breakthrough

The Due Diligence Agent automates a process that previously required multiple teams and tools.

Behind the scenes, it:

- Aggregates data from a global corporate graph
- Resolves entities and normalizes records
- Structures information into standardized modules
- Continuously refreshes and updates changes

For users, the workflow is simple:

1. Provide a company name
2. Confirm the matched entity
3. Receive a complete or chapter-specific due diligence report

Results are:

- Structured
- Auditable
- Enterprise-ready
- Continuously updated


## Why SupplyGraph AI

Powered by:

- 100M+ global companies
- 8,000+ industry benchmarks
- 1M+ key products and entities

SupplyGraph AI delivers **institution-grade due diligence intelligence** without requiring you to upload any proprietary internal data.

Every insight is:

- Evidence-linked
- Contextualized
- Continuously monitored
- Built for compliance and risk teams


## Try the Due Diligence Agent (Live Chatbot)

Before integrating via API, you can experience this agent directly through our interactive chatbot.

This live demo allows you to:

- Enter a company name
- Confirm the correct legal entity
- Generate a full or chapter-specific report
- Preview the structure and quality of the final output

Launch the Due Diligence Chatbot  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=due_diligence_report

To use the chatbot, you’ll first need to:

- Create a SupplyGraph AI account
- Top up your credit balance

The chatbot is powered by the **same Due Diligence Agent and A2A endpoints** described in this documentation.  
Credits consumed in the chatbot are deducted in the same way as API / A2A usage.

Everything you experience here can be fully embedded into your own system through A2A integration.


## API Overview

This section provides an overview of the **A2A (Agent-to-Agent)** interface.


## Endpoints Summary

| Endpoint | Method | Description |
|---------|--------|-------------|
| `/api/v1/agents/due_diligence_report/manifest` | GET | Retrieve metadata, schema, pricing, and version information |
| `/api/v1/agents/due_diligence_report/run` | POST | Execute or manage tasks via `mode` |


**Supported modes:**

- `mode=run` — start a new task
- `mode=status` — check task progress
- `mode=results` — retrieve completed output


## Manifest

### Request

```bash
curl -X GET https://agent.supplygraph.ai/api/v1/agents/due_diligence_report/manifest
  -H "Authorization: Bearer <YOUR_API_KEY>"
```

### Example Response

```json
{
  "agent_id": "due_diligence_report",
  "name": "Due Diligence Agent",
  "version": "1.0.0",
  "description": "Generates structured and continuously updated company due diligence intelligence",
  "input_schema": { ... },
  "output_schema": { ... },
  "pricing": { "unit": "credits", "per_run": 10 },
  "status": "active"
}
```


## Run Endpoint

### Purpose

Start a new due diligence task.  
Supports **entity resolution + chapter-level control** via `chapter_name`.

### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/due_diligence_report/run
  -H "Authorization: Bearer <YOUR_API_KEY>"
  -H "Content-Type: application/json"
  -d '{
        "text": "Tesla, Inc. United States",
        "stream": false
      }'
```

### Example Response (WAITING_USER)

```json
{
  "success": false,
  "code": "WAITING_USER",
  "data": {
    "task_id": "<task-id>",
    "agent": "due_diligence_report",
    "stage": "interpreting",
    "content": "Here are the companies we’ve identified.\nCompany Name: Tesla, Inc.\nCountry: United States\nPlease reply [Yes] or [No] to confirm."
  }
}
```


## chapter_name Parameter

The `chapter_name` parameter allows you to generate **specific sections** of the report instead of the full report.

```json
{
  "type": "string",
  "enum": [
    "ALL",
    "Company Registration Information",
    "Branch Offices",
    "Corporate Brand Initiatives",
    "Administrative Sanctions",
    "Software Copyright Details",
    "Outbound Investments",
    "Financing Activities",
    "Competitor Analysis",
    "Subsidiary Companies",
    "Trademark Portfolio",
    "Patent Holdings",
    "Website Registrations",
    "Court Judgments",
    "Shareholder Structure",
    "Senior Management Team",
    "Administrative Permits",
    "Court Hearing Notices",
    "Court Notices",
    "Equity Pledges",
    "Mobile Applications",
    "Copyrighted Works",
    "Equity Freezes",
    "Chattel Mortgages",
    "WeChat Official Accounts",
    "Tendering and Bidding Activities",
    "Qualification Certificates",
    "Engineering Irregularities",
    "Major Regulatory Violations",
    "Compensation and Benefits",
    "Enforcement Targets",
    "Supplier Network",
    "Credit Ratings",
    "Tax Offenses",
    "Regulatory Spot Checks",
    "Import-Export Credit Records",
    "Regulatory Actions",
    "Granted Government Subsidies",
    "Eligible Government Subsidies",
    "Consolidated Statements of Operations",
    "Income Statement",
    "Statement of Cash Flows",
    "Consolidated Balance Sheets"
  ],
  "default": "ALL"
}
```


## Status Endpoint

### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/due_diligence_report/run
  -H "Authorization: Bearer <YOUR_API_KEY>"
  -H "Content-Type: application/json"
  -d '{
        "mode": "status",
        "task_id": "<task-id>"
      }'
```

### Example Response

```json
{
  "success": true,
  "code": "TASK_RUNNING",
  "data": {
    "task_id": "<task-id>",
    "agent": "due_diligence_report",
    "stage": "running",
    "progress": 65.5
  }
}
```


## Results Endpoint

### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/due_diligence_report/run
  -H "Authorization: Bearer <YOUR_API_KEY>"
  -H "Content-Type: application/json"
  -d '{
        "mode": "results",
        "task_id": "<task-id>"
      }'
```

### Example Response

```json
{
  "success": true,
  "code": "TASK_COMPLETED",
  "data": {
    "task_id": "<task-id>",
    "agent": "due_diligence_report",
    "progress": 100,
    "content": "## Due Diligence Analysis..."
  },
  "metadata": {
    "credits_used": 10
  }
}
```


## Make Your First A2A Call

Typical workflow:

1. Start the task with `mode=run`
2. Confirm company (if prompted)
3. Poll using `mode=status`
4. Retrieve the report via `mode=results`


## Integration Options


### Protocols

| Protocol | Description | Docs |
|------|------|------|
| **A2A (Agent-to-Agent)** | Autonomous workflow integration standard | [A2A Protocol](../a2a.md) |
| **MCP (Multi-Channel Protocol)** | Enterprise orchestration layer | *(Coming Soon)* |


### Developer Interfaces

| Interface | Description | Docs |
|------|------|------|
| **Python SDK (A2A Client)** | Official wrapper for rapid integration | https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk |


## Error Handling & Rate Limits


### Common Error Codes

| Code | Description |
|------|--------------|
| UNAUTHORIZED | Missing or expired API key |
| INSUFFICIENT_CREDITS | Not enough credits |
| RATE_LIMITED | Too many requests |
| INVALID_REQUEST | Input outside agent’s scope |


### Stage-Specific Codes

```
interpreting:
  INTERPRETING
  INVALID_REQUEST
  UNAUTHORIZED
  WAITING_USER

executing:
  TASK_ACCEPTED
  TASK_RUNNING

completed:
  TASK_COMPLETED
  TASK_FAILED

cancelled:
  TASK_CANCELLED
```


Maintainer: info@supplygraph.ai  
License: Proprietary / Internal  
© 2025 SupplyGraph AI. All rights reserved.
