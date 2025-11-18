# Enterprise Supply Graph Agent

## Overview
automatically generates a multi-tiered, company-centric global supply-chain product dependency map for any enterprise in the world.

## Pain
Companies struggle with limited data to visualize their worldwide supply networks. Without this infrastructure, they are unable to assess single-region concentration risks, identify alternative sources for critical components, or respond to thousands of daily incidents that could trigger disruptions.

## Breakthrough
Visualizes a company-centered global supply product graph that maps dependencies from Tier-1 suppliers to raw materials, linking components to supply, demand, and channels entities. The data graph also lays the infrastructure to quantify regional concentration, evaluate alternatives sourcing, and identify high-impact incidents before they trigger costly disruptions.

## Why Us
Powered by data on 100M+ companies, 1M+ key products, and 8,000+ industry benchmarks, continuously monitored 24/7, SupplyGraph.AI delivers the world’s first intelligent platform that allows any enterprise to visualize, monitor, and analyze its global supply graph in real time — transforming hidden dependencies into actionable resilience.

---

## API Overview
This section provides an overview of the A2A API structure and usage.

### Endpoints Summary
| Endpoint | Method | Description |
|-----------|---------|-------------|
| `/api/v1/agents/sg_visualization/manifest` | GET | Retrieve metadata, schema, pricing, and version info. |
| `/api/v1/agents/sg_visualization/run` | POST | Execute or manage tasks via `mode`. |

**Supported modes:**  
- `mode=run` (default) — start a new task (supports streaming)  
- `mode=status` — check task progress (non-streaming)  
- `mode=results` — retrieve task output (non-streaming)

---

## Manifest

### Purpose
The Manifest provides metadata about the Agent including version, schema definitions, pricing, and available capabilities.

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
  "description": "Performs {{agent_function_description}}",
  "input_schema": { ... },
  "output_schema": { ... },
  "pricing": { "unit": "credits", "per_run": 10 },
  "status": "active"
}
```

---

## Run Endpoint

### Purpose
Start a new task with this Agent.  
This endpoint supports both streaming (`stream=true`) and non-streaming (`stream=false`) modes.

### Request
```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/sg_visualization/run
  -H "Authorization: Bearer <YOUR_API_KEY>"
  -H "Content-Type: application/json"
  -d '{"text": "{{example_text}}", "stream": true}'
```

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
    "agent": "sg_visualization",
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

---

## Status Endpoint

### Purpose
Check the current progress or completion state of a previously submitted task.

### Request
```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/sg_visualization/run   
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
    "agent": "sg_visualization",
    "stage": "running",
    "code": "TASK_RUNNING",
    "progress": 65.5,
    "timestamp": "2025-11-12T09:05:00Z",
    "is_final": false
  },
  "metadata": {
    "agent": "sg_visualization",
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

---

## Results Endpoint

### Purpose
Retrieve the final output of a completed task.

### Request
```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/sg_visualization/run
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
    "agent": "sg_visualization",
    "stage": "completed",
    "code": "TASK_COMPLETED",
    "progress": 100,
    "timestamp": "2025-11-12T09:10:00Z",
    "is_final": true,
    "content": "## Example final output text"
  },
  "metadata": {
    "agent": "sg_visualization",
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

---

## Make Your First A2A Call

Demonstrates the typical three-step workflow:  
1. Start task with `mode=run`  
2. Check progress with `mode=status`  
3. Retrieve output with `mode=results`  

---

## Integration Options

| Mode | Description | Documentation |
|------|--------------|----------------|
| A2A (Agent-to-Agent) | Autonomous inter-agent workflow mode. | A2A Protocol |
| MCP (Multi-Channel Protocol) | Large-scale enterprise orchestration (coming soon). | Coming Soon |

---

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

---

## Maintainer & License

Maintainer: {{maintainer_email}}  
License: Proprietary / Internal  
© 2025 SupplyGraph AI
