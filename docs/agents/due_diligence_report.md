# Due Diligence Agent

### Pain
Traditional due diligence is slow, fragmented, and costly—analysts often spend more than 10 hours per company gathering unstructured data that quickly becomes outdated.

### Breakthrough
Automated due diligence cuts research time by up to 90%, delivers standardized, comparable intelligence, and continuously tracks critical changes.

### Why Us
Our due diligence is powered by real-time data from 100M+ companies, 8,000+ benchmarks, and 1M+ key products—continuously structured and monitored for faster, deeper, always-current insights.

---

## API Overview
This section provides an overview of the A2A API structure and usage.

### Endpoints Summary
| Endpoint | Method | Description |
|-----------|---------|-------------|
| `/api/v1/agents/due_diligence_report/manifest` | GET | Retrieve metadata, schema, pricing, and version info. |
| `/api/v1/agents/due_diligence_report/run` | POST | Execute or manage tasks via `mode`. |

**Supported modes:**  
- `mode=run` (default) — start a new task (non-streaming)  
- `mode=status` — check task progress (non-streaming)  
- `mode=results` — retrieve task output (non-streaming)

---

## Manifest

### Purpose
The Manifest provides metadata about the Agent including version, schema definitions, pricing, and available capabilities.

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
  "description": "Performs {{agent_function_description}}",
  "input_schema": { ... },
  "output_schema": { ... },
  "pricing": { "unit": "credits", "per_run": 10 },
  "status": "active"
}
```

---

## Run Endpoint

### 1. Submit the Initial Request
#### Purpose
Start a new task with this Agent.  
This endpoint supports non-streaming (`stream=false`) modes and allows the use of the `chapter_name` parameter to control the generation of a specific individual chapter in the report.

#### Request
```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/due_diligence_report/run
  -H "Authorization: Bearer <YOUR_API_KEY>"
  -H "Content-Type: application/json"
  -d '{"text": "Tesla, Inc. United States", "stream": false}'
```
**Example Response 1:**
```text
{"success": false, "code": "WAITING_USER", "message": "waiting user", "data": {"task_id": "<system-generated-task-id>", "agent": "due_diligence_report", "stage": "interpreting", "code": "WAITING_USER", "progress": 0, "reasoning": [], "timestamp": "2025-11-14T02:46:44.242486Z", "is_final": true, "content": "Your input must include a valid company name (ideally the full legal name, e.g. Apple Inc.) so that we can provide more accurate and precise service for you."}, "metadata": {"agent": "due_diligence_report", "timestamp": "2025-11-14T02:46:44.242541Z"}, "errors": null}
```

**Example Response 2:**
```text
{"success": false, "code": "WAITING_USER", "message": "waiting user", "data": {"task_id": "<system-generated-task-id>", "agent": "due_diligence_report", "stage": "interpreting", "code": "WAITING_USER", "progress": 0, "reasoning": [], "timestamp": "2025-11-07T01:44:57.409211+00:00", "is_final": true, "content": "Here are the companies we’ve identified.\nPlease review the information below to confirm accuracy.\nCompany Name: Tesla, Inc.\nCountry: United States\nIf everything looks correct, type [Yes] or [Y] to proceed.\nIf this isn’t the intended company, type [No] or [N]."}, "metadata": {"agent": "due_diligence_report", "timestamp": "2025-11-07T01:44:57.409256+00:00"}, "errors": null}
```

### 2. Confirm the Execution
#### Purpose
Respond with `Yes` to confirm and proceed with task execution, ensuring the `chapter_name` parameter is kept consistent with the initial task creation.

#### Request
```bash
curl -N -X POST https://agent.supplygraph.ai/api/v1/agents/due_diligence_report/run \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer <YOUR_API_KEY>" \
     -d '{"text": "Yes", "stream": false, "task_id": "<system-generated-task-id>"}'
```

**Example Response:**
```text
data: {"success": true, "code": "TASK_ACCEPTED", "message": "Task accepted and queued for execution.", "data": {"task_id": "<system-generated-task-id>", "agent": "due_diligence_report", "stage": "executing", "code": "TASK_ACCEPTED", "progress": 0, "reasoning": [], "timestamp": "2025-11-07T01:44:57.409211+00:00", "is_final": true, "content": "Task accepted and queued for execution."}, "metadata": {"agent": "due_diligence_report", "timestamp": "2025-11-07T01:44:57.409256+00:00"}, "errors": null}
```

### chapter_name Parameter

The `chapter_name` parameter specifies the chapter of the report to generate. It can be set to "ALL" to include all chapters or one of the specific chapter names listed below.

**Schema Definition:**

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

### Purpose
Check the current progress or completion state of a previously submitted task.

### Request
```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/due_diligence_report/run   
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
    "agent": "due_diligence_report",
    "stage": "running",
    "code": "TASK_RUNNING",
    "progress": 65.5,
    "timestamp": "2025-11-12T09:05:00Z",
    "is_final": false
  },
  "metadata": {
    "agent": "due_diligence_report",
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
curl -X POST https://agent.supplygraph.ai/api/v1/agents/due_diligence_report/run
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
    "agent": "due_diligence_report",
    "stage": "completed",
    "code": "TASK_COMPLETED",
    "progress": 100,
    "timestamp": "2025-11-12T09:10:00Z",
    "is_final": true,
    "content": "## Example final output text"
  },
  "metadata": {
    "agent": "due_diligence_report",
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

Demonstrates the typical workflow:  
1. Submit initial request with `mode=run`  
2. Confirm execution (if prompted) with `mode=run`  
3. Check progress with `mode=status`  
4. Retrieve output with `mode=results`  

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
