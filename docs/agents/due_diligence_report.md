# Due Diligence Agent

### Pain
Traditional due diligence is slow, fragmented, and costly—analysts often spend more than 10 hours per company gathering unstructured data that quickly becomes outdated.

### Breakthrough
Automated due diligence cuts research time by up to 90%, delivers standardized, comparable intelligence, and continuously tracks critical changes.

### Why Us
Our due diligence is powered by real-time data from 100M+ companies, 8,000+ benchmarks, and 1M+ key products—continuously structured and monitored for faster, deeper, always-current insights.

### A2A Endpoint
`https://agent.supplygraph.ai/api/v1/agents/due_diligence_report/run`

## Example: Complete A2A Run Workflow
Below is a complete, step-by-step example of calling the **Due Diligence Report Agent** through the A2A interface.

### 1. Submit the Initial Request
**Purpose:** Start a new task by providing a company name.
```bash
curl -N -X POST https://agent.supplygraph.ai/api/v1/agents/due_diligence_report/run \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer <YOUR_API_KEY>" \
     -d '{"text": "Tesla, Inc.", "stream": true}'
```

**Example Response:**
```text
data: {"event": "stream", "data": {"task_id": "<system-generated-task-id>", "agent": "due_diligence_report", "stage": "interpreting", "code": "THINKING", "progress": 0, "reasoning": ["Understanding input context..."], "timestamp": "2025-11-07T01:44:53.382946+00:00", "is_final": false}}

data: {"success": false, "code": "WAITING_USER", "message": "waiting user", "data": {"task_id": "<system-generated-task-id>", "agent": "due_diligence_report", "stage": "interpreting", "code": "WAITING_USER", "progress": 0, "reasoning": [], "timestamp": "2025-11-07T01:44:57.409211+00:00", "is_final": true, "content": "Here are the companies we’ve identified.\nPlease review the information below to confirm accuracy.\nCompany Name: Tesla, Inc.\nCountry: United States\nIf everything looks correct, type [Yes] or [Y] to proceed.\nIf this isn’t the intended company, type [No] or [N]."}, "metadata": {"agent": "due_diligence_report", "timestamp": "2025-11-07T01:44:57.409256+00:00"}, "errors": null}

event: end
data: [DONE]
```

### 2. Confirm the Execution
**Purpose:** Respond with `Yes` to confirm and proceed with task execution.
```bash
curl -N -X POST https://agent.supplygraph.ai/api/v1/agents/due_diligence_report/run \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer <YOUR_API_KEY>" \
     -d '{"text": "Yes", "stream": true, "task_id": "<system-generated-task-id>"}'
```

**Example Response:**
```text
data: {"success": true, "code": "TASK_ACCEPTED", "message": "Task accepted and queued for execution.", "data": {"task_id": "<system-generated-task-id>", "agent": "due_diligence_report", "stage": "executing", "code": "TASK_ACCEPTED", "progress": 0, "reasoning": [], "timestamp": "2025-11-07T01:44:57.409211+00:00", "is_final": true, "content": "Task accepted and queued for execution."}, "metadata": {"agent": "due_diligence_report", "timestamp": "2025-11-07T01:44:57.409256+00:00"}, "errors": null}

event: end
data: [DONE]
```

### 3. Check Task Status
**Purpose:** Query the current status of the task using the same `task_id`.
```bash
curl -N -X POST https://agent.supplygraph.ai/api/v1/agents/due_diligence_report/run \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer <YOUR_API_KEY>" \
     -d '{"text": "", "stream": true, "task_id": "<system-generated-task-id>", "mode": "status"}'
```

**Example Response:**
```text
data: {"success": true, "code": "TASK_COMPLETED", "message": "Task completed successfully.", "data": {"task_id": "<system-generated-task-id>", "agent": "due_diligence_report", "stage": "completed", "code": "TASK_COMPLETED", "progress": 100, "reasoning": [], "timestamp": "2025-11-07T02:03:08.404853+00:00", "is_final": true, "content": ""}, "metadata": {"agent": "due_diligence_report", "timestamp": "2025-11-07T02:05:08.404898+00:00"}, "errors": null}

event: end
data: [DONE]
```

### 4. Retrieve the Final Results
**Purpose:** Once the task is complete, request the final report content.
```bash
curl -N -X POST https://agent.supplygraph.ai/api/v1/agents/due_diligence_report/run \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer <YOUR_API_KEY>" \
     -d '{"text": "", "stream": true, "task_id": "<system-generated-task-id>", "mode": "results"}'
```

**Example Response:**
```text
data: {"success": true, "code": "TASK_COMPLETED", "message": "Task completed successfully.", "data": {"task_id": "<system-generated-task-id>", "agent": "due_diligence_report", "stage": "completed", "code": "TASK_COMPLETED", "progress": 100, "reasoning": [], "timestamp": "2025-11-07T02:03:08.404853+00:00", "is_final": true, "content": "Final due diligence report data..."}, "metadata": {"agent": "due_diligence_report", "timestamp": "2025-11-07T02:05:13.204279+00:00"}, "errors": null}

event: end
data: [DONE]
```


### Notes
- Each `task_id` uniquely identifies one execution instance.  
- The same `task_id` is used across confirmation, status check, and result retrieval.  
- Responses can be returned in **two modes**:
  - **Streaming Mode (SSE):**  
    When `"stream": true` is included in the request, responses are delivered as **Server-Sent Events (SSE)**.  
    This mode provides real-time insight into the model’s intermediate reasoning (e.g., interpreting, progress updates, and partial outputs).  
    Each stream ends with:
    ```
    event: end
    data: [DONE]
    ```
  - **Non-Streaming Mode:**  
    When `"stream": false` (or the `stream` parameter is omitted), the API returns a **single consolidated JSON response** after the task completes —  
    without intermediate “thinking” or step-by-step reasoning outputs.  


### Process Summary
| Step | Description |
|------|--------------|
| 1️⃣ | Submit a company name — the system interprets and requests confirmation |
| 2️⃣ | Confirm execution — the task is accepted and queued |
| 3️⃣ | Check task status — view task progress |
| 4️⃣ | Retrieve results — obtain the final due diligence report |


## Related Docs
- [Getting Started](../getting-started.md)  
- [API Reference](../api.md)  
- [A2A Protocol](../a2a.md)
