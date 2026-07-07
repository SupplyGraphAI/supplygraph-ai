# Model Context Protocol (MCP)

The **SupplyGraph AI MCP Server** exposes SupplyGraph AI capabilities as [Model Context Protocol (MCP)](https://modelcontextprotocol.io) tools, using **Streamable HTTP** transport and the **MCP Tasks** extension for long-running agent workloads.

**Service URL (production):**

```
https://mcp.supplygraph.ai/mcp
```

Each exposed capability is registered as an MCP **Tool** named by its agent identifier (for example `tariff_calc`, `tariff_classification`, `due_diligence_report`). Tools use structured JSON Schema arguments and return results through the standard MCP Tasks lifecycle.

SupplyGraph also publishes a broader **Agentic Resource Discovery (ARD)** catalog at:

```
https://supplygraph.ai/.well-known/ai-catalog.json
```

Obtain a **SupplyGraph API Key** from the Console and pass it on authenticated requests:

```
Authorization: Bearer {api_key}
```

> All timestamps use UTC ISO 8601 format, e.g. `2026-06-29T12:00:00Z`.

---

## 1. Overview

Through MCP, clients can:

- **Discover tools** via `tools/list`
- **Invoke agents** via task-augmented `tools/call`
- **Poll progress** via `tasks/get`
- **Retrieve final output** via `tasks/result`

### Client workflow

All SupplyGraph MCP tools declare `execution.taskSupport = "required"`. Every invocation follows the MCP Tasks pattern:

```
1. tools/list              → discover available tools and input schemas
2. tools/call + task       → CreateTaskResult (returns taskId)
3. tasks/get (poll)        → Task status updates
4. tasks/result (block)    → CallToolResult when terminal
```

```
┌──────────┐   tools/call    ┌─────────────┐   tasks/get     ┌──────────┐
│  Client  │ ──────────────► │ MCP Server  │ ◄────────────── │  Client  │
│          │ ◄────────────── │             │ ──────────────► │          │
└──────────┘ CreateTaskResult└─────────────┘   Task status   └──────────┘
       │                              │
       │         tasks/result         │
       └─────────────────────────────►│
                                      │ CallToolResult
```

> Do **not** expect synchronous `CallToolResult` directly from `tools/call`. Long- and short-running work both return a **Task** first.

---

## 2. Transport & Authentication

| Property | Description |
|----------|-------------|
| **Transport** | MCP Streamable HTTP |
| **Endpoint** | `POST /mcp` (JSON-RPC 2.0 over HTTP) |
| **Protocol** | MCP with Tasks extension |
| **Authentication** | `Authorization: Bearer {api_key}` |
| **Encoding** | UTF-8 JSON |

Obtain API keys from the SupplyGraph Console:

```
https://supplygraph.ai/zk_chat_os/dashboard/dashboard.html
```

| Method | Authentication |
|--------|----------------|
| `tools/list` | Optional (recommended for production clients) |
| `tools/call` | **Required** |
| `tasks/get` | **Required** |
| `tasks/result` | **Required** |

Unauthorized requests return a JSON-RPC error (`code: -32602`, message `"Unauthorized"`).

---

## 3. Endpoints Summary

| URL | Protocol | Purpose |
|-----|----------|---------|
| `https://mcp.supplygraph.ai/mcp` | MCP Streamable HTTP | Primary MCP endpoint (initialize, tools, tasks) |
| `https://mcp.supplygraph.ai/health` | HTTP JSON | Service liveness and tool count |

MCP methods exposed by the server:

| Method | Purpose |
|--------|---------|
| `initialize` | Session handshake |
| `tools/list` | List available tools |
| `tools/call` | Invoke a tool (task-augmented; returns `CreateTaskResult`) |
| `tasks/get` | Query task status |
| `tasks/result` | Block until terminal state; return `CallToolResult` |

---

## 4. Tools

### 4.1 List tools (`tools/list`)

Returns all MCP-enabled SupplyGraph agents. Each tool corresponds to one agent capability.

**Example tool entry (abbreviated):**

```json
{
  "name": "tariff_calc",
  "title": "U.S. Tariff Calculation",
  "description": "Calculates U.S. tariff rates and duty impacts.",
  "inputSchema": {
    "type": "object",
    "additionalProperties": false,
    "properties": {
      "text": {
        "type": "string",
        "description": "Product description or HS code query."
      }
    },
    "required": ["text"]
  },
  "outputSchema": {
    "type": "object",
    "properties": {
      "code": { "type": "string" },
      "content": { "type": "string" }
    }
  },
  "execution": {
    "taskSupport": "required"
  },
  "annotations": {
    "openWorldHint": true
  }
}
```

| Field | Description |
|-------|-------------|
| `name` | Tool identifier (matches agent ID); used in `tools/call` |
| `title` | Human-readable name |
| `description` | Capability summary; may include pricing notes |
| `inputSchema` | JSON Schema for `tools/call` arguments |
| `outputSchema` | Expected result shape (when available) |
| `execution.taskSupport` | Always `"required"` — task-augmented calls only |

Use `inputSchema` to construct valid `arguments` for each tool. Required fields must be present and non-empty.

### 4.2 Invoke tool (`tools/call`)

Task-augmented invocation is **mandatory** for all SupplyGraph tools.

**Request (conceptual):**

```json
{
  "jsonrpc": "2.0",
  "method": "tools/call",
  "params": {
    "name": "tariff_calc",
    "arguments": {
      "text": "Calculate U.S. import duty for HS 8471 from China"
    },
    "task": {
      "ttl": 3600000
    }
  },
  "id": 1
}
```

| Field | Required | Description |
|-------|----------|-------------|
| `params.name` | Yes | Tool name from `tools/list` |
| `params.arguments` | Yes | Object conforming to the tool's `inputSchema` |
| `params.task.ttl` | No | Task time-to-live in milliseconds (server default: 3,600,000 ms) |

**Response (`CreateTaskResult`):**

```json
{
  "jsonrpc": "2.0",
  "result": {
    "task": {
      "taskId": "task-uuid",
      "status": "working",
      "statusMessage": "Task accepted and queued for execution.",
      "createdAt": "2026-06-29T12:00:00Z",
      "lastUpdatedAt": "2026-06-29T12:00:00Z",
      "ttl": 3600000,
      "pollInterval": 30000
    }
  },
  "id": 1
}
```

Argument validation failures (missing required fields, schema violations) return a JSON-RPC error **before** task creation — the tool is not invoked.

---

## 5. Task Lifecycle

### 5.1 Task statuses

| Status | Description |
|--------|-------------|
| `working` | Task accepted and executing |
| `input_required` | Agent needs additional input; see §6 |
| `completed` | Task finished successfully |
| `failed` | Task failed |
| `cancelled` | Task was cancelled |

Terminal states: `completed`, `failed`, `cancelled`.

### 5.2 Poll task (`tasks/get`)

```json
{
  "jsonrpc": "2.0",
  "method": "tasks/get",
  "params": {
    "taskId": "task-uuid"
  },
  "id": 2
}
```

**Response:**

```json
{
  "jsonrpc": "2.0",
  "result": {
    "taskId": "task-uuid",
    "status": "working",
    "statusMessage": "Analyzing tariff schedule...",
    "createdAt": "2026-06-29T12:00:00Z",
    "lastUpdatedAt": "2026-06-29T12:00:05Z",
    "ttl": 3600000,
    "pollInterval": 30000
  },
  "id": 2
}
```

Poll at intervals suggested by `pollInterval` (milliseconds) until the status is terminal, or proceed directly to `tasks/result` for blocking retrieval.

Only `taskId` is required — the server resolves the associated tool automatically.

### 5.3 Get result (`tasks/result`)

Blocks until the task reaches a terminal state, then returns a `CallToolResult`.

```json
{
  "jsonrpc": "2.0",
  "method": "tasks/result",
  "params": {
    "taskId": "task-uuid"
  },
  "id": 3
}
```

**Success response:**

```json
{
  "jsonrpc": "2.0",
  "result": {
    "content": [
      {
        "type": "text",
        "text": "{\"duty_rate\": 0.075, \"total_duty\": 750.00}"
      }
    ],
    "isError": false,
    "_meta": {
      "io.modelcontextprotocol/related-task": {
        "taskId": "task-uuid"
      }
    }
  },
  "id": 3
}
```

| Field | Description |
|-------|-------------|
| `content` | Result payload (typically `text` or structured JSON as text) |
| `isError` | `true` when the task failed or was cancelled |
| `_meta["io.modelcontextprotocol/related-task"]` | Links the result back to the originating task |

On failure:

```json
{
  "content": [{ "type": "text", "text": "Task failed: insufficient credits" }],
  "isError": true,
  "_meta": {
    "io.modelcontextprotocol/related-task": { "taskId": "task-uuid" }
  }
}
```

---

## 6. Multi-Turn Interaction

When `tasks/get` returns `input_required`, the agent is waiting for additional user input.

1. Read `statusMessage` for guidance on what is needed.
2. Issue a new `tools/call` with updated `arguments` for the same tool.
3. Follow the new `taskId` through `tasks/get` → `tasks/result`.

Each `tools/call` creates a new task. Use the tool's `inputSchema` and `statusMessage` to supply the follow-up input.

---

## 7. Error Handling

MCP errors use JSON-RPC 2.0 error objects:

```json
{
  "jsonrpc": "2.0",
  "error": {
    "code": -32602,
    "message": "Unauthorized"
  },
  "id": 1
}
```

| Scenario | code | message (typical) |
|----------|------|-------------------|
| Missing or invalid API key | `-32602` | `Unauthorized` |
| Unknown tool name | `-32601` | `Unknown tool: {name}` |
| Invalid arguments / schema violation | `-32602` | Description of validation failure |
| Task not found | `-32602` | `Task not found: {taskId}` |
| Agent execution error | `-32602` | Agent error message |

`tools/call` validation and authorization failures return JSON-RPC errors — **not** a `CallToolResult` with `isError: true`.

Task execution failures are returned via `tasks/result` as `CallToolResult` with `isError: true`.

---

## 8. Security & Access Control

- Protect your API key; treat it like a password
- Use TLS 1.2 or later for all connections
- Pass `Authorization: Bearer {api_key}` on every authenticated request
- Validate tool arguments client-side against `inputSchema` before calling
- Store `taskId` securely; it grants access to task status and results for your account

---

## 9. Related Documentation

📘 **Getting Started**  
https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/getting-started.md

🤖 **Agent Library**  
https://github.com/SupplyGraphAI/supplygraph-ai/tree/main/docs/agents

🌐 **ARD Resource Catalog**  
https://supplygraph.ai/.well-known/ai-catalog.json

🌐 **Official Website**  
https://www.supplygraph.ai

🔗 **MCP Specification**  
https://modelcontextprotocol.io/specification

---

## 10. Why MCP Matters

MCP is the emerging standard for connecting AI assistants and agents to external tools and data. By exposing SupplyGraph supply-chain intelligence through MCP:

- **MCP-native clients** (IDEs, assistants, orchestrators) can discover and invoke agents without custom integration code
- **Structured tool schemas** enable reliable, typed invocations with validation
- **Task-based execution** handles long-running analytical workloads gracefully
- **Bearer API key authentication** for secure access

SupplyGraph MCP turns domain-specific supply chain, trade, and risk agents into composable tools in the modern agentic ecosystem.
