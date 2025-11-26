# Agent-to-Agent (A2A) Protocol

The **SupplyGraph AI A2A Protocol** defines how autonomous agents collaborate within the SupplyGraph ecosystem.  
It standardizes message formats, authentication, and interaction patterns between agents, complementing the `/api/v1/agents` HTTP interface used by each individual agent.


## 1. Overview
The **Agent-to-Agent (A2A) Protocol** defines a unified, public-facing interface that allows external developers and systems to interact with any SupplyGraph AI Agent as an independent API service.

Each agent — such as the **Tariff Classification Agent**, **Due Diligence Agent**, or **Risk Sentinel Agent** — operates autonomously, exposing its own `/manifest`, `/run` endpoints through the A2A gateway.

Through this protocol, developers can:
- Discover agents and their capabilities via `/manifest`
- Invoke complex analytical or data-processing tasks using `/run`(default `mode=run`, supports streaming).
- Query task status via `POST /run` with `mode=status` (non‑streaming).
- Retrieve final results via `POST /run` with `mode=results` (non‑streaming).

> All timestamps use UTC ISO format `YYYY-MM-DDTHH:MM:SSZ`.


## 2. Transport & Authentication
| Property | Description |
|-----------|-------------|
| Transport | HTTPS (primary). WebSocket optional for persistent channels. |
| Protocol | JSON envelope aligned with individual agent responses. |
| Authentication | `Authorization: Bearer <token>` (API key or service token). |
| Encoding | UTF‑8 JSON. |
| Direction | Request/response. Agents may act as both caller and callee. |

> Unauthorized or malformed requests return `success: false` with `code: UNAUTHORIZED`.


## 3. Endpoints Summary
A2A surfaces a single POST endpoint with **mode switching** plus a read-only manifest,
enabling a unified interaction pattern across heterogeneous systems and agent-based architectures.


| Endpoint | Method | Mode | Streaming | Purpose |
|----------|--------|------|-----------|---------|
| `/api/v1/agents/{agent_id}/manifest` | GET | — | No | Retrieve metadata, versions, and schemas. |
| `/api/v1/agents/{agent_id}/run` | POST | `run` (default) | Yes | Start a task. May return immediately or assign a `task_id`. |
| `/api/v1/agents/{agent_id}/run` | POST | `status` | No | Query task status using `task_id`. |
| `/api/v1/agents/{agent_id}/run` | POST | `results` | No | Retrieve final output using `task_id`. |

> Streaming is currently supported **only** when `mode=run` and `"stream": true`.


## 4. Message Envelope

A2A requests and responses share a consistent structure across agents.

### 4.1 Request (generic)

```json
{
  "mode": "run",
  "text": "example task input",
  "stream": true,
  "task_id": null,
  "extra": {}
}
```

- `mode` ∈ {`run` (default), `status`, `results`}.
- `stream` applies only to `mode=run`.
- `task_id` required for `status` and `results`.
- `extra` is agent‑specific options.

> This unified envelope allows orchestration layers to handle agents generically without
hardcoding agent-specific contracts.


### 4.2 Response (envelope)

```json
{
  "success": true,
  "code": "TASK_ACCEPTED",
  "message": "Task accepted and queued for execution.",
  "data": {
    "task_id": "t_123",
    "agent": "example_agent",
    "stage": "executing",
    "code": "TASK_ACCEPTED",
    "progress": 0,
    "timestamp": "2025-11-12T09:00:10Z",
    "is_final": true,
    "content": ""
  },
  "metadata": {
    "agent": "example_agent",
    "timestamp": "2025-11-12T09:00:10Z",
    "credits_used": 0
  },
  "errors": null
}
```


## 5. Streaming Events (mode=run only)

When `"stream": true`, SupplyGraph AI exposes its internal reasoning and
execution phases through Server-Sent Events (SSE), enabling real-time
observability into autonomous agent behavior.


| Event | Stage | Code | Description |
|-------|-------|------|-------------|
| `stream` | `interpreting` | `THINKING` | Reasoning/analysis updates. |
| `stream` | `executing` | `TASK_ACCEPTED` | Task accepted and queued/started. |
| `end` | — | — | Stream completed. |

Example event blocks:

#### Event 1 — Interpreting (THINKING)
```json
{
  "event": "stream",
  "data": {
    "task_id": "t_123",
    "agent": "example_agent",
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
    "task_id": "t_123",
    "agent": "example_agent",
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


## 6. Error Handling
Unified codes (align with agent docs):
| Code | Description |
|------|-------------|
| `UNAUTHORIZED` | Missing or invalid token |
| `INVALID_REQUEST` | Bad parameters or out‑of‑scope task |
| `INSUFFICIENT_CREDITS` | Not enough credits |
| `RATE_LIMITED` | Too many requests |
| `TASK_FAILED` | Execution failed |
| `INVALID_INTENT` | Unknown/unsupported semantic intent |
| `TARGET_UNAVAILABLE` | Destination offline/unregistered |
| `TIMEOUT` | No response within allowed window |

All responses include `success`, `code`, and `message`. See agent docs for stage‑specific codes per lifecycle.


## 7. Retry & Acknowledgment(Forward-Looking / Reserved)
This section describes **future-oriented reliability mechanisms** designed for advanced Agent-to-Agent and event-driven communication scenarios.

In **v1.0**, SupplyGraph AI primarily operates on a **request–task–result** model:

`run → status → results`

Requests are initiated by the client and responses are retrieved via task polling.  
**Acknowledgment (ACK) and message retry mechanisms are not required for normal operation in this version.**

However, as SupplyGraph AI evolves to support:
- Direct Agent-to-Agent task delegation
- Event-driven orchestration
- Pub/Sub or message-queue-based delivery
- Cross-system autonomous collaboration

a standardized acknowledgment and retry mechanism will be introduced to ensure **reliable**, **idempotent delivery** of agent instructions and events.

### **Example (future format)**
```json
{
  "ack": true,
  "event_id": "d4f7a91",
  "received_at": "2025-11-12T09:05:30Z",
  "metadata": {
    "target_agent": "example_agent",
    "trace_id": "a2a-873bc1"
  }
}
```

### **Planned Guidelines**
When this mechanism is activated in future versions:
- If no acknowledgment is received within **3 seconds**, the sender SHOULD retry.
- Use **exponential backoff**, capped at **3 retry attempts**.
- All event deliveries MUST be **idempotent** (duplicate events must not trigger duplicate processing).
- Every event SHOULD carry a unique `event_id` and optional `trace_id` for observability.

This section is intentionally included to establish **forward protocol compatibility** and to enable future support for fully autonomous, distributed multi-agent architectures within the SupplyGraph AI ecosystem.


## 8. Security & Access Control
- Authenticate with Bearer tokens (API keys or signed service tokens).  
- Authorize per agent and scope; agents publish capabilities in `/manifest`.  
- Use TLS 1.3+ for HTTPS/WSS; encrypt sensitive payloads as policy requires.  
- Log cross‑agent activity with a `trace_id` for observability.


## 9. Versioning(Forward Compatibility Strategy)
In the current **v1.0** release, SupplyGraph AI focuses on providing a **stable and simplified integration surface** for external developers and systems.

At this stage:
- A fixed API path structure is used (`/api/v1/agents/...`)
- Breaking changes are **avoided**
- Backward compatibility within the same major version is preserved

Formal, multi-version negotiation using `metadata.version` is **not required for standard client integrations at this time**.

However, in future releases, SupplyGraph AI will introduce:
- Semantic versioning for agent capabilities
- Explicit protocol version fields in the metadata layer
- Compatibility validation between agents and orchestration systems
- Graceful rejection of incompatible requests with `code: INVALID_REQUEST` and a descriptive message

This design ensures that:

✅ Existing integrations remain stable  
✅ New capabilities can evolve independently  
✅ Multi-agent ecosystems can safely operate across versions  
✅ Enterprise orchestration platforms can programmatically validate compatibility

> Note: Forward compatibility is built into the protocol design from day one,
> even when not fully activated in the current version.


## 10. Related Documentation

Explore more about the SupplyGraph AI ecosystem:

📘 **Getting Started Guide**  
  https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/getting-started.md

🤖 **Agent Specifications & Library**  
  https://github.com/SupplyGraphAI/supplygraph-ai/tree/main/docs/agents

🧠 **SupplyGraph AI Documentation Hub**  
  https://github.com/SupplyGraphAI/supplygraph-ai

📦 **Python A2A SDK (Official Repository)**  
  https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk

🌐 **Official Website & Use Cases**  
  https://www.supplygraph.ai

📄 **Future Protocols & Enterprise Specifications** *(Coming Soon)*  
  MCP, multi-agent orchestration & deployment whitepapers


## 11. SupplyGraph AI A2A SDK (Python)

Looking for the official Python SDK for integrating SupplyGraph A2A Agents?

➡️ **[SupplyGraph AI A2A Python SDK](https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk)**  
Directly maintained under the same GitHub organization.

This SDK bridges SupplyGraph AI into the broader Agentic & LLM ecosystem, 
allowing seamless integration with modern multi-agent stacks.

The SDK includes:
- Core A2A API client  
- Multi-round BaseAgent abstraction  
- Full adapter ecosystem (LangChain, LangGraph, AutoGen, CrewAI, DSPy, Semantic Kernel, Flowise, Haystack, Airflow, BentoML, Google A2A, MCP)  
- Documentation & integration examples  


## 12. Why A2A Matters

Traditional APIs expose static functions.  
The SupplyGraph AI A2A Protocol exposes **living agents** — capable of reasoning, 
decision-making, adaptation, and collaboration.

This enables:

- True **Agent-to-Agent collaboration**
- Cross-platform **agent orchestration**
- Multi-step autonomous workflows
- Native integration with AI-native ecosystems
- Scalable, distributed thinking architectures

In other words, A2A is not just a technical interface — it is the foundation
of **next-generation autonomous enterprise intelligence**.
