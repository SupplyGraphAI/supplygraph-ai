# Customs Classification Agent

### Overview  
Automatically maps products to the correct HS/HTS codes, ensuring compliance, accuracy, and tariff optimization — all within seconds.

### Pain  
For customs specialists, getting an item’s HTS code is a critical workflow task that involves manually checking product descriptions against thousands of classification tables.  
This process is time-consuming, mentally exhausting, and prone to human error.  
Moreover, single products may correspond to multiple potential HTS codes, each implying different tariff implications.

### Breakthrough  
Automated classification reduces manual work time from **hours or days to seconds**, while unlocking opportunities for **tariff optimization**.  
By automating classification, organizations can ensure consistency, accuracy, and compliance across product catalogs and trade operations.

### Why Us  
SupplyGraph AI integrates **real-time global tariff databases** with **graph-based reasoning** to deliver precise, auditable classifications with full traceability.  
Each classification is supported by evidence paths that link back to official customs sources, minimizing compliance risk at scale.


## API Overview
This section provides an overview of the A2A API structure and usage.

### Endpoints Summary
| Endpoint | Method | Description |
|-----------|---------|-------------|
| `/api/v1/agents/tariff_classification/manifest` | GET | Retrieve metadata, schema, pricing, and version info. |
| `/api/v1/agents/tariff_classification/run` | POST | Execute or manage tasks via `mode`. |

**Supported modes:**  
- `mode=run` (default) — start a new task (supports streaming)  
- `mode=status` — check task progress (non-streaming)  
- `mode=results` — retrieve task output (non-streaming)


## Manifest

### Purpose
The Manifest provides metadata about the Agent including version, schema definitions, pricing, and available capabilities.

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
  "description": "Classifies products into correct HTS codes from text.",
  "input_schema": { ... },
  "output_schema": { ... },
  "pricing": { "unit": "credits", "per_run": 2 },
  "status": "active"
}
```


## Run Endpoint

### Purpose
Start a new task with this Agent.  
This endpoint supports both streaming (`stream=true`) and non-streaming (`stream=false`) modes.

### Request
```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/tariff_classification/run
  -H "Authorization: Bearer <YOUR_API_KEY>"
  -H "Content-Type: application/json"
  -d '{"text": "Cotton T-shirts for women, 100%cotton, made in Mexico", "stream": true}'
```

### Text Input Requirements

The `text` field contains the detailed natural-language product description used to perform HTS code classification. It should be as specific and accurate as possible; country-of-origin information may be included but is not required.

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
    "agent": "tariff_classification",
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
    "agent": "tariff_classification",
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
    "agent": "tariff_classification",
    "stage": "executing",
    "code": "TASK_ACCEPTED",
    "progress": 0,
    "timestamp": "2025-11-12T09:00:10Z",
    "is_final": true
  },
  "metadata": {
    "agent": "tariff_classification",
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
curl -X POST https://agent.supplygraph.ai/api/v1/agents/tariff_classification/run   
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
    "agent": "tariff_classification",
    "stage": "running",
    "code": "TASK_RUNNING",
    "progress": 65.5,
    "timestamp": "2025-11-12T09:05:00Z",
    "is_final": false
  },
  "metadata": {
    "agent": "tariff_classification",
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
curl -X POST https://agent.supplygraph.ai/api/v1/agents/tariff_classification/run
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
    "agent": "tariff_classification",
    "stage": "completed",
    "code": "TASK_COMPLETED",
    "progress": 100,
    "timestamp": "2025-11-12T09:10:00Z",
    "is_final": true,
    "content": {
      "type": "result",
      "data": {
        "classification_results": [
          {
            "hts_code": "6109.10.00.40",
            "confidence_score": 0.9,
            "reasoning": "The product is a T-shirt made of 100% cotton, specifically for women. HTS 6109.10.00.40 is specifically for T-shirts, singlets, tank tops and similar garments, knitted or crocheted, of cotton, women's or girls', other than singlets and thermal undershirts. This subheading accurately describes women's cotton T-shirts.",
            "description": "T-shirts, singlets, tank tops and similar garments, knitted or crocheted > Of cotton > Women's or girls' > Other > T-shirts > Women's (339)"
          },
		  ...
        ],
        "country_of_origin": "Mexico"
      }
    }
  },
  "metadata": {
    "agent": "tariff_classification",
    "timestamp": "2025-11-12T09:10:00Z",
    "credits_used": 2
  },
  "errors": null
}
```


### Response Structure Explained
- Finalized data including computed `content` and credit usage.  
- Common codes: `TASK_COMPLETED`, `TASK_FAILED`.

### `content` Field Explanation (On Successful Execution)

When the task completes successfully (`success: true` and `code: "TASK_COMPLETED"`), the `content` field contains the structured result data generated by the tariff classification agent. The structure is as follows:

```json
"content": {
  "type": "result",
  "data": {
    "classification_results": [ ... ],
    "country_of_origin": "<country-if-detected>"
  }
}
```

#### Field Details

* **`type`**
  Always `"result"`, indicating this object contains the final output data.

* **`data`**
  Contains the key structured information for the tariff classification task:

  * **`classification_results`**
    An array of candidate HTS codes.
    The agent may return one or multiple possible matches depending on confidence levels.
    Each item in the array includes:

    * `hts_code`: The suggested HTS code
    * `confidence_score`: The model’s confidence for this classification
    * `reasoning`: Explanation of why the product matches this HTS code
    * `description`: The corresponding HTS code description

  * **`country_of_origin`**
    Extracted from the user’s input **if provided**.
    If not present in the input, this field returned as null.

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


## Try the Customs Classification Agent (Live Chatbot)

Before integrating this agent via API, you can experience it instantly through our interactive classification chatbot.

This live demo allows you to:
- Enter a product description in natural language
- Receive the most relevant HS / HTS code candidates
- Understand why each code is suggested
- Explore alternative classifications and their implications
- Reduce ambiguity and manual lookup work

👉 [**Launch the Customs Classification Chatbot**](https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=tariff_classification)

To use the chatbot, you’ll first need to:
- Create a SupplyGraph AI account
- Top up your credit balance

This chatbot is powered by the **same Customs Classification Agent and A2A endpoints** described in this documentation.  
Credits used in the chatbot are deducted in the same way as API/A2A usage.


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
