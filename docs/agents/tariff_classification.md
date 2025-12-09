# Customs Classification Agent


## Overview

The **Customs Classification Agent** automatically maps product descriptions to the **most accurate HS / HTS codes** in seconds.

It eliminates manual lookup, reduces classification errors, and enables downstream **duty calculation, compliance analysis, and tariff optimization** at scale.


## Pain

For customs specialists and trade compliance teams, determining the correct HTS code is a critical but extremely demanding task:

- Manually comparing product descriptions to complex classification rules
- Navigating thousands of pages in tariff schedules
- Interpreting vague or ambiguous product language
- Handling cases where one product may match **multiple potential codes**

This process is:

- Time-consuming
- Mentally exhausting
- Highly error-prone
- Inconsistent across teams

A single incorrect classification can result in **penalties, shipment delays, revenue loss, or compliance risk**.


## Breakthrough

The Customs Classification Agent reduces manual work from **hours (or days) to seconds**.

By leveraging:

- Real-time global tariff databases
- Domain-specific classification knowledge
- Graph-based reasoning over product attributes

SupplyGraph AI delivers:

- Multiple candidate HTS codes
- Confidence scores for each option
- Traceable reasoning paths
- Evidence-backed decision support

This not only improves speed, but also **unlocks tariff optimization opportunities** by making alternative classifications transparent and comparable.


## Why SupplyGraph AI

SupplyGraph AI combines:

- Official HTS / HS source data
- Graph intelligence across products and industries
- Structured customs reasoning models
- Continuous regulatory updates

Every classification is:

- Explainable
- Auditable
- Consistent
- Enterprise-grade

This dramatically reduces compliance risk while creating a reliable foundation for tariff calculation, sourcing optimization, and trade strategy planning.


## Try the Customs Classification Agent (Live Chatbot)

Before integrating via API, you can experience this agent instantly through our interactive classification chatbot.

This live demo allows you to:

- Enter a product description in natural language
- Receive the most relevant HS / HTS code candidates
- Understand why each code is suggested
- Explore alternative classifications
- Reduce ambiguity and manual lookup work

Launch the Customs Classification Chatbot  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=tariff_classification

To use the chatbot, you’ll first need to:

- Create a SupplyGraph AI account
- Top up your credit balance

This chatbot is powered by the **same Customs Classification Agent and A2A endpoints** described below.  
Credits used in the chatbot are deducted in the same way as API / A2A usage.

Everything you experience here can be directly embedded into your own systems.

## Sandbox Key Support (for Development)

This agent fully supports **Sandbox API Keys**, which allow developers to test integrations without consuming credits.

When using a Sandbox Key:
- No credits are deducted  
- The agent returns **mocked sample output** that follows the final response schema  
- Returned content is deterministic and does not reflect real-world data  
- Long-running computation is skipped and results are returned instantly  
- Only input validation and request-format checks are executed  

Sandbox Keys are recommended for:
- Local development  
- SDK / API integration testing  
- CI/CD automation  

⚠️ Sandbox Keys do **not** produce real analytical results and must not be used in production systems.

For a full comparison of Production vs. Sandbox keys, see  
[Getting Started Guide → API Keys](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/getting-started.md#3-generate-an-a2amcp-key).


## API Overview

This section provides an overview of the **A2A (Agent-to-Agent)** API structure and usage.


## Endpoints Summary

| Endpoint | Method | Description |
|--------|------|-------------|
| `/api/v1/agents/tariff_classification/manifest` | GET | Retrieve metadata, schema, pricing, and version info |
| `/api/v1/agents/tariff_classification/run` | POST | Execute or manage tasks via `mode` |

**Supported modes:**

- `mode=run` — start a new task (supports streaming)
- `mode=status` — check task progress
- `mode=results` — retrieve task output


## Manifest

### Request

```bash
curl -X GET https://agent.supplygraph.ai/api/v1/agents/tariff_classification/manifest
  -H "Authorization: Bearer <YOUR_API_KEY>"
```

### Example Response

```json
{
  "agent_id": "tariff_classification",
  "name": "Customs Classification Agent",
  "version": "1.0.0",
  "description": "Automatically maps product descriptions to correct HS/HTS codes with confidence scoring and reasoning.",
  "input_schema": { ... },
  "output_schema": { ... },
  "pricing": { "unit": "credits", "per_run": 2 },
  "status": "active"
}
```


## Run Endpoint


### Purpose

Start a new classification task.

This endpoint supports both:

- Streaming (`stream=true`)
- Non-streaming (`stream=false`)


### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/tariff_classification/run
  -H "Authorization: Bearer <YOUR_API_KEY>"
  -H "Content-Type: application/json"
  -d '{
        "text": "Cotton T-shirts for women, 100% cotton, made in Mexico",
        "stream": true
      }'
```


## Text Input Requirements

The `text` field should contain a **clear and detailed natural-language product description**.

- The more specific the description, the higher the classification precision
- Country-of-origin may be included, but is **optional**
- No HTS knowledge is required from the user


## Example Response (Streaming)

| Event | Stage | Code | Description |
|------|------|------|-------------|
| stream | interpreting | THINKING | Agent is analyzing product attributes |
| stream | executing | TASK_ACCEPTED | Task accepted and queued |
| end | — | — | Stream completed |


### Event 1 — Interpreting (THINKING)

```json
{
  "event": "stream",
  "data": {
    "task_id": "<task-id>",
    "agent": "tariff_classification",
    "stage": "interpreting",
    "code": "THINKING",
    "reasoning": ["Analyzing product description..."],
    "timestamp": "2025-11-12T09:00:00Z",
    "is_final": false
  }
}
```


### Event 2 — Task Accepted

```json
{
  "success": true,
  "code": "TASK_ACCEPTED",
  "message": "Task accepted and queued.",
  "data": {
    "task_id": "<task-id>",
    "agent": "tariff_classification",
    "stage": "executing",
    "timestamp": "2025-11-12T09:00:10Z",
    "is_final": true
  }
}
```


### Stream End

```
event: end
data: [DONE]
```


## Handling WAITING_USER

If the Agent returns `code = "WAITING_USER"`, additional confirmation or clarification is required.

To continue, you **must include the original `task_id`** in the follow-up request.  
Otherwise, the system will treat the message as a **new task**.


## Status Endpoint


### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/tariff_classification/run
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
    "agent": "tariff_classification",
    "stage": "running",
    "progress": 72.4
  }
}
```


## Results Endpoint


### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/tariff_classification/run
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
    "agent": "tariff_classification",
    "content": {
      "type": "result",
      "data": {
        "classification_results": [
          {
            "hts_code": "6109.10.00.40",
            "confidence_score": 0.90,
            "reasoning": "The product is a knitted cotton T-shirt for women. This subheading explicitly covers women's cotton T-shirts.",
            "description": "T-shirts, singlets, tank tops and similar garments, knitted or crocheted > Of cotton > Women's or girls' > Other"
          }
        ],
        "country_of_origin": "Mexico"
      }
    }
  },
  "metadata": {
    "agent": "tariff_classification",
    "credits_used": 2
  }
}
```


## `content` Field Explanation

On successful execution (`TASK_COMPLETED`), the `content` object contains:

```json
"content": {
  "type": "result",
  "data": {
    "classification_results": [ ... ],
    "country_of_origin": "<if detected>"
  }
}
```

Each item in `classification_results` includes:

- `hts_code` — Suggested HTS code
- `confidence_score` — Classification confidence (0–1)
- `reasoning` — Why this code matches
- `description` — Official HTS description

This structure is designed for **direct downstream handoff** to:

- U.S. Tariff Calculation Agent (`tariff_calc`)
- Tariff Optimization workflows
- Compliance verification pipelines


## Make Your First A2A Call

Typical workflow:

1. Start classification with `mode=run`
2. Monitor progress with `mode=status`
3. Retrieve result with `mode=results`
4. (Optional) Pass HTS result to `tariff_calc`


## Integration Options


### Protocols

| Protocol | Description | Docs |
|------|------|------|
| **A2A (Agent-to-Agent)** | Native protocol for autonomous multi-agent workflows | [A2A Protocol](../a2a.md) |
| **MCP (Multi-Channel Protocol)** | Next-generation enterprise orchestration standard | *(Coming Soon)* |


### Developer Interfaces

| Interface | Description | Docs |
|------|------|------|
| **Python SDK (A2A Client)** | Official wrapper for rapid integration | https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk |


## Error Handling & Rate Limits


### Common Error Codes

| Code | Description |
|------|------------|
| UNAUTHORIZED | Missing or expired API key |
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
