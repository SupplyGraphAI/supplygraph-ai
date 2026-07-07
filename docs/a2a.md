# Agent-to-Agent (A2A) Protocol

The **SupplyGraph AI A2A Protocol** defines how external developers and systems discover, authenticate, and interact with SupplyGraph AI Agents using the **[Agent2Agent (A2A) Protocol v1.0](https://a2a-protocol.org)**.

**Service base URL (production):**

```
https://agent.supplygraph.ai/a2a
```

Each agent — such as **Tariff Calculation** (`tariff_calc`), **Customs Classification** (`tariff_classification`), **Due Diligence** (`due_diligence_report`), or **Supply Chain Risk Sentinel** (`risk_propagation_summary`) — is an independent A2A Agent with its own **Agent Card** and **Agent Base URL**.

SupplyGraph also publishes a broader **Agentic Resource Discovery (ARD)** catalog at:

```
https://supplygraph.ai/.well-known/ai-catalog.json
```

> All timestamps use UTC ISO 8601 format, e.g. `2026-06-29T12:00:00Z`.

---

## 1. Overview

Through this protocol, developers can:

- **Discover** agents via Well-Known URI, Registry, and per-agent **Agent Cards**
- **Send messages** with `POST .../message:send`
- **Stream** real-time updates with `POST .../message:stream` (SSE)
- **Query tasks** with `GET .../tasks/{task_id}` (`tasks/get`)
- **Receive completion webhooks** by registering `pushNotificationConfig` on the first message request (see §6)
- **Retrieve extended metadata** (when enabled) via `GET .../extendedAgentCard`

### Agent Base URL

Every business agent has an **Agent Base URL** — the `url` field in its Agent Card:

```
{agent_base} = https://agent.supplygraph.ai/a2a/agents/{agent_id}
```

All execution and task endpoints are resolved **relative to `{agent_base}`**, per the A2A HTTP/REST Binding.

### Discovery flow

**Recommended (generic A2A clients):**

```
GET /.well-known/agent-card.json
  → metadata.registryUrl
  → GET /a2a/agents
  → GET /a2a/agents/{agent_id}     (Agent Card)
  → use Card.url as {agent_base}
  → POST {agent_base}/message:send | message:stream
```

**Direct access (when `agent_id` is known):**

```
GET /a2a/agents/{agent_id}  →  execute against Card.url
```

> The platform Well-Known Card (`/.well-known/agent-card.json`) is **discovery-only**.  
> Do **not** send execution requests to the platform root URL.

---

## 2. Transport & Authentication

| Property | Description |
|----------|-------------|
| **Protocol version** | A2A `1.0` |
| **Version header** | `A2A-Version: 1.0` (recommended; defaults to `1.0` if omitted) |
| **Transport** | HTTPS |
| **REST Content-Type** | `application/a2a+json` (non-SSE) |
| **Streaming Content-Type** | `text/event-stream` (SSE) |
| **JSON-RPC Content-Type** | `application/json` |
| **JSON field naming** | camelCase |
| **Authentication** | `Authorization: Bearer {api_key}` |
| **Encoding** | UTF-8 JSON |

Obtain API keys from the SupplyGraph Console:

```
https://supplygraph.ai/zk_chat_os/dashboard/dashboard.html
```

Unauthorized or malformed requests return an A2A error envelope with HTTP `401` or `400` (see §7).

---

## 3. Endpoints Summary

### 3.1 Discovery & Registry

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/.well-known/agent-card.json` | GET | Platform entry Agent Card (RFC 8615) |
| `/.well-known/agent.json` | GET | Legacy Well-Known alias (A2A v0.2.x clients) |
| `/.well-known/jwks.json` | GET | JWKS for webhook JWT verification |
| `/a2a/agents` | GET | Agent Registry list |
| `/a2a/agents/{agent_id}` | GET | Agent Card for a business agent |
| `/a2a/agents/{agent_id}/extendedAgentCard` | GET | Authenticated extended Agent Card |

### 3.2 Execution & Tasks (relative to `{agent_base}`)

| Endpoint | Method | A2A method | Streaming | Purpose |
|----------|--------|------------|-----------|---------|
| `{agent_base}/message:send` | POST | `message/send` | No | Send a user message; returns a **Task** |
| `{agent_base}/message:stream` | POST | `message/stream` | Yes (SSE) | Send a message and subscribe to live events |
| `{agent_base}/tasks/{task_id}` | GET | `tasks/get` | No | Get task status, history, and artifacts |
| `{agent_base}` | POST | JSON-RPC | Varies | Unified JSON-RPC entry (A2A primary binding) |

**Example `{agent_base}`:**

```
https://agent.supplygraph.ai/a2a/agents/tariff_calc
```

### 3.3 Health

| Endpoint | Method | Purpose |
|----------|--------|---------|
| `/health` | GET | Service liveness check |

---

## 4. Agent Card

Each agent publishes a standard **Agent Card**:

```http
GET https://agent.supplygraph.ai/a2a/agents/tariff_calc
```

**Example response (abbreviated):**

```json
{
  "name": "U.S. Tariff Calculation",
  "description": "Calculates U.S. tariff rates and duty impacts.",
  "url": "https://agent.supplygraph.ai/a2a/agents/tariff_calc",
  "version": "1.0.0",
  "protocolVersion": "1.0",
  "capabilities": {
    "streaming": true,
    "extendedAgentCard": true,
    "pushNotifications": true
  },
  "defaultInputModes": ["text/plain"],
  "defaultOutputModes": ["text/plain", "application/json"],
  "skills": [
    {
      "id": "tariff_calc",
      "name": "U.S. Tariff Calculation",
      "description": "Calculate import duty for a product description or HS code.",
      "tags": ["tariff", "trade"],
      "examples": ["Calculate U.S. import duty for HS 8471 from China"]
    }
  ],
  "supportedInterfaces": [
    {
      "protocolBinding": "HTTP+JSON",
      "url": "https://agent.supplygraph.ai/a2a/agents/tariff_calc",
      "protocolVersion": "1.0"
    },
    {
      "protocolBinding": "JSONRPC",
      "url": "https://agent.supplygraph.ai/a2a/agents/tariff_calc",
      "protocolVersion": "1.0"
    }
  ]
}
```

| Field | Description |
|-------|-------------|
| `name` | Human-readable agent name |
| `description` | Capability summary |
| `url` | Agent Base URL; **must equal `{agent_base}`** |
| `version` | Agent version |
| `protocolVersion` | A2A protocol version (`"1.0"`) |
| `capabilities` | Supported features (`streaming`, `extendedAgentCard`, `pushNotifications`) |
| `skills` | Skill definitions with tags and examples |
| `supportedInterfaces` | Protocol bindings and their URLs |

### Platform entry Agent Card

```http
GET https://agent.supplygraph.ai/.well-known/agent-card.json
```

Returns a **discovery-only** platform card. Key metadata fields:

| Field | Description |
|-------|-------------|
| `metadata.registryUrl` | Registry list URL |
| `metadata.agentCardPattern` | URL template for per-agent cards |
| `metadata.discoveryOnly` | `true` — this card does not accept execution |

---

## 5. Message & Task Model

A2A interaction follows a **Message → Task** lifecycle.

### 5.1 Send message (`message/send`)

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
    "blocking": false,
    "pushNotificationConfig": {
      "id": "webhook-config-uuid",
      "url": "https://client.example.com/webhook/a2a",
      "token": "optional-client-verification-token"
    }
  },
  "metadata": {}
}
```

| Field | Required | Description |
|-------|----------|-------------|
| `message.messageId` | Yes | Unique message ID |
| `message.role` | Yes | `"user"` |
| `message.parts` | Yes | Message parts (`text`, `file`, or `data`) |
| `message.contextId` | No | Session ID for multi-turn dialogue |
| `message.taskId` | No | Existing task ID to continue a conversation |
| `configuration.blocking` | No | `true` blocks until complete; default `false` |
| `configuration.pushNotificationConfig` | No | Webhook to notify when the task reaches a terminal state (see §6). **Only accepted on the first request** for a new task (when `message.taskId` is omitted) |

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

| State | Meaning |
|-------|---------|
| `submitted` | Task accepted, not yet started |
| `working` | Task in progress |
| `input-required` | Agent needs another user message; continue with same `contextId` / `taskId` |
| `completed` | Task finished; output in `artifacts` |
| `failed` | Task failed |
| `canceled` | Task canceled |
| `rejected` | Task rejected |

### 5.2 Stream message (`message/stream`)

Same request body as `message:send`. Requires `capabilities.streaming = true` on the Agent Card.

**Response:** `Content-Type: text/event-stream`

```
event: message
data: {"jsonrpc":"2.0","result":{"kind":"task","task":{...}},"id":"1"}
```

`result.kind` values:

| kind | Description |
|------|-------------|
| `task` | Initial or updated Task snapshot |
| `status-update` | Task state change |
| `artifact-update` | Partial or final output chunk |
| `message` | Agent message in the conversation |

### 5.3 Get task (`tasks/get`)

```http
GET https://agent.supplygraph.ai/a2a/agents/tariff_calc/tasks/{task_id}?historyLength=10
A2A-Version: 1.0
Authorization: Bearer {api_key}
```

Returns the current Task object, including `artifacts` when the task has reached a terminal state.

Query parameters:

| Parameter | Description |
|-----------|-------------|
| `historyLength` | Number of history messages to include (optional) |
| `contextId` | Context ID filter (optional) |

### 5.4 JSON-RPC binding

All A2A methods may also be invoked via the JSON-RPC binding at `{agent_base}`:

```http
POST https://agent.supplygraph.ai/a2a/agents/tariff_calc
Content-Type: application/json
A2A-Version: 1.0
Authorization: Bearer {api_key}
```

**Example (`message/send`):**

```json
{
  "jsonrpc": "2.0",
  "method": "message/send",
  "params": {
    "message": {
      "messageId": "msg-uuid",
      "role": "user",
      "parts": [{ "kind": "text", "text": "Hello" }]
    }
  },
  "id": "1"
}
```

**Supported methods:**

| method | Description |
|--------|-------------|
| `message/send` | Send a message |
| `message/stream` | Send a message (SSE response) |
| `tasks/get` | Query a task |
| `tasks/cancel` | Cancel a task |
| `tasks/resubscribe` | Re-subscribe to task SSE stream |
| `agent/getAuthenticatedExtendedCard` | Get authenticated extended Agent Card |

> Push notification webhooks are registered inline via `configuration.pushNotificationConfig` on the initial `message/send` or `message/stream` request (see §6). Separate `tasks/pushNotificationConfig/*` management endpoints are not used.

---

## 6. Push Notifications (Webhook)

SupplyGraph AI supports A2A push notifications for **task completion callbacks**. When an agent declares `capabilities.pushNotifications = true`, clients may register a webhook that receives a single notification after the task reaches a **terminal state**.

### 6.1 Register a webhook (first request only)

Provide `pushNotificationConfig` inside `configuration` on the **initial** `message:send` or `message:stream` request — i.e. when starting a **new** task and **`message.taskId` is not set**.

```json
{
  "message": {
    "messageId": "msg-uuid",
    "role": "user",
    "parts": [{ "kind": "text", "text": "Analyze supply chain risk for Tesla" }]
  },
  "configuration": {
    "blocking": false,
    "pushNotificationConfig": {
      "id": "webhook-config-uuid",
      "url": "https://client.example.com/webhook/a2a",
      "token": "optional-client-verification-token"
    }
  }
}
```

| Field | Required | Description |
|-------|----------|-------------|
| `pushNotificationConfig.url` | Yes | HTTPS endpoint that accepts the completion POST |
| `pushNotificationConfig.id` | No | Client-defined config identifier |
| `pushNotificationConfig.token` | No | Optional shared secret; returned in `X-A2A-Notification-Token` on delivery |

**Rules:**

- Webhook registration is accepted **only on the first execution request** for a task (no `message.taskId`).
- Follow-up messages in a multi-turn flow (with an existing `message.taskId`) **do not** update or replace the webhook.
- One webhook registration applies to the entire task lifecycle until a terminal state is reached.

### 6.2 Webhook delivery (on task completion)

When the task reaches a terminal state (`completed`, `failed`, or `canceled`), SupplyGraph AI sends **one POST** to the registered `url`.

**Request:**

```http
POST https://client.example.com/webhook/a2a
Content-Type: application/a2a+json
A2A-Version: 1.0
Authorization: Bearer {platform_jwt}
X-A2A-Notification-Token: optional-client-verification-token
```

**Body:**

```json
{
  "statusUpdate": {
    "taskId": "task-uuid",
    "status": {
      "state": "completed",
      "timestamp": "2026-06-29T12:05:00Z"
    }
  }
}
```

| Field | Description |
|-------|-------------|
| `statusUpdate.taskId` | The task that has finished |
| `statusUpdate.status.state` | Terminal state: `completed`, `failed`, or `canceled` |
| `statusUpdate.status.timestamp` | UTC ISO 8601 timestamp of the notification |

After receiving the webhook, clients SHOULD call `tasks/get` to retrieve the full Task (including `artifacts`).

> Webhooks notify **completion only**. Intermediate states (`working`, `input-required`) are not pushed; use `message/stream` or poll `tasks/get` for in-progress updates.

### 6.3 Verify webhook authenticity

The `Authorization` header carries a platform-issued JWT. Verify it using the public keys published at:

```
GET https://agent.supplygraph.ai/.well-known/jwks.json
```

JWT claims:

| Claim | Description |
|-------|-------------|
| `iss` | Platform issuer (`https://agent.supplygraph.ai/a2a`) |
| `aud` | Must match your webhook URL |
| `sub` / `taskId` | The task ID being notified |

If you supplied `pushNotificationConfig.token`, also verify the `X-A2A-Notification-Token` header matches your token.

---

## 7. Multi-Turn Interaction

When a Task reaches `input-required`, send another message with the same `contextId` and `taskId`:

```json
{
  "message": {
    "messageId": "msg-uuid-2",
    "role": "user",
    "parts": [{ "kind": "text", "text": "Yes, proceed with that company." }],
    "contextId": "ctx-uuid",
    "taskId": "task-uuid"
  },
  "configuration": { "blocking": false },
  "metadata": {}
}
```

Continue until the Task reaches a terminal state (`completed`, `failed`, `canceled`, or `rejected`).

> `pushNotificationConfig` is **not** re-read on follow-up messages. Register it only on the first request.

---

## 8. Error Handling

A2A errors follow a standard envelope:

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

| JSON-RPC Code | HTTP Status | reason | Description |
|---------------|-------------|--------|-------------|
| `-32001` | 404 | `TASK_NOT_FOUND` | Task does not exist |
| `-32002` | 400 | `TASK_NOT_CANCELABLE` | Task cannot be canceled |
| `-32003` | 400 | `PUSH_NOTIFICATION_NOT_SUPPORTED` | Push notifications not supported |
| `-32004` | 400/401/404 | `INVALID_REQUEST`, `UNAUTHORIZED`, `AGENT_NOT_FOUND` | Bad input, auth failure, or unknown agent |
| `-32005` | 400 | `CONTENT_TYPE_NOT_SUPPORTED` | Unsupported content type |
| `-32006` | 500 | `INVALID_AGENT_RESPONSE` | Agent returned an invalid response |
| `-32007` | 400 | `EXTENDED_AGENT_CARD_NOT_CONFIGURED` | Extended Agent Card not available |
| `-32009` | 400 | `VERSION_NOT_SUPPORTED` | Unsupported A2A version |

---

## 9. Streaming Events (SSE)

When using `message/stream`, each SSE `data` payload is a JSON-RPC 2.0 response.

**Status update:**

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

**Artifact update:**

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

Stream or poll until `status.state` reaches a terminal value, or handle `input-required` for multi-turn flows.

---

## 10. Security & Access Control

- All execution and task requests require `Authorization: Bearer {api_key}`
- Agents that require authentication declare it in their Agent Card
- Use TLS 1.2 or later for all HTTPS connections
- Push notification webhooks are authenticated with a platform JWT (see §6.3); verify via `/.well-known/jwks.json`

- Use `contextId` and `taskId` for tracing multi-step workflows

---

## 11. Versioning

| Layer | Version | Notes |
|-------|---------|-------|
| **A2A protocol** | `1.0` | Negotiated via `A2A-Version` request header |
| **Agent** | per-agent `version` in Agent Card | Independent agent releases |

Clients SHOULD send:

```http
A2A-Version: 1.0
```

Incompatible version requests are rejected with error code `-32009` (`VERSION_NOT_SUPPORTED`).

---

## 12. Related Documentation

📘 **Getting Started**  
https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/getting-started.md

🤖 **Agent Library**  
https://github.com/SupplyGraphAI/supplygraph-ai/tree/main/docs/agents

🌐 **ARD Resource Catalog**  
https://supplygraph.ai/.well-known/ai-catalog.json

📦 **Python A2A SDK**  
https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk

🌐 **Official Website**  
https://www.supplygraph.ai

---

## 13. SupplyGraph AI A2A SDK (Python)

➡️ **[SupplyGraph AI A2A Python SDK](https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk)**

The SDK provides:

- Agent discovery (Well-Known URI, Registry, Agent Card)
- `message/send` and `message/stream`
- Task management (`tasks/get`)
- Push notification webhooks (§6)
- Adapters for LangChain, LangGraph, AutoGen, CrewAI, Google A2A, MCP, and more

---

## 14. Why A2A Matters

Traditional APIs expose static functions.  
The SupplyGraph AI A2A Protocol exposes **interoperable agents** — discoverable via standard Agent Cards, invokable by any A2A-compliant client, and composable in multi-agent orchestration stacks.

This enables:

- **Standards-based** agent discovery and invocation (A2A 1.0 + ARD)
- **Cross-platform** agent orchestration
- Multi-step, multi-turn autonomous workflows
- Native integration with the broader agentic ecosystem

A2A is the foundation for next-generation autonomous enterprise intelligence on SupplyGraph AI.
