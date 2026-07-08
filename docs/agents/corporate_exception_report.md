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


## Output Format

Primary output lives in `data.content` (Agent API) or task artifacts (A2A / MCP). The payload is a **oneOf**:

| State | `content` type | Description |
|-------|----------------|-------------|
| In progress, failed, cancelled, subscription inactive, or **waiting for user input** | `string` | Text or Markdown — validation prompts, confirmation requests, or error messages |
| **Task completed successfully** | `object` | Structured corporate exception report (see below) |

**Structured success object:**

```json
{
  "type": "results",
  "data": {
    "languages": ["en", "zk"],
    "report_infos": {
      "en": {
        "report_date": "2025-11-30",
        "report_name": "Corporate Exception Report — Tesla, Inc.",
        "report_type": "enterprise changes report"
      },
      "zh": {
        "report_date": "2025-11-30",
        "report_name": "企业异常报告 — Tesla, Inc.",
        "report_type": "enterprise changes report"
      }
    },
    "chapter_infos": [
      {
        "en": {
          "chapter_name": "Compliance",
          "sub_sections": [],
          "chapter_texts": ["Markdown paragraph…"]
        },
        "zh": {
          "chapter_name": "合规风险",
          "sub_sections": [],
          "chapter_texts": ["Markdown 段落…"]
        }
      }
    ]
  }
}
```

| Field | Description |
|-------|-------------|
| `type` | Always `"results"` on success |
| `data.languages` | Language codes in the response — `en` (English), `zk` (Chinese) |
| `data.report_infos` | Report metadata keyed by language (`en` required; `zh` when Chinese is included) |
| `data.chapter_infos[]` | Array of exception-domain chapters, each with `en` / `zh` blocks |
| `chapter_infos[].en\|zh.chapter_name` | Chapter title (e.g. `Compliance`, `Reputation`, `Ops`) |
| `chapter_infos[].en\|zh.sub_sections[]` | Nested sub-sections (`section_title`, `sub_sections`, `section_texts`) |
| `chapter_infos[].en\|zh.chapter_texts[]` | Chapter body paragraphs (Markdown) |

During an **active subscription**, each `results` call returns the **latest snapshot** of the monitored company — not a one-time static export.

Initial baseline build: **~2–3 hours** (Production). Sandbox returns instantly.


## Sample Response (Sandbox)

> Sandbox returns Tesla, Inc. or BYD fixture data with the **same structure** as Production. Content below is abbreviated.

**Success (`TASK_COMPLETED`) — Agent API `results`:**

```json
{
  "success": true,
  "code": "TASK_COMPLETED",
  "message": "Task completed successfully.",
  "data": {
    "task_id": "<task-id>",
    "agent": "corporate_exception_report",
    "stage": "completed",
    "progress": 100,
    "content": {
      "type": "results",
      "data": {
        "languages": ["en", "zk"],
        "report_infos": {
          "en": {
            "report_date": "2025-11-30",
            "report_name": "Corporate Exception Report — Tesla, Inc.",
            "report_type": "enterprise changes report"
          },
          "zh": {
            "report_date": "2025-11-30",
            "report_name": "企业异常报告 — Tesla, Inc.",
            "report_type": "enterprise changes report"
          }
        },
        "chapter_infos": [
          {
            "en": {
              "chapter_name": "Compliance",
              "sub_sections": [
                {
                  "section_title": "Regulatory Actions",
                  "sub_sections": [],
                  "section_texts": ["No material regulatory enforcement actions detected in the monitoring window."]
                }
              ],
              "chapter_texts": ["### Summary\nCompliance posture remains stable with no critical exceptions flagged."]
            },
            "zh": {
              "chapter_name": "合规风险",
              "sub_sections": [],
              "chapter_texts": ["### 摘要\n监控周期内未发现重大合规异常信号。"]
            }
          },
          {
            "en": {
              "chapter_name": "Reputation",
              "sub_sections": [],
              "chapter_texts": ["### Media Exposure\nNegative media sentiment remains within normal baseline levels."]
            },
            "zh": {
              "chapter_name": "声誉风险",
              "sub_sections": [],
              "chapter_texts": ["### 媒体曝光\n负面舆情处于正常基线范围内。"]
            }
          }
        ]
      }
    }
  },
  "metadata": { "credits_used": 0 },
  "errors": null
}
```


## Multi-turn Example (A2A)

When the agent cannot unambiguously match a company from `text`, it returns `WAITING_USER` with a Markdown prompt in `content` (string). Continue with the **same `task_id`**.

**Turn 1 — submit company name (optional `chapter_name`):**

```bash
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/corporate_exception_report/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"text": "Tesla, Inc. United States", "chapter_name": "ALL", "stream": false}'
```

Poll until `WAITING_USER`. Example content:

```json
{
  "success": true,
  "code": "WAITING_USER",
  "data": {
    "task_id": "<task-id>",
    "content": "Here are the companies we've identified.\nCompany Name: Tesla, Inc.\nCountry: United States\nPlease reply [Yes] or [No] to confirm."
  }
}
```

**Turn 2 — confirm entity (same `task_id`):**

```bash
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/corporate_exception_report/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"text": "yes", "task_id": "<task-id>", "stream": false}'
```

The agent activates the subscription and begins baseline data preparation. Poll `status` every **~10 seconds** until `TASK_COMPLETED` (initial build **~2–3 h** in Production), then retrieve the report via `results`.

After the initial build completes, use `results` only to fetch the latest snapshot — no further `status` polling is required during the subscription.

See [Agent API §8](../agent-api/agent-api.md#8-multi-turn-waiting_user) for the general multi-turn pattern.


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
