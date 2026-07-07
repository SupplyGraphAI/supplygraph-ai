# Agent-to-Agent (A2A) Protocol

The **SupplyGraph AI A2A Protocol** exposes SupplyGraph agents through the **[Agent2Agent (A2A) Protocol v1.0](https://a2a-protocol.org)** — a standardized, public-facing interface for discovery, messaging, streaming, and task management.

**Public gateway (production):**

```
https://agent.supplygraph.ai/a2a
```

Each agent — such as **Tariff Calculation** (`tariff_calc`), **Customs Classification** (`tariff_classification`), **Due Diligence** (`due_diligence_report`), or **Supply Chain Risk Sentinel** (`risk_propagation_summary`) — is published as an independent A2A Agent with its own **Agent Card** and **Agent Base URL**.

> **Architecture note**  
> 31190 is the **A2A-compliant gateway**. It adapts A2A `message/send`, `message/stream`, and `tasks/get` to SupplyGraph's internal agent runtime.  
> The legacy HTTP interface (`/api/v1/agents/{agent_id}/manifest`, `/run` with `mode=run|status|results`) remains the **internal backend contract** and is **not** the public A2A surface.

Broader resource discovery (agents + MCP + docs) is also published via ARD at:

```
https://supplygraph.ai/.well-known/ai-catalog.json
```

---

## 1. Overview

Through the A2A gateway, developers can:

- **Discover** agents via Well-Known URI, Registry, and per-agent **Agent Cards**
- **Send messages** with `POST .../message:send` (sync or async Task)
- **Stream** real-time updates with `POST .../message:stream` (SSE)
- **Poll tasks** with `GET .../tasks/{task_id}` (`tasks/get`)
- **Retrieve extended metadata** (when enabled) via `GET .../extendedAgentCard`

### Agent Base URL

Every business agent has an **Agent Base URL** (also the `url` field in its Agent Card):

```
{agent_base} = https://agent.supplygraph.ai/a2a/agents/{agent_id}
```

All execution and task endpoints are resolved **relative to `{agent_base}`**, per A2A HTTP/REST Binding.

### Discovery flow

**Full discovery (recommended for generic A2A SDKs):**

```
GET /.well-known/agent-card.json
  → metadata.registryUrl
  → GET /a2a/agents
  → GET /a2a/agents/{agent_id}          (Agent Card)
  → use Card.url as {agent_base}
  → POST {agent_base}/message:send | message:stream
```

**Direct access (when `agent_id` is known):**

```
GET /a2a/agents/{agent_id}  →  execute against Card.url
```

> The platform Well-Known Card (`/.well-known/agent-card.json`) is **discovery-only**.  
> Do **not** send `message/send` to the platform root URL.

> All timestamps use UTC ISO 8601 format, e.g. `2026-06-29T12:00:00Z`.

---

## 2. Transport & Authentication

| Property | Description |
|----------|-------------|
| **Protocol version** | A2A `1.0` |
| **Version header** | `A2A-Version: 1.0` (recommended; defaults to `1.0` if omitted) |
| **Transport** | HTTPS |
| **REST Content-Type** | `application/a2a+json` (non-SSE) |
| **Streaming Content-Type** | `text/event-stream` (SSE) |
| **JSON field naming** | camelCase |
| **Authentication** | `Authorization: Bearer {api_key}` |
| **Encoding** | UTF-8 JSON |

Obtain API keys from the SupplyGraph Console:

```
https://supplygraph.ai/zk_chat_os/dashboard/dashboard.html
```

> Unauthorized requests return HTTP `401` with an A2A error envelope (see §6).

---

## 3. Endpoints Summary

### 3.1 Discovery & Registry

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/.well-known/agent-card.json` | GET | Platform entry Agent Card (A2A v1.0, RFC 8615) |
| `/.well-known/agent.json` | GET | Legacy alias (A2A v0.2.x compatibility) |
| `/.well-known/jwks.json` | GET | JWKS for webhook JWT verification |
| `/a2a/agents` | GET | Agent Registry list (platform extension) |
| `/a2a/agents/{agent_id}` | GET | Full Agent Card for a business agent |
| `/a2a/agents/{agent_id}/extendedAgentCard` | GET | Authenticated extended Agent Card |

### 3.2 Execution & Tasks (relative to `{agent_base}`)

| Endpoint | Method | A2A method | Streaming | Purpose |
|----------|--------|------------|-----------|---------|
| `{agent_base}/message:send` | POST | `message/send` | No | Send a user message; returns a **Task** |
| `{agent_base}/message:stream` | POST | `message/stream` | Yes (SSE) | Send a message and subscribe to live events |
| `{agent_base}/tasks/{task_id}` | GET | `tasks/get` | No | Get task status, history, and artifacts |

**Example `{agent_base}`:**

```
https://agent.supplygraph.ai/a2a/agents/tariff_calc
```

### 3.3 Health

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/health` | GET | Liveness check |

> **Current implementation scope (31190 v1.0)**  
> The gateway currently exposes REST bindings for `message/send`, `message/stream`, and `tasks/get`.  
> Additional A2A methods (`tasks/cancel`, push notification configs, JSON-RPC unified entry) are defined in the A2A spec and may be added in future releases.

---

## 4. Agent Card

Each agent publishes a standard **Agent Card** (camelCase JSON):

```
GET https://agent.supplygraph.ai/a2a/agents/tariff_calc
```

Key fields:

| Field | Description |
|-------|-------------|
| `name` | Human-readable agent name |
| `description` | Capability summary |
| `url` | **Must equal `{agent_base}`** |
| `version` | Agent version |
| `protocolVersion` | `"1.0"` |
| `capabilities` | e.g. `streaming`, `extendedAgentCard`, `pushNotifications` |
| `skills` | Skill definitions with tags and examples |
| `supportedInterfaces` | Protocol bindings (`HTTP+JSON`, `JSONRPC`) |

---

## 5. Message & Task Model

A2A uses a **Message → Task** lifecycle, not a single `/run` endpoint with `mode` switching.

### 5.1 Send message (`message:send`)

**Request:**

```http
POST https://agent.supplygraph.ai/a2a/agents/tariff_calc/message:send
A2A-Version: 1.0
Content-Type: application/a2a+json
Authorization: Bearer {api_key}
```

```json
{
  "message": {
    "messageId": "msg-uuid",
    "role": "user",
    "parts": [
      { "kind": "text", "text": "Calculate U.S. import duty for HS 8471 from China" }
    ],
    "contextId": "ctx-uuid",
    "taskId": "task-uuid"
  },
  "configuration": {
    "acceptedOutputModes": ["text/plain", "application/json"],
    "historyLength": 10,
    "blocking": false
  },
  "metadata": {}
}
```

| Field | Required | Description |
|-------|----------|-------------|
| `message.messageId` | Yes | Unique message ID |
| `message.role` | Yes | `"user"` |
| `message.parts` | Yes | Text / file / data parts |
| `message.contextId` | No | Session ID for multi-turn dialogue |
| `message.taskId` | No | Existing task ID to continue a conversation |
| `configuration.blocking` | No | `true` = block until complete; default `false` |

**Response (Task):**

```json
{
  "id": "task-uuid",
  "contextId": "ctx-uuid",
  "status": {
    "state": "working",
    "timestamp": "2026-06-29T12:00:00Z"
  },
  "artifacts": [],
  "history": []
}
```

**Task state values:**

`submitted` | `working` | `input-required` | `completed` | `failed` | `canceled` | `rejected`

- **`input-required`**: agent needs another user message → send again with the same `contextId` / `taskId`
- **`completed`**: final output is in `artifacts`

### 5.2 Stream message (`message:stream`)

Same request body as `message:send`.

**Response:** `Content-Type: text/event-stream`

Each SSE event:

```
event: message
data: {"jsonrpc":"2.0","result":{"kind":"task","task":{...}},"id":"1"}
```

`result.kind` may be:

| kind | Description |
|------|-------------|
| `task` | Initial or updated Task snapshot |
| `status-update` | Task state change |
| `artifact-update` | Partial or final output chunk |
| `message` | Agent message in the conversation |

Requires `capabilities.streaming = true` on the Agent Card.

### 5.3 Get task (`tasks/get`)

```http
GET https://agent.supplygraph.ai/a2a/agents/tariff_calc/tasks/{task_id}?historyLength=10
A2A-Version: 1.0
Authorization: Bearer {api_key}
```

Returns the current Task, including `artifacts` when the task reaches a terminal state.

> **Mapping from legacy API**  
> Internally, 31190 translates A2A `tasks/get` to backend `mode=status` and, when complete, `mode=results`.  
> External clients should **only** use A2A endpoints — not the legacy `/run` modes.

---

## 6. Error Handling

A2A errors use a standard envelope:

```json
{
  "error": {
    "code": -32004,
    "message": "Unauthorized",
    "data": {
      "@type": "type.googleapis.com/google.rpc.ErrorInfo",
      "reason": "UNAUTHORIZED"
    }
  }
}
```

| JSON-RPC Code | HTTP | reason (typical) | Description |
|---------------|------|------------------|-------------|
| `-32001` | 404 | `TASK_NOT_FOUND` | Task does not exist |
| `-32004` | 400/401/404 | `INVALID_REQUEST`, `UNAUTHORIZED`, `AGENT_NOT_FOUND` | Bad input, auth failure, unknown agent |
| `-32006` | 502 | `INVALID_AGENT_RESPONSE` | Upstream agent error |

Legacy codes such as `TASK_ACCEPTED`, `THINKING`, `INSUFFICIENT_CREDITS` belong to the **internal `/run` envelope** and are **not** the public A2A error format. The gateway maps backend responses into A2A **Task states** and **SSE events**.

---

## 7. Streaming Events (SSE)

When using `message:stream`, events follow **A2A JSON-RPC-over-SSE**, not the legacy `{ "event": "stream", "code": "THINKING" }` format.

**Example — status update:**

```json
{
  "jsonrpc": "2.0",
  "result": {
    "kind": "status-update",
    "status": {
      "state": "working",
      "timestamp": "2026-06-29T12:00:05Z"
    }
  },
  "id": "1"
}
```

**Example — artifact update:**

```json
{
  "jsonrpc": "2.0",
  "result": {
    "kind": "artifact-update",
    "artifact": {
      "parts": [{ "kind": "text", "text": "Analyzing tariff schedule..." }]
    }
  },
  "id": "1"
}
```

Poll or stream until `status.state` is terminal (`completed`, `failed`, `canceled`, `rejected`), or handle `input-required` for multi-turn flows.

---

## 8. Security & Access Control

- Authenticate every execution/task request with `Authorization: Bearer {api_key}`
- Agent Cards declare whether authentication is required (`extendedAgentCard` when sensitive metadata is present)
- Use TLS 1.2+ for all HTTPS traffic
- Webhook push notifications (when enabled) use platform JWT; verify with:

  ```
  GET https://agent.supplygraph.ai/.well-known/jwks.json
  ```

- Log and trace cross-agent workflows with `contextId` and `taskId`

---

## 9. Versioning

| Layer | Version | Notes |
|-------|---------|-------|
| **A2A protocol** | `1.0` | Public gateway contract |
| **Agent Card** | per-agent `version` field | Independent agent releases |
| **Legacy ZK API** | `/api/v1/agents/...` | Internal; not for external A2A clients |

Clients SHOULD send:

```http
A2A-Version: 1.0
```

Backward-compatible changes stay within A2A 1.0. Breaking changes will bump the major A2A version and be reflected in Agent Cards and release notes.

---

## 10. Legacy vs Standard API (Migration Guide)

| Concern | Legacy (internal) | Standard A2A (31190 public) |
|---------|-------------------|----------------------------|
| Discovery | `GET .../manifest` | `GET /.well-known/agent-card.json` → `/a2a/agents` → `/a2a/agents/{id}` |
| Start task | `POST .../run` `mode=run` | `POST {agent_base}/message:send` or `message:stream` |
| Task status | `POST .../run` `mode=status` | `GET {agent_base}/tasks/{task_id}` |
| Results | `POST .../run` `mode=results` | Included in Task `artifacts` via `tasks/get` |
| Request shape | `{ mode, text, stream, task_id }` | `{ message, configuration, metadata }` |
| Response shape | `{ success, code, data }` | A2A **Task** object or **error** envelope |
| Streaming | Custom SSE (`event: stream/end`) | A2A SSE (`result.kind`: task / status-update / artifact-update) |

Existing integrations on the legacy API can continue internally; **new public integrations should use the A2A gateway**.

---

## 11. Related Documentation

📘 **Getting Started**  
https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/getting-started.md

🤖 **Agent Library**  
https://github.com/SupplyGraphAI/supplygraph-ai/tree/main/docs/agents

🌐 **ARD Resource Catalog**  
https://supplygraph.ai/.well-known/ai-catalog.json

📦 **Python A2A SDK**  
https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk

🧠 **31190 internal reference**  
Project `server31190.md` (full endpoint matrix)

---

## 12. SupplyGraph AI A2A SDK (Python)

➡️ **[SupplyGraph AI A2A Python SDK](https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk)**

The SDK targets the **standard A2A gateway** (`agent.supplygraph.ai`), including:

- Agent discovery (Well-Known + Registry + Agent Card)
- `message/send` and `message/stream`
- Task polling (`tasks/get`)
- Adapters for LangChain, LangGraph, AutoGen, CrewAI, Google A2A, MCP, and more

---

## 13. Why A2A Matters

Traditional APIs expose static functions.  
The SupplyGraph AI A2A Protocol exposes **interoperable agents** — discoverable via standard Agent Cards, invokable by any A2A-compliant client, and composable in multi-agent orchestration stacks.

This enables:

- **Standards-based** agent discovery and invocation (A2A 1.0 + ARD)
- **Cross-platform** agent orchestration
- Multi-step, multi-turn autonomous workflows
- Native integration with the broader agentic ecosystem (Google A2A, MCP, ARD registries)
- A clear separation between **public protocol** (31190) and **internal runtime** (ZK agents)

A2A is the public contract; SupplyGraph agents are the intelligence behind it.
