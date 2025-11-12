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

All timestamps use UTC ISO format `YYYY-MM-DDTHH:MM:SSZ`.


## 2. Transport & Authentication

| Property | Description |
|-----------|--------------|
| **Transport** | HTTPS (primary) or WebSocket (optional for persistent connections) |
| **Protocol** | JSON-based envelope compatible with JSON-RPC 2.0 semantics |
| **Authentication** | Bearer token (API key) or internal service token |
| **Encoding** | UTF-8 JSON |
| **Direction** | Bidirectional (agents may act as both sender and receiver) |

Each A2A message or HTTP request must include an `Authorization: Bearer <token>` header.  
Unauthorized or malformed requests return `code: UNAUTHORIZED`.


## 3. Message Envelope
All A2A communications use a structured JSON envelope consistent with agent responses.

```json
{
  "source_agent": "tariff_monitoring",
  "target_agent": "visualization",
  "intent": "tariff_change",
  "payload": {
    "hts_code": "9903.88.15",
    "change_summary": "Additional duty reduced from 25% to 15%",
    "effective_date": "2025-11-01"
  },
  "timestamp": "2025-11-12T09:00:00Z",
  "metadata": {
    "version": "1.0.0",
    "signature": "<sha256>",
    "trace_id": "a2a-873bc1"
  }
}
```

- **timestamp** uses UTC ISO format: `YYYY-MM-DDTHH:MM:SSZ`  
- **metadata.signature** is a signed SHA-256 digest ensuring integrity  
- **trace_id** allows end-to-end event tracing across agents


## 4. Standard Intents
| Intent | Description | Typical Source | Typical Target |
|--------|--------------|----------------|----------------|
| `tariff_change` | Notify tariff schedule changes | Tariff Monitoring Agent | Visualization Agent |
| `risk_alert` | Propagate risk alerts | Risk Sentinel Agent | Dashboard Agent |
| `supply_update` | Notify supply graph updates | Graph Builder Agent | Visualization Agent |
| `due_diligence_ready` | Notify due diligence report completion | Due Diligence Agent | Consulting Partner Agent |
| `agent_status` | Query target agent readiness | Any | Any |
| `agent_manifest` | Retrieve target agent manifest | Any | Target Agent |

Each intent corresponds to a `mode` value when invoking the `/api/v1/agents/{agent_id}/run` endpoint (e.g., `mode=run` for execution).


## 5. Response Schema
Responses mirror the standard agent envelope format:

```json
{
  "success": true,
  "code": "ACKNOWLEDGED",
  "message": "Tariff change event received and processed.",
  "metadata": {
    "target_agent": "visualization",
    "timestamp": "2025-11-12T09:10:00Z"
  }
}
```

Error example:

```json
{
  "success": false,
  "code": "INVALID_INTENT",
  "message": "Unrecognized A2A intent.",
  "errors": { "detail": "Intent not registered in target manifest." },
  "timestamp": "2025-11-12T09:10:05Z"
}
```


## 6. Event Flow Example

1. **Tariff Monitoring Agent** detects a new duty rate.  
2. It sends a `tariff_change` event to the **Visualization Agent** via HTTPS `/run` call.  
3. The Visualization Agent acknowledges the event with `code: ACKNOWLEDGED`.  
4. Optionally, the **Risk Sentinel Agent** may trigger a secondary event (`risk_alert`).

Each event maps to a `/run` request on the target agent.  
If asynchronous, `status` and `results` modes can be used to poll progress or retrieve final output.


## 7. Error Handling
A2A errors align with the unified status code definitions from the agent-level API.

| Code | Description |
|------|--------------|
| `UNAUTHORIZED` | Missing or invalid token |
| `INVALID_INTENT` | Unsupported or unknown intent |
| `MALFORMED_PAYLOAD` | Schema violation or invalid JSON |
| `TARGET_UNAVAILABLE` | Destination agent offline or unregistered |
| `TASK_FAILED` | Target agent execution failed |
| `TIMEOUT` | No acknowledgment received within allowed window |

All responses use `success`, `code`, and `message` fields.


## 8. Retry & Acknowledgment
Agents may implement acknowledgment and retry policies for reliability.

Example acknowledgment:

```json
{
  "ack": true,
  "event_id": "d4f7a91",
  "received_at": "2025-11-12T09:05:30Z",
  "metadata": {
    "target_agent": "visualization",
    "trace_id": "a2a-873bc1"
  }
}
```

Guidelines:
- A retry SHOULD occur if no acknowledgment is received within 3 seconds.  
- Use exponential backoff for retries (up to 3 attempts).  
- Events are idempotent—duplicate deliveries must be safely ignored.


## 9. Security & Access Control
- All A2A requests are authenticated via **JWT or API tokens**.  
- Each agent defines its access scope and capabilities in its `/manifest`.  
- Only authorized agents may publish or subscribe to a given intent.  
- All data in transit must be protected via HTTPS/WSS using **TLS 1.3+**.  
- Sensitive payloads (e.g., company data, tariff updates) are encrypted when required.  
- Cross-agent communication is logged and traceable via `trace_id`.


## 10. Versioning
Each A2A envelope includes a `metadata.version` field.  
Breaking changes in schema or authentication will increment the minor or major version accordingly.  
Agents SHOULD validate compatible versions before accepting events.


## 10. Related Documentation
- [Getting Started Guide](./getting-started.md)  
- [Agent Specifications](./agents/)
