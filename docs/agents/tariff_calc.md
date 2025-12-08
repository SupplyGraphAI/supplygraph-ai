# U.S. Tariff Calculation Agent


## Overview

The **U.S. Tariff Calculation Agent** automatically calculates **U.S. import duty rates** and all **applicable additional tariffs** based on:

- Product description or HTS code
- Country of origin
- Applicable trade measures (e.g. Chapter 99, Section 301 / 232 / 201)

It delivers **accurate, auditable, regulation-aware duty calculations** in seconds — enabling instant compliance validation, cost modeling, and sourcing optimization.


## Pain

For customs brokers, procurement teams, and trade compliance specialists, determining a product’s duty burden usually requires:

- Manually searching the HTSUS
- Parsing Chapter 99 notes
- Tracking trade measures and exemptions
- Interpreting complex legal classification rules
- Verifying country-of-origin impacts

This process can take **hours per product**, is highly **error-prone**, and often produces **inconsistent or incomplete results** — exposing companies to penalties, misclassification risks, and unexpected landed costs.


## Breakthrough

SupplyGraph AI transforms this process.

The U.S. Tariff Calculation Agent:

- Automatically determines the correct HS / HTS classification (if not provided)
- Applies real-time base duty rates
- Checks and applies all relevant Chapter 99 & special tariff measures
- Evaluates country-of-origin impacts
- Generates a clear, transparent duty analysis

Manual work drops from **hours to seconds**.

The result is not just a number — it is a **traceable, explainable, policy-backed tariff decision**.


## Why SupplyGraph AI

Powered by:

- Real-time global tariff & trade datasets
- HTS + Chapter 99 rule intelligence
- Graph-based product and country reasoning
- Continuously updated regulatory tracking

Every tariff result delivered by SupplyGraph AI is:

- Explainable
- Evidence-backed
- Policy-aware
- Enterprise-grade

This dramatically reduces compliance risk while enabling smarter sourcing and pricing decisions at scale.


## Try the U.S. Tariff Calculation Agent (Live Chatbot)

Before integrating via API, you can experience this agent through our interactive tariff analysis chatbot.

This live demo allows you to:

- Describe your product in natural language
- Identify the most relevant HTS codes
- Apply the correct country of origin
- Calculate U.S. base duties and applicable additional tariffs (including Chapter 99)
- See how duties impact landed cost in real time
- Compare alternative sourcing or classification scenarios

Launch the U.S. Tariff Calculation Chatbot  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=tariff_calc

To use the chatbot, you’ll need to:

- Create a SupplyGraph AI account
- Top up your credit balance

This chatbot is powered by the **same U.S. Tariff Calculation Agent and A2A endpoints** described below.  
Credits are deducted in the same way as API / A2A usage.

Everything you experience here can be embedded into your own system.

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

This section provides an overview of the **A2A (Agent-to-Agent)** interface used by this agent.


## Endpoints Summary

| Endpoint | Method | Description |
|---------|--------|-------------|
| `/api/v1/agents/tariff_calc/manifest` | GET | Retrieve metadata, schema, pricing, and version info |
| `/api/v1/agents/tariff_calc/run` | POST | Execute or manage tasks via `mode` |

**Supported modes:**

- `mode=run` — start a new task (supports streaming)
- `mode=status` — check task progress
- `mode=results` — retrieve completed output


## Manifest

### Request

```bash
curl -X GET https://agent.supplygraph.ai/api/v1/agents/tariff_calc/manifest
  -H "Authorization: Bearer <YOUR_API_KEY>"
```

### Example Response

```json
{
  "agent_id": "tariff_calc",
  "name": "U.S. Tariff Calculation Agent",
  "version": "1.0.0",
  "description": "Calculates U.S. customs duties by combining HTS base rates with applicable Chapter 99 measures, providing transparent, regulation-aware tariff outcomes.",
  "input_schema": { ... },
  "output_schema": { ... },
  "pricing": { "unit": "credits", "per_run": 10 },
  "status": "active"
}
```


## Run Endpoint


### Purpose

Start a new tariff calculation task.

This endpoint supports both:

- Streaming (`stream=true`)
- Non-streaming (`stream=false`)


### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/tariff_calc/run
  -H "Authorization: Bearer <YOUR_API_KEY>"
  -H "Content-Type: application/json"
  -d '{
        "text": "Cotton T-shirts for women, 100% cotton, made in Mexico",
        "stream": true
      }'
```


## Text Input Requirements

The `text` field contains the natural-language instructions that define the tariff calculation.

**Minimum required information:**

- Either:
  - a **10-digit HTS code** (e.g. `5601.21.0010`)  
  **or**
  - a detailed **product description**
- A **country of origin** must be included

**Optional parameters:**

- Product weight
- Quantity
- Declared customs value

If optional fields are omitted, the Agent applies standardized defaults for estimation purposes.

**Example:**

> “Calculate import duties for 5601.21.0010, country of origin China, shipment value 200 USD, 50 kg.”


## Example Response (Streaming)

| Event | Stage | Code | Description |
|------|------|------|-------------|
| stream | interpreting | THINKING | Agent is analyzing product and policy context |
| stream | executing | TASK_ACCEPTED | Task accepted and queued for execution |
| end | — | — | Stream completed |


### Event 1 — Interpreting (THINKING)

```json
{
  "event": "stream",
  "data": {
    "task_id": "<task-id>",
    "agent": "tariff_calc",
    "stage": "interpreting",
    "code": "THINKING",
    "reasoning": ["Analyzing product and country of origin..."],
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
    "agent": "tariff_calc",
    "stage": "executing",
    "code": "TASK_ACCEPTED",
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

If the response includes `code = "WAITING_USER"`, the agent needs additional information or confirmation.

To continue execution, you **must**:

- Send the follow-up message
- Include the original `task_id`

Without the correct `task_id`, the message will be treated as a **new request** and the original task will remain paused.


## Status Endpoint


### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/tariff_calc/run
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
    "agent": "tariff_calc",
    "stage": "running",
    "progress": 65.5
  }
}
```


## Results Endpoint


### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/tariff_calc/run
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
    "agent": "tariff_calc",
    "content": {
      "type": "tariff_analysis",
      "data": {
        "hts_code": "8415.90.80.25",
        "product": "Air conditioning evaporator coils",
        "country_of_origin": "Japan",
        "base_duty_rate": "X%",
        "additional_measures": ["Chapter 99"],
        "total_effective_duty": "X.XX%",
        "explanation": "..."
      }
    }
  },
  "metadata": {
    "credits_used": 10
  }
}
```


## Make Your First A2A Call

Typical workflow:

1. Start the task with `mode=run`
2. (Optional) receive live reasoning via streaming
3. Poll using `mode=status`
4. Retrieve the final result using `mode=results`


## Integration Options


### Protocols

| Protocol | Description | Docs |
|------|------|------|
| **A2A (Agent-to-Agent)** | Native communication protocol for autonomous agent workflows | [A2A Protocol](../a2a.md) |
| **MCP (Multi-Channel Protocol)** | Next-generation orchestration protocol for complex environments | *(Coming Soon)* |


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
| INVALID_REQUEST | Input outside agent’s task scope |


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
