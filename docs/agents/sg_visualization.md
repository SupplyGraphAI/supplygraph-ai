# Enterprise Supply Graph Agent


## Overview

The **Enterprise Supply Graph Agent** generates a multi-tier, company-centric global supply chain graph for any enterprise in the world.

It automatically maps relationships across:

- Tier-1 suppliers
- Tier-N upstream entities
- Products and sub-components
- Regions and countries
- Raw materials and critical inputs

This agent transforms fragmented supply networks into a **living, explorable, and actionable graph** — enabling organizations to see, understand, and manage their real dependencies for the first time.


## Pain

Most companies operate with **severely limited visibility** into their true supply networks.

Without deep-tier transparency:

- Hidden single-country dependencies go undetected  
- Alternatives remain invisible  
- Regional concentration risks accumulate silently  
- Geopolitical or regulatory shocks cause sudden disruption  
- Incident response is reactive instead of strategic  

Traditional tools only reveal Tier-1 suppliers at best — leaving 80–90% of the real risk surface invisible.


## Breakthrough

The Enterprise Supply Graph Agent solves this by:

- Expanding a company-centered graph node-by-node
- Linking suppliers, products, regions, and materials across multiple tiers
- Structuring dependencies into analyzable paths and clusters
- Creating the foundation for:
  - Concentration risk analysis
  - Alternative supplier discovery
  - Impact simulation
  - Disruption forecasting

For the user, the experience is simple:

1. Enter a company name
2. Confirm the correct entity
3. Instantly visualize an interactive, multi-tier supply network

What was once invisible becomes navigable, measurable, and actionable.


## Why SupplyGraph AI

Powered by:

- 100M+ global companies
- 1M+ key products and components
- 8,000+ industry benchmarks
- Continuous monitoring, 24/7

SupplyGraph AI delivers the **first truly intelligent, real-time, multi-tier enterprise supply graph** in the world — turning hidden dependency structures into strategic advantage.


## Try the Enterprise Supply Graph Agent (Live Chatbot)

Before integrating this agent via API, you can experience it directly through our interactive visualization chatbot.

This live demo allows you to:

- Enter a company name
- Automatically generate a multi-tier supply graph (Tier-1 to deep-tier)
- Visualize product, supplier, and regional dependencies
- Identify potential concentration and exposure risks
- Explore alternative sourcing pathways
- Experience the depth and structure of the intelligence graph

Launch the Enterprise Supply Graph Chatbot  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=sg_visualization

To use the chatbot, you’ll first need to:

- Create a SupplyGraph AI account
- Top up your credit balance

This chatbot is powered by the **same Enterprise Supply Graph Agent and A2A endpoints** described in this documentation.  
Credits used in the chatbot are deducted in the same way as API / A2A usage.

Everything you experience here can be fully embedded into your own system through A2A integration.


## API Overview

This section provides an overview of the **A2A (Agent-to-Agent)** interface for this agent.


## Endpoints Summary

| Endpoint | Method | Description |
|---------|--------|-------------|
| `/api/v1/agents/sg_visualization/manifest` | GET | Retrieve metadata, schema, pricing, and version information |
| `/api/v1/agents/sg_visualization/run` | POST | Execute or manage tasks via `mode` |


**Supported modes:**

- `mode=run` — start a new task (supports streaming)
- `mode=status` — check task progress
- `mode=results` — retrieve completed output


## Manifest

### Request

```bash
curl -X GET https://agent.supplygraph.ai/api/v1/agents/sg_visualization/manifest
  -H "Authorization: Bearer <YOUR_API_KEY>"
```

### Example Response

```json
{
  "agent_id": "sg_visualization",
  "name": "Enterprise Supply Graph Agent",
  "version": "1.0.0",
  "description": "Generates a multi-tier enterprise supply chain dependency graph",
  "input_schema": { ... },
  "output_schema": { ... },
  "pricing": { "unit": "credits", "per_run": 10 },
  "status": "active"
}
```


## Run Endpoint


### Purpose

Start a new task with this agent.  
Supports both **streaming (`stream=true`)** and **non-streaming (`stream=false`)** modes.


### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/sg_visualization/run
  -H "Authorization: Bearer <YOUR_API_KEY>"
  -H "Content-Type: application/json"
  -d '{
        "text": "{{example_company_name}}",
        "stream": true
      }'
```


### Example Response (Streaming)

| Event | Stage | Code | Description |
|------|------|------|-------------|
| stream | interpreting | THINKING | Agent is analyzing input and expanding the graph |
| stream | executing | TASK_ACCEPTED | Task accepted and queued for processing |
| end | — | — | Stream completed |


#### Event 1 — Interpreting (THINKING)

```json
{
  "event": "stream",
  "data": {
    "task_id": "<task-id>",
    "agent": "sg_visualization",
    "stage": "interpreting",
    "code": "THINKING",
    "reasoning": ["Analyzing input and identifying target entity..."],
    "timestamp": "2025-11-12T09:00:00Z",
    "is_final": false
  }
}
```


#### Event 2 — Task Accepted

```json
{
  "success": true,
  "code": "TASK_ACCEPTED",
  "message": "Task accepted and queued.",
  "data": {
    "task_id": "<task-id>",
    "agent": "sg_visualization",
    "stage": "executing",
    "code": "TASK_ACCEPTED",
    "timestamp": "2025-11-12T09:00:10Z",
    "is_final": true
  },
  "metadata": {
    "timestamp": "2025-11-12T09:00:10Z"
  },
  "errors": null
}
```


#### Stream End

```
event: end
data: [DONE]
```


### Example Response (Non-Streaming)

```json
{
  "success": true,
  "code": "TASK_ACCEPTED",
  "message": "Task accepted and queued for execution.",
  "data": {
    "task_id": "<task-id>",
    "agent": "sg_visualization",
    "stage": "executing",
    "code": "TASK_ACCEPTED",
    "progress": 0,
    "timestamp": "2025-11-12T09:00:10Z",
    "is_final": true
  },
  "metadata": {
    "agent": "sg_visualization",
    "timestamp": "2025-11-12T09:00:10Z"
  },
  "errors": null
}
```


## Status Endpoint


### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/sg_visualization/run
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
    "agent": "sg_visualization",
    "stage": "running",
    "progress": 65.5
  }
}
```


## Results Endpoint


### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/sg_visualization/run
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
    "agent": "sg_visualization",
    "progress": 100,
    "content": "## Generated multi-tier supply graph output"
  },
  "metadata": {
    "credits_used": 10
  }
}
```


## Make Your First A2A Call

Typical workflow:

1. Start the task with `mode=run`
2. Follow reasoning (if streaming is enabled)
3. Poll using `mode=status`
4. Retrieve the final output via `mode=results`


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
