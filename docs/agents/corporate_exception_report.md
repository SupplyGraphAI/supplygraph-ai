# Corporate Exception Report Agent


## Overview

The **Corporate Exception Report Agent** continuously monitors and analyzes companies to detect **critical exceptions, risk signals, regulatory events, and abnormal changes** that can impact operational stability, compliance posture, or reputation.

It transforms fragmented, unstructured external information into a **structured, verified, and continuously updated intelligence report**, enabling organizations to respond to corporate risks in near real-time.


## Pain

Traditional corporate monitoring is slow, fragmented, and resource-intensive.

Analysts frequently spend **10+ hours per company** gathering information from scattered public sources — and by the time a report is completed, much of the data is already outdated.

Even worse, most organizations lack:

- Continuous monitoring capability  
- Automated exception detection  
- Centralized, standardized reporting  

This creates blind spots in:

- Compliance
- Governance
- Financial exposure
- Supply chain risk
- Reputation management


## Breakthrough

The Corporate Exception Report Agent automates what previously required multiple teams and systems.

Behind the scenes, it:

- Monitors millions of enterprise signals in real time
- Detects abnormal events and exceptions
- Structures findings into standardized intelligence modules
- Continuously refreshes risk status

For the user, it’s simple:

- Provide a company name
- Confirm the matched entity
- Receive a detailed, structured exception analysis report

Typical insights include:

- Regulatory and compliance risks
- Legal or administrative issues
- Reputation and media exposure
- Operational anomalies
- Governance concerns


## Why SupplyGraph AI

Powered by:

- 100M+ global companies
- 8,000+ industry benchmarks
- 1M+ key products and entities

SupplyGraph AI delivers a **continuously updated corporate risk intelligence layer** without requiring users to upload proprietary data.

All findings are **auditable, traceable, and evidence-linked**, making this agent suitable for:

- Compliance teams
- Risk & governance functions
- Investors and analysts
- Procurement and sourcing teams
- Enterprise decision-makers


## Try the Corporate Exception Report Agent (Live Chatbot)

Before integrating via API, you can experience this agent directly through our interactive chatbot.

This live demo allows you to:

- Enter a company name
- Confirm the correct legal entity
- Generate a full exception report
- Identify hidden risks and abnormalities
- Preview the exact structure of enterprise-grade outputs

Launch the Corporate Exception Report Chatbot  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=corporate_exception

To use the chatbot, you’ll first need to:

- Create a SupplyGraph AI account
- Top up your credit balance

The chatbot is powered by the **same Corporate Exception Report Agent and A2A endpoints** described in this documentation.  
Credits consumed in the chatbot are deducted in the same way as API / A2A usage.

Everything you experience here can be fully embedded into your own systems through A2A integration.


## Sandbox Key Support (for Development)

This agent supports **Sandbox API Keys**, enabling developers to test integrations without consuming credits.

When using a Sandbox Key:
- No credits are deducted  
- The agent returns **predefined real sample data** for companies such as **Tesla** or **BYD**, depending on your input  
- Returned data is real and structured, but limited to our predefined sample set  
- Processing is accelerated, and results are returned instantly  
- Only input validation and structural checks are performed  

Sandbox Keys are ideal for:
- UI development  
- Validating your processing logic against realistic datasets  
- SDK / API behavior testing  

⚠️ Sandbox results are not dynamically generated — they come from a predefined dataset and must not be used in production analytics.

For a full comparison of Production vs. Sandbox keys, see  
[Getting Started Guide → API Keys](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/getting-started.md#3-generate-an-a2amcp-key).




## API Overview

This section provides an overview of the **A2A (Agent-to-Agent)** interface used to integrate this agent into other systems.


## Endpoints Summary

| Endpoint | Method | Description |
|---------|--------|-------------|
| `/api/v1/agents/corporate_exception_report/manifest` | GET | Retrieve metadata, schema, pricing, and version info |
| `/api/v1/agents/corporate_exception_report/run` | POST | Execute or manage tasks via `mode` |


**Supported modes:**

- `mode=run` (default) — start a new task
- `mode=status` — check task progress
- `mode=results` — retrieve task output


## Manifest

### Request

```bash
curl -X GET https://agent.supplygraph.ai/api/v1/agents/corporate_exception_report/manifest
  -H "Authorization: Bearer <YOUR_API_KEY>"
```


### Example Response

```json
{
  "agent_id": "corporate_exception_report",
  "name": "Corporate Exception Report Agent",
  "version": "1.0.0",
  "description": "Detects and analyzes corporate anomalies and exception signals",
  "input_schema": { ... },
  "output_schema": { ... },
  "pricing": { "unit": "credits", "per_run": 10 },
  "status": "active"
}
```


## Run Endpoint

### Purpose

Start a new task with this agent.  
Supports structured chapter control via the `chapter_name` parameter.

### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/corporate_exception_report/run
  -H "Authorization: Bearer <YOUR_API_KEY>"
  -H "Content-Type: application/json"
  -d '{
        "text": "Tesla, Inc. United States",
        "chapter_name": "ALL",
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
    "agent": "corporate_exception_report",
    "stage": "interpreting",
    "content": "Here are the companies we’ve identified.\nCompany Name: Tesla, Inc.\nCountry: United States\nPlease reply [Yes] or [No] to confirm."
  }
}
```


## chapter_name Parameter

The `chapter_name` field controls which types of exception domains are generated.

```json
{
  "type": "string",
  "enum": [
    "ALL",
    "Profile",
    "License",
    "GovRel",
    "R&D",
    "Reputation",
    "Ops",
    "GeneralRisks",
    "Cost",
    "Compliance",
    "Competition",
    "Brand",
    "HR"
  ],
  "default": "ALL"
}
```


## Status Endpoint

### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/corporate_exception_report/run
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
    "agent": "corporate_exception_report",
    "stage": "running",
    "progress": 65.5
  }
}
```


## Results Endpoint

### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/corporate_exception_report/run
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
    "agent": "corporate_exception_report",
    "progress": 100,
    "content": "## Corporate Exception Analysis..."
  },
  "metadata": {
    "credits_used": 10
  }
}
```


## Make Your First A2A Call

Typical workflow:

1. Start with `mode=run`
2. Confirm company (if prompted)
3. Poll via `mode=status`
4. Retrieve report via `mode=results`


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
|------|-------------|
| UNAUTHORIZED | Invalid or missing API key |
| INSUFFICIENT_CREDITS | Not enough balance |
| RATE_LIMITED | Too many requests |
| INVALID_REQUEST | Input outside agent scope |


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
