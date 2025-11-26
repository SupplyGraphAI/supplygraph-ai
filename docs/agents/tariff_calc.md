# U.S. Tariff Calculation Agent

### Overview
Automatically calculates U.S. duty rates and applicable additional tariffs based on the product’s HTS code and country of origin.

### Pain
For customs and trade compliance specialists, determining an item’s duty rate and applicable tariffs usually means manually searching through thousands of pages of tariff schedules.  
This process is not only time-consuming but also error-prone, leading to inconsistent or incomplete results.

### Breakthrough
**AutoTariff** automates the calculation of duty rates and applicable additional tariffs, cutting analysis time from **hours to seconds**.  
It supports accurate decision-making, enables tariff optimization, and ensures compliance with evolving customs regulations.

### Why Us
SupplyGraph AI integrates **real-time global tariff databases** with **graph-based reasoning**, producing precise and auditable calculations.  
Each result is backed by verifiable source data, ensuring full transparency and minimizing compliance risk at scale.



## API Overview
This section provides an overview of the A2A API structure and usage.

### Endpoints Summary
| Endpoint | Method | Description |
|-----------|---------|-------------|
| `/api/v1/agents/tariff_calc/manifest` | GET | Retrieve metadata, schema, pricing, and version info. |
| `/api/v1/agents/tariff_calc/run` | POST | Execute or manage tasks via `mode`. |

**Supported modes:**  
- `mode=run` (default) — start a new task (supports streaming)  
- `mode=status` — check task progress (non-streaming)  
- `mode=results` — retrieve task output (non-streaming)


## Manifest

### Purpose
The Manifest provides metadata about the Agent including version, schema definitions, pricing, and available capabilities.

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
  "description": "Calculates U.S. customs duties by combining HTS base rates with applicable Chapter 99 measures, providing transparent, rule-based tariff outcomes.",
  "input_schema": { ... },
  "output_schema": { ... },
  "pricing": { "unit": "credits", "per_run": 10 },
  "status": "active"
}
```


## Run Endpoint

### Purpose
Start a new task with this Agent.  
This endpoint supports both streaming (`stream=true`) and non-streaming (`stream=false`) modes.

### Request
```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/tariff_calc/run
  -H "Authorization: Bearer <YOUR_API_KEY>"
  -H "Content-Type: application/json"
  -d '{"text": "Cotton T-shirts for women, 100%cotton, made in Mexico", "stream": true}'
```

### Text Input Requirements

The `text` field contains the natural-language instructions or query used to initiate a tariff or duty calculation task.

**Input guidelines:**

* The text **must** include either:

  * a **10-digit HTS code** (e.g., `5601.21.0010`), **or**
  * a **product description** from which the Agent will determine the appropriate HS/HTS classification.
* The **country of origin** must also be specified.
* Optional details may include:

  * product weight
  * quantity
  * merchandise value
* If optional values are omitted, the Agent will automatically apply system defaults to perform a mock-up or estimated duty calculation.

**Example:**

> “Calculate import duties for 5601.21.0010, country of origin China, shipment value 200 USD, 50 kg.”

### Example Response (Streaming)

The example below demonstrates a typical streaming sequence with intermediate reasoning and task acceptance events.

| Event | Stage | Code | Description |
|--------|--------|------|-------------|
| stream | interpreting | THINKING | Agent is analyzing input and generating reasoning. |
| stream | executing | TASK_ACCEPTED | Task accepted and queued for processing. |
| end | — | — | Stream completed. |

#### Event 1 — Interpreting (THINKING)
```json
{
  "event": "stream",
  "data": {
    "task_id": "<task-id>",
    "agent": "tariff_calc",
    "stage": "interpreting",
    "code": "THINKING",
    "reasoning": ["Analyzing input..."],
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
    "agent": "tariff_calc",
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
    "agent": "tariff_calc",
    "stage": "executing",
    "code": "TASK_ACCEPTED",
    "progress": 0,
    "timestamp": "2025-11-12T09:00:10Z",
    "is_final": true
  },
  "metadata": {
    "agent": "tariff_calc",
    "timestamp": "2025-11-12T09:00:10Z"
  },
  "errors": null
}
```

### Response Structure Explained
- **Envelope:** `{ success, code, message, data, metadata, errors }`
- **Data Fields:**
  - `task_id`: unique identifier for the task
  - `stage`: current execution stage (`interpreting`, `executing`, etc.)
  - `code`: task state code (see Error Handling)
  - `progress`: percentage (0–100)
  - `reasoning`: reasoning messages (streaming only)
  - `is_final`: whether this is the last message for this stage

### Fields Reference
| Field | Type | Required | Description |
|--------|------|-----------|--------------|
| `text` | string | ☑ | User query or task description |
| `stream` | boolean | optional | Enable SSE streaming |
| `mode` | string | optional | Default is `run` |
| `task_id` | string | optional | Returned if asynchronous |
| `reasoning` | array | optional | Returned only in streaming mode |
| `is_final` | boolean | ☑ | Marks final output of this stage |

### Parameters & Best Practices
- Use `stream=true` for long-running analysis to observe reasoning updates.  
- Always store the returned `task_id` if task is asynchronous.  
- Handle `"WAITING_USER"` to collect additional user input.  
- Use UTC timestamps (`YYYY-MM-DDTHH:MM:SSZ`).  

### Handling `"WAITING_USER"`

When the response field **`code="WAITING_USER"`** appears, it indicates that the Agent requires **additional user confirmation or missing information** before the task can continue. The task is temporarily **paused**, and the Agent will not resume execution until the client provides the requested input.

#### Mandatory: Include `task_id` When Continuing

To continue a paused task, the client **must** send the follow-up message with the original `task_id`.
If the continuation request does **not** include the correct `task_id`, the system will treat it as a **new task**, leaving the original task unresolved.


## Status Endpoint

### Purpose
Check the current progress or completion state of a previously submitted task.

### Request
```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/tariff_calc/run   
  -H "Authorization: Bearer <YOUR_API_KEY>"   
  -H "Content-Type: application/json"   
  -d '{"mode": "status", "task_id": "<task-id>"}'
```

### Example Response
```json
{
  "success": true,
  "code": "TASK_RUNNING",
  "message": "Task is still running.",
  "data": {
    "task_id": "<task-id>",
    "agent": "tariff_calc",
    "stage": "running",
    "code": "TASK_RUNNING",
    "progress": 65.5,
    "timestamp": "2025-11-12T09:05:00Z",
    "is_final": false
  },
  "metadata": {
    "agent": "tariff_calc",
    "timestamp": "2025-11-12T09:05:00Z"
  },
  "errors": null
}
```

### Response Structure Explained
- This endpoint returns a **single JSON object** (non-streaming).  
- Common codes: `TASK_RUNNING`, `TASK_COMPLETED`, `TASK_FAILED`.

### Fields Reference
| Field | Type | Required | Description |
|--------|------|-----------|--------------|
| `task_id` | string | ☑ | Task identifier |
| `progress` | number | optional | Current completion percentage |
| `stage` | string | ☑ | Current stage (`running`, `completed`) |
| `code` | string | ☑ | State code (e.g., `TASK_RUNNING`) |

### Parameters & Best Practices
- Poll periodically (e.g., every 5–10 seconds) until `TASK_COMPLETED`.  
- If `TASK_FAILED`, review `errors` for details.  
- Avoid excessive polling to prevent rate limiting.  


## Results Endpoint

### Purpose
Retrieve the final output of a completed task.

### Request
```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/tariff_calc/run
  -H "Authorization: Bearer <YOUR_API_KEY>"
  -H "Content-Type: application/json"
  -d '{"mode": "results", "task_id": "<task-id>"}'
```

### Example Response
```json
{
  "success": true,
  "code": "TASK_COMPLETED",
  "message": "Task completed successfully.",
  "data": {
    "task_id": "<task-id>",
    "agent": "tariff_calc",
    "stage": "completed",
    "code": "TASK_COMPLETED",
    "progress": 100,
    "timestamp": "2025-11-12T09:10:00Z",
    "is_final": true,
    "content": {"type": "result", "data": {"calculation_result": "### Summary of Input Information\n- **HTS Code:** 8415.90.80.25 (Air conditioning evaporator coils)\n- **Country of Origin:** Japan (JP) ..."}} 
  },
  "metadata": {
    "agent": "tariff_calc",
    "timestamp": "2025-11-12T09:10:00Z",
    "credits_used": 10
  },
  "errors": null
}
```

### Response Structure Explained
- Finalized data including computed `content` and credit usage.  
- Common codes: `TASK_COMPLETED`, `TASK_FAILED`.

### Fields Reference
| Field | Type | Required | Description |
|--------|------|-----------|--------------|
| `content` | string | ☑ | Final output text or structured data |
| `credits_used` | number | optional | Credits consumed |
| `errors` | object | optional | Error information if failed |

### Parameters & Best Practices
- Always ensure `code` is `TASK_COMPLETED` before using content.  
- Parse Markdown in `content` for structured rendering if applicable.  
- Handle `"TASK_FAILED"` with fallback or retry logic.  


## Try the U.S. Tariff Calculation Agent (Live Chatbot)

Before integrating this agent via API, you can experience it instantly through our interactive tariff analysis chatbot.

This live demo allows you to:
- Describe your product in natural language
- Identify the most relevant HTS codes
- Apply the correct country of origin
- Calculate U.S. base duties and applicable additional tariffs (including Chapter 99)
- See how tariffs impact your landed cost in real time
- Compare alternative classification or sourcing scenarios

👉 [**Launch the U.S. Tariff Calculation Chatbot**](https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=tariff_calc)

To use the chatbot, you’ll first need to:
- Create a SupplyGraph AI account
- Top up your credit balance

This chatbot is powered by the **same U.S. Tariff Calculation Agent and A2A endpoints** described in this documentation.  
Credits used in the chatbot are deducted in the same way as API/A2A usage.

Everything you experience in the chatbot can be fully embedded into your own system through A2A integration below.



## Make Your First A2A Call

Demonstrates the typical three-step workflow:  
1. Start task with `mode=run`  
2. Check progress with `mode=status`  
3. Retrieve output with `mode=results`  


## Integration Options
### Protocols

| Protocol | Description | Docs |
|------|------|------|
| **A2A (Agent-to-Agent)** | Native protocol for autonomous agent workflows and communication | [A2A Protocol](../a2a.md) |
| **MCP (Multi-Channel Protocol)** | Next-generation orchestration protocol for enterprise and multi-system environments | *(Coming Soon)* |

### Developer Interfaces

| Interface | Description | Docs |
|------|------|------|
| **Python SDK (A2A Client)** | Official Python wrapper built on top of the A2A protocol for rapid integration | [supplygraphai_a2a_sdk](https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk) |


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


## Maintainer & License

Maintainer: info@supplygraph.ai  
License: Proprietary / Internal  
© 2025 SupplyGraph AI
