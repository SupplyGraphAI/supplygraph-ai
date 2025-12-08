# Geographic Concentration Analysis Agent

## Overview

The **Geographic Concentration Analysis Agent** performs a quantitative, multi-tier evaluation of an enterprise’s global supply chain to identify **country-level and regional over-concentration risks**.

By applying **Herfindahl–Hirschman Index (HHI)** calculations and deep-tier graph traversal, it exposes hidden dependency structures that are often invisible in traditional Tier-1 or Tier-2 analysis.

The agent delivers a **clear, decision-ready report** that highlights systemic vulnerabilities, enabling organizations to assess exposure, design mitigation strategies, and strengthen supply resilience before disruption occurs.


## Pain

Most companies believe their supply chain is diversified — but in reality, **hidden concentration often exists deep in Tier-3 through Tier-8 levels** of their network.

Even globally diversified enterprises can rely on a **single dominant geography** for critical upstream inputs without realizing it. In one analysis, Nvidia — widely regarded as having a highly diversified supply chain — was found to have **Tier-6 components with over 90% concentration in a single country** based on HHI analysis.

Such invisible dependencies introduce severe enterprise risks:

- Geopolitical shocks
- Export controls and sanctions
- Natural disasters
- Regional instability

Any one of these can trigger **cascading failures**: component shortages, production delays, revenue loss, and reputational damage.


## Breakthrough

Behind the scenes, SupplyGraph AI executes a node-by-node analysis of a company-centric supply graph, calculating geographic concentration at each tier using statistical benchmarks and real-world enterprise data.

User experience remains simple:

- Provide a company name
- Confirm the matched entity
- Receive a structured, fully documented concentration analysis

The final output includes:

- HHI-based concentration metrics
- Country and region dominance scores
- Dependency distribution charts
- Executive-ready risk summaries

Advanced network science, delivered through an intuitive and actionable format.


## Why SupplyGraph AI

Powered by a continuously updated knowledge graph spanning:

- 100M+ companies
- 1M+ key products
- 8,000+ industry benchmarks

SupplyGraph AI is the **first AI-native infrastructure** designed to perform deep-tier geographic dependency analysis at scale — **without requiring any proprietary data uploads from clients**.

Our platform transforms invisible structural risk into **auditable, explainable intelligence** that decision-makers can trust.


## Try the Geographic Concentration Analysis Agent (Live Chatbot)

Before integrating this agent via API, you can experience it directly through our interactive chatbot.

This live demo allows you to:

- Enter a company name
- Confirm the correct entity
- Analyze geographic concentration across the supply chain
- Identify single-country dependency and hidden exposure risks
- Preview the structure, clarity, and analytical depth of the final report

Launch the Geographic Concentration Analysis Agent  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=sg_chokepoint

To use the chatbot, you’ll first need to:

- Create a SupplyGraph AI account
- Top up your credit balance

The chatbot is powered by the **same Geographic Concentration Analysis Agent and A2A endpoints** described in this documentation.  
Credits consumed in the chatbot are deducted in the same way as API / A2A usage.

Everything you experience here can be fully embedded into your own systems through A2A integration.

## Sandbox Key Support (for Development)

This agent fully supports **Sandbox API Keys**, which allow developers to test integrations without consuming credits.

When using a Sandbox Key:
- No credits are deducted  
- The agent returns **mocked sample output** that follows the final response schema  
- Long-running processing is skipped, and results are returned instantly  
- Only input validation and structural checks are executed  

Sandbox Keys are recommended for:
- Local development  
- SDK integration testing  
- CI/CD automated tests  

⚠️ Sandbox Keys do **not** produce real analytical results and must not be used in production systems.

For a detailed comparison between Production Keys and Sandbox Keys, see the [Getting Started Guide → API Keys](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/getting-started.md#3-generate-an-a2amcp-key) section.

## API Overview

This section is intended for developers integrating the agent programmatically via the **SupplyGraph A2A (Agent-to-Agent) protocol**.


## Endpoints Summary

| Endpoint | Method | Description |
|---------|--------|-------------|
| `/api/v1/agents/sg_chokepoint/manifest` | GET | Retrieve metadata, schema, pricing, and version info |
| `/api/v1/agents/sg_chokepoint/run` | POST | Start task or manage execution via mode |


**Supported modes:**

- `mode=run` — Start a new task (default, supports streaming)
- `mode=status` — Check task progress (non-streaming)
- `mode=results` — Retrieve final output (non-streaming)


## Manifest

### Purpose

The manifest exposes information about the agent, including:

- Version
- Accepted input schema
- Output structure
- Pricing model
- Capability description


### Request

```bash
curl -X GET https://agent.supplygraph.ai/api/v1/agents/sg_chokepoint/manifest
  -H "Authorization: Bearer <YOUR_API_KEY>"
```


### Example Response

```json
{
  "agent_id": "sg_chokepoint",
  "name": "Geographic Concentration Analysis Agent",
  "version": "1.0.0",
  "description": "Performs geographic concentration analysis on enterprise supply chains",
  "input_schema": { ... },
  "output_schema": { ... },
  "pricing": { "unit": "credits", "per_run": 10 },
  "status": "active"
}
```


## Run Endpoint

### Purpose

Start a new analysis task with this agent.  
Supports both **streaming** (`stream=true`) and **non-streaming** (`stream=false`) execution.

### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/sg_chokepoint/run
  -H "Authorization: Bearer <YOUR_API_KEY>"
  -H "Content-Type: application/json"
  -d '{"text":"{{company_name}}","stream":true}'
```


### Example Response (Streaming)

| Event | Stage | Code | Description |
|------|------|------|-------------|
| stream | interpreting | THINKING | Input analysis and reasoning |
| stream | executing | TASK_ACCEPTED | Task accepted and queued |
| end | — | — | Stream completed |


### Example Event: Interpreting (THINKING)

```json
{
  "event": "stream",
  "data": {
    "task_id": "<task-id>",
    "agent": "sg_chokepoint",
    "stage": "interpreting",
    "code": "THINKING",
    "reasoning": ["Analyzing input..."],
    "timestamp": "2025-11-12T09:00:00Z",
    "is_final": false
  }
}
```


### Example Event: TASK_ACCEPTED

```json
{
  "success": true,
  "code": "TASK_ACCEPTED",
  "message": "Task accepted and queued.",
  "data": {
    "task_id": "<task-id>",
    "agent": "sg_chokepoint",
    "stage": "executing",
    "timestamp": "2025-11-12T09:00:10Z",
    "is_final": true
  }
}
```


## Status Endpoint

### Purpose

Check the processing status of an existing task.

### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/sg_chokepoint/run
  -H "Authorization: Bearer <YOUR_API_KEY>"
  -H "Content-Type: application/json"
  -d '{"mode":"status","task_id":"<task-id>"}'
```


### Example Response

```json
{
  "success": true,
  "code": "TASK_RUNNING",
  "data": {
    "task_id": "<task-id>",
    "stage": "running",
    "progress": 65.5
  }
}
```


## Results Endpoint

### Purpose

Retrieve the final output of a completed task.

### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/sg_chokepoint/run
  -H "Authorization: Bearer <YOUR_API_KEY>"
  -H "Content-Type: application/json"
  -d '{"mode":"results","task_id":"<task-id>"}'
```


### Example Response

```json
{
  "success": true,
  "code": "TASK_COMPLETED",
  "data": {
    "task_id": "<task-id>",
    "agent": "sg_chokepoint",
    "progress": 100,
    "content": "## Concentration Analysis Report ..."
  },
  "metadata": {
    "credits_used": 10
  }
}
```


## Integration Options

### Protocols

| Protocol | Description | Docs |
|------|------|------|
| **A2A (Agent-to-Agent)** | Standard interface for autonomous agent workflows | [A2A Protocol](../a2a.md) |
| **MCP (Multi-Channel Protocol)** | Enterprise orchestration layer | *(Coming Soon)* |


### Developer Interfaces

| Interface | Description | Docs |
|------|------|------|
| **Python SDK (A2A Client)** | Official wrapper for rapid A2A integration | https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk |


## Error Handling & Status Codes

### Common Errors

| Code | Description |
|------|-------------|
| UNAUTHORIZED | Invalid or missing API key |
| INSUFFICIENT_CREDITS | Not enough credits |
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
