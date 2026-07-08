# Corporate Exception Report Agent

## Overview

The **Corporate Exception Report Agent** continuously monitors and analyzes companies to detect **critical exceptions, risk signals, regulatory events, and abnormal changes** that can impact operational stability, compliance posture, or reputation.

It transforms fragmented, unstructured external information into a **structured, verified, and continuously updated intelligence report**, enabling organizations to respond to corporate risks in near real-time.


## Pain

Traditional corporate monitoring is slow, fragmented, and resource-intensive.

Analysts frequently spend **10+ hours per company** gathering information from scattered public sources — and by the time a report is completed, much of the data is already outdated.

Most organizations also lack:

- Continuous monitoring capability  
- Automated exception detection  
- Centralized, standardized reporting  

This leads to blind spots in:

- Compliance  
- Governance  
- Financial exposure  
- Supply chain risk  
- Reputation management  


## Breakthrough

The Corporate Exception Report Agent automates what previously required multiple teams and systems.

Behind the scenes, it:

- Monitors millions of enterprise signals in real time  
- Detects abnormal events and exception patterns  
- Structures findings into standardized intelligence modules  
- Continuously refreshes risk status  

For the user, the workflow remains simple:

1. Provide a company name  
2. Confirm the matched entity  
3. Receive a detailed, structured exception report  

Typical insights include:

- Regulatory and compliance risks  
- Legal or administrative issues  
- Reputation and media exposure  
- Operational anomalies  
- Governance concerns  


## Why SupplyGraph AI

Powered by:

- 100M+ global companies  
- 8,000+ industry benchmarks  
- 1M+ key products and entities  

SupplyGraph AI provides a **continuously updated, audit-ready corporate exception intelligence layer** without requiring any proprietary data.

This makes the agent suitable for:

- Compliance teams  
- Risk & governance functions  
- Investors and analysts  
- Procurement and sourcing  
- Enterprise decision-makers  


# Agent Behavior Model  

## Sandbox Key Support (for Development)

This agent fully supports **Sandbox API Keys**, enabling developers to test integrations without consuming credits.

When using a Sandbox Key:

- No credits are deducted  
- The agent returns **predefined real sample datasets** (e.g., Tesla, BYD)  
- Data is real but **static** — it does not update over time  
- Responses do *not* trigger real monitoring or data ingestion  
- Processing is accelerated for instant return  
- Only validation and schema checks are executed  

Sandbox Keys are ideal for:

- UI prototyping  
- API integration testing  
- SDK development  
- CI/CD pipelines  

⚠️ **Sandbox data must not be used for production analytics.**

The full A2A workflow (`run → status → results`) behaves the same as production, enabling accurate integration testing.


## Initial Data Preparation (First-Time Subscription)

When a user initiates a new monitoring request (via `/run` without a `task_id`), the agent begins constructing a **full baseline dataset** for the specified company.

This includes:

- Historical anomaly reconstruction  
- Compliance and governance signal extraction  
- Exception classification  
- Full structuring of all exception categories  

**This initial build requires approximately 2–3 hours.**

First-time workflow:

1. User calls `/run` without task_id → system creates task_id  
2. User confirms the matched company  
3. Agent begins data preparation  
4. User polls the Status Endpoint  
5. Once `TASK_COMPLETED` is returned → first dataset is ready  
6. User retrieves the initial report via `mode=results`  

After this initial build:

- All updates become **incremental**  
- Status polling is no longer required  


## Subscription & Billing Model

When a user confirms monitoring for a specific company:

- A subscription becomes active  
- The system deducts **265 credits per month**  
- The agent continuously monitors and updates exception findings  
- Users can retrieve the latest structured report any time via `results`  

A subscription remains active as long as:

- Credit deductions succeed  
- The user does not cancel the subscription  


## Understanding task_id and Subscription Lifecycle

A `task_id` represents a **subscription instance**, not a company identity.

### How task_id is created and maintained

- `/run` without task_id → system generates a new one  
- After company confirmation → task_id becomes the subscription ID  
- task_id remains valid throughout the subscription period  
- The system recognizes subscriptions **only by task_id**, not company name  

Implications:

- If a user loses their task_id and initiates a new `/run`, the system treats it as a **new subscription**, even for the same company  
- If the subscription ends (cancellation or failed credit deduction), the task_id becomes inactive  
- Any resubscription generates a **new task_id**  


## Results Endpoint Behavior During Subscription

The `results` endpoint always returns the **current latest snapshot** for the monitored company, as long as the task_id is active.

- It may be called repeatedly  
- It always reflects the freshest available risk dataset  
- Once the subscription becomes inactive, no content is returned  


## Recommended Polling Strategy

- During the initial 2–3 hour build → poll `status` every **10 seconds**  
- After initial completion → do not poll `status`  
- Use `results` to fetch the latest updated snapshot  


## Subscription Cancellation

A subscription may end through:

### 1. User-Initiated Cancellation

Users may cancel by calling `/run` with `mode=cancel` + task_id.

- Service continues until end of billing period  
- After expiration, task_id becomes inactive  

#### Example: Cancel Subscription

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/corporate_exception_report/run \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
        "mode": "cancel",
        "task_id": "<task-id>"
      }'
```

##### Example Response

```json
{
  "success": true,
  "code": "TASK_CANCELLED",
  "data": {
    "task_id": "<task-id>",
    "agent": "corporate_exception_report",
    "stage": "cancelled",
    "content": "Your subscription has been successfully cancelled. Monitoring will continue until the current billing period ends."
  }
}
```


### 2. Credit Deduction Failure

If credits cannot be deducted:

- The subscription terminates immediately  
- task_id becomes inactive  
- `results`/`status` return no meaningful data  

Any restart requires a **new subscription**, resulting in a new task_id.


## Input Requirements

`agent_id`: `corporate_exception_report` · MCP tool: `corporate_exception_report`

| Field | Required | Description |
|-------|----------|-------------|
| `text` | Yes | Company name and country |
| `chapter_name` | No | Exception domain; default `ALL` |

**`chapter_name` values:** `ALL`, `Profile`, `License`, `GovRel`, `R&D`, `Reputation`, `Ops`, `GeneralRisks`, `Cost`, `Compliance`, `Competition`, `Brand`, `HR`

**Supported modes:** `run`, `status` (initial build only), `results`, `cancel`. See Agent Behavior Model above for subscription lifecycle.


## Integration

| Method | ID / Tool | Documentation |
|--------|-----------|---------------|
| **A2A** | `corporate_exception_report` | [a2a.md](../a2a_mcp/a2a.md) |
| **MCP** | `corporate_exception_report` | [mcp.md](../a2a_mcp/mcp.md) |
| **Agent API** | `corporate_exception_report` | [agent-api.md](../agent-api/agent-api.md) |

Quick examples: [A2A / MCP](../a2a_mcp/quick_example.md) · [Agent API](../agent-api/quick_example.md)

## Errors

Common codes → [Agent API §10](../agent-api/agent-api.md#10-error--status-codes). Supports `mode=cancel` for subscription cancellation. Returns `WAITING_USER` for company confirmation.

Maintainer: info@supplygraph.ai  
License: Proprietary / Internal  
© 2025–2026 SupplyGraph AI. All rights reserved.
