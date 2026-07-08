# Agent API

The **SupplyGraph Agent API** is a traditional REST interface for developers who prefer a familiar `run → status → results` workflow over standard [A2A](../a2a_mcp/a2a.md) or [MCP](../a2a_mcp/mcp.md) integration.

> For new projects, we recommend **A2A** or **MCP**. Agent API remains fully supported.

**Base URL:**

```
https://agent.supplygraph.ai/api/v1/agents/{agent_id}
```

Obtain an API key from the [SupplyGraph Console](https://supplygraph.ai/zk_chat_os/dashboard/dashboard.html). See [Getting Started](../getting-started.md) for account setup and Sandbox keys.

---

## 1. Overview

Each agent exposes two endpoints relative to `{agent_id}`:

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `{base}/manifest` | GET | Agent metadata, input/output schema, pricing |
| `{base}/run` | POST | Start tasks and manage lifecycle via `mode` |

**Task lifecycle** (single endpoint, `mode` field):

```
run → status → results
```

| `mode` | Purpose |
|--------|---------|
| `run` (default) | Start a new task |
| `status` | Poll task progress |
| `results` | Retrieve completed output |

Streaming reasoning events are available when `"stream": true`.

Per-agent input requirements (fields, examples) are documented under [docs/agents/](../agents/).

---

## 2. Authentication

All requests require:

```http
Authorization: Bearer {api_key}
Content-Type: application/json
```

Unauthorized requests return `code: UNAUTHORIZED`.

---

## 3. Manifest

Retrieve agent metadata before integration:

```bash
curl -X GET "https://agent.supplygraph.ai/api/v1/agents/tariff_calc/manifest" \
  -H "Authorization: Bearer <YOUR_API_KEY>"
```

**Example response (abbreviated):**

```json
{
  "agent_id": "tariff_calc",
  "name": "U.S. Tariff Calculation Agent",
  "version": "1.0.0",
  "description": "Calculates U.S. customs duties...",
  "input_schema": { },
  "output_schema": { },
  "pricing": { "unit": "credits", "per_run": 10 },
  "status": "active"
}
```

Use `input_schema` / `output_schema` to validate client payloads.

---

## 4. Run — Start a Task

**Request:**

```bash
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/tariff_classification/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"text": "Cotton T-shirts for women, 100% cotton, made in Mexico", "stream": false}'
```

| Field | Required | Description |
|-------|----------|-------------|
| `text` | Yes* | Natural-language input (*required for most agents) |
| `stream` | No | `true` enables SSE reasoning stream; default `false` |
| `task_id` | No | Include to continue an existing task (multi-turn) |
| `mode` | No | Omit or `"run"` for new tasks |

**Non-streaming response envelope:**

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
    "progress": 0
  },
  "metadata": { "agent": "tariff_classification" },
  "errors": null
}
```

Save `data.task_id` for `status` and `results` calls.

---

## 5. Status — Poll Progress

```bash
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/tariff_classification/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"mode": "status", "task_id": "<task-id>", "stream": false}'
```

**Example response:**

```json
{
  "success": true,
  "code": "TASK_RUNNING",
  "data": {
    "task_id": "<task-id>",
    "agent": "tariff_classification",
    "stage": "running",
    "progress": 65.5
  }
}
```

Poll until `code` is terminal (`TASK_COMPLETED`, `TASK_FAILED`, `TASK_CANCELLED`).

---

## 6. Results — Retrieve Output

```bash
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/tariff_classification/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"mode": "results", "task_id": "<task-id>", "stream": false}'
```

**Example response:**

```json
{
  "success": true,
  "code": "TASK_COMPLETED",
  "data": {
    "task_id": "<task-id>",
    "agent": "tariff_classification",
    "stage": "completed",
    "progress": 100,
    "content": "## Best match:\nHTS code: 6109.10.00.40\n..."
  },
  "metadata": {
    "credits_used": 10
  }
}
```

Result shape varies by agent; see each agent's documentation under [docs/agents/](../agents/index.md).

---

## 7. Streaming (SSE)

Set `"stream": true` on `mode=run` to receive Server-Sent Events during interpretation and execution.

**Event types:**

| `data.code` | Stage | Meaning |
|-------------|-------|---------|
| `THINKING` | interpreting | Agent reasoning in progress |
| `TASK_ACCEPTED` | executing | Task queued |
| `TASK_RUNNING` | executing | Task in progress |
| `TASK_COMPLETED` | completed | Ready for `results` |
| `WAITING_USER` | interpreting | Agent needs follow-up input |

**Stream frame example:**

```
data: {"event": "stream", "data": {"task_id": "<task-id>", "code": "THINKING", "reasoning": ["..."], "is_final": false}}

data: {"success": true, "code": "TASK_ACCEPTED", "data": {"task_id": "<task-id>", "is_final": true}, ...}

event: end
data: [DONE]
```

Use `curl -N` to disable buffering when testing SSE from the command line.

---

## 8. Multi-Turn (`WAITING_USER`)

When a response includes `code: "WAITING_USER"`, the agent needs additional input.

To continue:

1. Send a new `run` request with the **same `task_id`**
2. Include the follow-up text in `text`
3. Resume `status` → `results`

Without the correct `task_id`, the request starts a **new** task and the original task remains paused.

---

## 9. Response Envelope

All Agent API responses share a common structure:

```json
{
  "success": true,
  "code": "TASK_COMPLETED",
  "message": "Human-readable summary",
  "data": { },
  "metadata": { "credits_used": 10 },
  "errors": null
}
```

| Field | Description |
|-------|-------------|
| `success` | Business-level success flag |
| `code` | Primary state code (see §10) |
| `message` | Safe to display to end users |
| `data.task_id` | Task identifier |
| `data.content` | Result payload (in `results` or terminal responses) |
| `metadata.credits_used` | Credits consumed (when applicable) |

---

## 10. Error & Status Codes

### Common errors

| Code | Description |
|------|-------------|
| `UNAUTHORIZED` | API key missing or expired |
| `INSUFFICIENT_CREDITS` | Not enough credits |
| `RATE_LIMITED` | Too many requests |
| `INVALID_REQUEST` | Input outside agent scope |

### Lifecycle codes

```
interpreting:  INTERPRETING, WAITING_USER, INVALID_REQUEST
executing:     TASK_ACCEPTED, TASK_RUNNING
completed:     TASK_COMPLETED, TASK_FAILED
cancelled:     TASK_CANCELLED
```

---

## 11. Rate Limits

Default limits per API key:

| Limit | Default |
|-------|---------|
| Requests per minute | 60 |
| Maximum concurrent tasks | 5 |

Credits are deducted per agent invocation. See each agent's `manifest` for `pricing.per_run`.

High-volume users: **enterprise@supplygraph.ai**

---

## 12. Optional SDK

**[supplygraphai_a2a_sdk](https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk)** wraps Agent API with `AgentClient.run()` / `.status()` / `.results()`.

For standard A2A or MCP, use **[a2a-sdk](https://pypi.org/project/a2a-sdk/)** or **[mcp](https://pypi.org/project/mcp/)** instead — see [Quick Example](./quick_example.md) and [a2a_mcp/quick_example.md](../a2a_mcp/quick_example.md).

---

## 13. Related Documentation

- [Quick Example (Agent API)](./quick_example.md)
- [Getting Started](../getting-started.md) — API keys & Sandbox
- [Integration Guide](../a2a_mcp/integration.md) — A2A vs MCP vs Agent API
- [Agent Library](../agents/index.md) — per-agent input requirements
- [A2A Protocol](../a2a_mcp/a2a.md)
- [MCP Protocol](../a2a_mcp/mcp.md)
