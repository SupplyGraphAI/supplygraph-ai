# Agent-to-Agent (A2A) Protocol

The **SupplyGraph AI A2A Protocol** enables direct collaboration between autonomous agents — allowing one agent to trigger, query, or respond to another within the SupplyGraph ecosystem.

This document defines the message schema, supported events, and recommended integration patterns.



## 1. Overview

A2A (Agent-to-Agent) communication allows multiple agents to work together autonomously.  
For example:
- **Tariff Monitoring Agent** → notifies **Visualization Agent** of tariff changes.  
- **Due Diligence Agent** → provides context to **Risk Sentinel Agent** for dynamic scoring.



## 2. Transport & Authentication

| Property | Description |
|-----------|--------------|
| **Protocol** | JSON-RPC 2.0 over WebSocket or HTTPS |
| **Authentication** | Internal service token or enterprise-issued key |
| **Format** | UTF-8 JSON envelope |
| **Direction** | bidirectional (publish / subscribe) |

Each message is signed with a valid service credential.  
Unauthorized or malformed messages are ignored.



## 3. Message Envelope

All A2A communications follow this format:

```
{
  "source_agent": "TariffMonitoringAgent",
  "target_agent": "VisualizationAgent",
  "intent": "tariff_change",
  "payload": {
    "hts_code": "9903.88.15",
    "change_summary": "Additional duty reduced from 25% to 15%",
    "effective_date": "2025-11-01"
  },
  "timestamp": "2025-10-29T13:00:00Z"
}
```



## 4. Standard Intents

| Intent | Description | Typical Source | Typical Target |
|:--------|:-------------|:----------------|:----------------|
| `tariff_change` | Broadcasts tariff schedule updates | Tariff Monitoring Agent | Visualization Agent |
| `risk_alert` | Sends risk propagation warnings | Risk Sentinel Agent | Dashboard Agent |
| `supply_update` | Pushes supply graph structure changes | Graph Builder Agent | Visualization Agent |
| `due_diligence_ready` | Notifies report completion | Due Diligence Agent | Consulting Partner Agent |



## 5. Response Schema

Responses mirror the same JSON-RPC envelope:

```
{
  "status": "acknowledged",
  "target_agent": "VisualizationAgent",
  "message": "Tariff change received and visualized."
}
```

Error response:

```
{
  "status": "error",
  "code": "INVALID_INTENT",
  "message": "Unrecognized A2A intent."
}
```



## 6. Event Flow Example

1. **Tariff Monitoring Agent** detects a new duty rate.  
2. It publishes a `tariff_change` event via A2A.  
3. **Visualization Agent** receives it and updates the supply graph.  
4. **Risk Sentinel Agent** optionally triggers further analysis.



## 7. Error Handling

| Code | Description |
|------|--------------|
| `INVALID_AUTH` | Missing or invalid internal token |
| `INVALID_INTENT` | Unsupported event type |
| `MALFORMED_PAYLOAD` | JSON schema violation |
| `TARGET_UNAVAILABLE` | Destination agent offline or not registered |



## 8. Retry & Acknowledgment

Agents may implement `ack` and `retry_after` semantics for reliability.  
If an acknowledgment is not received within a defined timeout (e.g., 3 seconds), the event can be retried.

Example acknowledgment:

```
{
  "ack": true,
  "event_id": "d4f7a91",
  "received_at": "2025-10-29T13:05:30Z"
}
```



## 9. Security & Access Control

- All A2A events are authenticated through service tokens.  
- Each agent has a unique ID and access scope.  
- Sensitive payloads (e.g., tariff change data) are encrypted in transit.  
- Agents may only publish or subscribe to registered channels.



## 10. Related Documentation

- [Getting Started Guide](./getting-started.md)  
- [API Reference](./api.md)  
- [Agent Specifications](./agents/)
