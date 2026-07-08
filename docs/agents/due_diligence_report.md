# Due Diligence Agent


## Overview

The **Due Diligence Agent** generates a comprehensive, structured, and continually updated intelligence report for any company worldwide.

It consolidates fragmented public data — including corporate registration, legal records, subsidiaries, IP assets, compliance, financial disclosures, workforce, and regulatory signals — into a **standardized, enterprise-grade due diligence dossier**.

This agent is designed for:

- M&A screening
- Supplier onboarding
- Investment research
- Risk and compliance reviews
- Strategic partner evaluation


## Pain

Traditional due diligence is slow, fragmented, and expensive.

Analysts often spend **10+ hours per company** collecting unstructured information from dozens of sources — and by the time the report is finished, much of the data is already outdated.

Key challenges include:

- Manual data aggregation
- Inconsistent formats
- Incomplete coverage
- Lack of continuous monitoring
- High research costs

This creates blind spots in strategic decision-making and risk management.


## Breakthrough

The Due Diligence Agent automates a process that previously required multiple teams and tools.

Behind the scenes, it:

- Aggregates data from a global corporate graph
- Resolves entities and normalizes records
- Structures information into standardized modules
- Continuously refreshes and updates changes

For users, the workflow is simple:

1. Provide a company name
2. Confirm the matched entity
3. Receive a complete or chapter-specific due diligence report

Results are:

- Structured
- Auditable
- Enterprise-ready
- Continuously updated


## Why SupplyGraph AI

Powered by:

- 100M+ global companies
- 8,000+ industry benchmarks
- 1M+ key products and entities

SupplyGraph AI delivers **institution-grade due diligence intelligence** without requiring you to upload any proprietary internal data.

Every insight is:

- Evidence-linked
- Contextualized
- Continuously monitored
- Built for compliance and risk teams


## Try the Due Diligence Agent (Live Chatbot)

Before integrating via API, you can experience this agent directly through our interactive chatbot.

This live demo allows you to:

- Enter a company name
- Confirm the correct legal entity
- Generate a full or chapter-specific report
- Preview the structure and quality of the final output

Launch the Due Diligence Chatbot  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=due_diligence_report

To use the chatbot, you’ll first need to:

- Create a SupplyGraph AI account
- Top up your credit balance

The chatbot is powered by the **same Due Diligence Agent and A2A endpoints** described in this documentation.  
Credits consumed in the chatbot are deducted in the same way as API / A2A usage.

Everything you experience here can be fully embedded into your own system through A2A integration.

---

## Sandbox Key Support (for Development)

This agent supports **Sandbox API Keys**, enabling developers to test integrations without consuming credits.

When using a Sandbox Key:

- No credits are deducted
- The agent returns **predefined real sample data** for companies such as **Tesla, Inc.** or **BYD**, depending on your input
- Returned data is real and structured, but limited to the predefined sample set
- Processing is accelerated, and results are returned instantly
- Only input validation and structural checks are performed

Sandbox Keys are ideal for:

- UI development
- Validating your processing logic against realistic datasets
- SDK / API behavior testing

⚠️ Sandbox results are not dynamically generated — they come from a predefined dataset and must not be used in production analytics.

For a full comparison of Production vs. Sandbox keys, see  
[Getting Started Guide → API Keys](../getting-started.md#3-generate-an-a2amcp-key).


## Input Requirements

`agent_id`: `due_diligence_report` · MCP tool: `due_diligence_report`

Pricing is **per chapter** — see [Chapter pricing](#chapter-pricing) below. Full report (`ALL`): **26,400 credits**.

Input differs by integration surface:

### A2A & Agent API

| Field | Required | Description |
|-------|----------|-------------|
| `text` | Yes | Company name with country/region (e.g. `"Tesla, Inc. United States"`) |
| `chapter_name` | No | Report section to generate; default `ALL` |

**Example:** `"Tesla, Inc. United States"` with optional `"chapter_name": "Company Registration Information"`

The agent resolves the company name and may ask you to **confirm the matched entity** before report generation starts (see [Multi-turn Example (A2A)](#multi-turn-example-a2a)).

### MCP

MCP does **not** use multi-turn company confirmation on `due_diligence_report`. Resolve `pid` first, then pass `pid` and `chapter_name`.

| Step | MCP tool | Input | Output |
|------|----------|-------|--------|
| 1 | `search_company_candidates` | `text` | `candidates[].pid` |
| 2 | `due_diligence_report` | `pid`, `chapter_name` | Due diligence report |

**Step 1 example:** `{"text": "Tesla United States"}`  
**Step 2 example:** `{"pid": "a77828f060c866441f2403384b271e63", "chapter_name": "ALL"}`

`search_company_candidates` pricing: **1 credit / run**. See [MCP tool config](#mcp-tool-config) below.

Full machine-readable schemas → `GET /api/v1/agents/due_diligence_report/manifest` (see [Agent API §3](../agent-api/agent-api.md#3-manifest)).


### Chapter pricing

| `chapter_name` | Credits / run |
|----------------|---------------|
| `ALL` (full report) | 26,400 |
| `Company Registration Information` | 1,518 |
| `Branch Offices` | 1,650 |
| `Patent Holdings` | 12,045 |
| `Shareholder Structure` | 1,386 |
| … | … |

> 42 chapters total. See agent `manifest` → `pricing.options` for the complete list and current rates.


## Output Format

Primary output lives in `data.content` (Agent API) or task artifacts (A2A / MCP). The payload is a **oneOf**:

| State | `content` type | Description |
|-------|----------------|-------------|
| In progress, failed, cancelled, or **waiting for user input** | `string` | Text or Markdown — validation prompts, confirmation requests, or error messages |
| **Task completed successfully** | `object` | Structured due diligence report (see below) |

**Structured success object:**

```json
{
  "type": "results",
  "data": {
    "languages": ["en", "zk"],
    "report_infos": {
      "en": {
        "report_date": "2025-11-30",
        "report_name": "Due Diligence Report — Tesla, Inc.",
        "report_type": "dd report"
      },
      "zh": {
        "report_date": "2025-11-30",
        "report_name": "尽职调查报告 — Tesla, Inc.",
        "report_type": "dd report"
      }
    },
    "chapter_infos": [
      {
        "en": {
          "chapter_name": "Company Registration Information",
          "sub_sections": [],
          "chapter_texts": ["Markdown paragraph…"]
        },
        "zh": {
          "chapter_name": "工商注册信息",
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
| `data.chapter_infos[]` | Array of chapters, each with `en` / `zh` blocks |
| `chapter_infos[].en\|zh.chapter_name` | Chapter title |
| `chapter_infos[].en\|zh.sub_sections[]` | Nested sub-sections (`section_title`, `sub_sections`, `section_texts`) |
| `chapter_infos[].en\|zh.chapter_texts[]` | Chapter body paragraphs (Markdown) |

Estimated task duration: **1–10 hours** (Production). Sandbox returns instantly.

After entity confirmation (A2A) or `pid` submission (MCP), poll `status` until `TASK_COMPLETED`, then call `results`.


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
    "agent": "due_diligence_report",
    "stage": "completed",
    "progress": 100,
    "content": {
      "type": "results",
      "data": {
        "languages": ["en", "zk"],
        "report_infos": {
          "en": {
            "report_date": "2025-11-30",
            "report_name": "Due Diligence Report — Tesla, Inc.",
            "report_type": "dd report"
          },
          "zh": {
            "report_date": "2025-11-30",
            "report_name": "尽职调查报告 — Tesla, Inc.",
            "report_type": "dd report"
          }
        },
        "chapter_infos": [
          {
            "en": {
              "chapter_name": "Company Registration Information",
              "sub_sections": [
                {
                  "section_title": "Basic Registration",
                  "sub_sections": [],
                  "section_texts": ["Tesla, Inc. is registered in the State of Delaware, United States."]
                }
              ],
              "chapter_texts": ["### Overview\nCorporate registration records confirm active standing."]
            },
            "zh": {
              "chapter_name": "工商注册信息",
              "sub_sections": [],
              "chapter_texts": ["### 概述\n工商登记信息显示企业处于存续状态。"]
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

> **A2A and Agent API only.** MCP uses `search_company_candidates` → `due_diligence_report` instead of multi-turn confirmation.

When the agent cannot unambiguously match a company from `text`, it returns `WAITING_USER` with a Markdown prompt in `content` (string). Continue with the **same `task_id`**.

**Turn 1 — submit company name (optional `chapter_name` in body or via manifest defaults):**

```bash
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/due_diligence_report/run" \
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
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/due_diligence_report/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"text": "yes", "task_id": "<task-id>", "stream": false}'
```

The agent queues report generation. Poll `status` until `TASK_COMPLETED` (may take **1–10 h** in Production), then retrieve structured `results` via `results`.

See [Agent API §8](../agent-api/agent-api.md#8-multi-turn-waiting_user) for the general multi-turn pattern.


## MCP Resolution Workflow

### Step 1 — `search_company_candidates`

```json
{
  "type": "company_candidate_search",
  "data": {
    "candidates": [
      {
        "pid": "a77828f060c866441f2403384b271e63",
        "company_name": "Tesla, Inc.",
        "country_name": "United States"
      }
    ]
  }
}
```

### Step 2 — `due_diligence_report`

```python
report = await session.experimental.call_tool_as_task(
    "due_diligence_report",
    {
        "pid": "a77828f060c866441f2403384b271e63",
        "chapter_name": "Company Registration Information",  # or "ALL"
    },
)
# poll until complete; parse type: "results"
```


## MCP Tool Config

MCP `input_schema` for `due_diligence_report`:

```json
{
  "enabled": true,
  "input_schema": {
    "type": "object",
    "additionalProperties": false,
    "properties": {
      "pid": {
        "type": "string",
        "description": "Internal company ID for the target enterprise, obtained from the search_company_candidates MCP tool (e.g. a77828f060c866441f2403384b271e63 for Tesla, Inc.)."
      },
      "chapter_name": {
        "type": "string",
        "description": "Report chapter to generate. Use ALL for the full due diligence report, or a specific chapter name from the agent manifest.",
        "enum": [
          "ALL",
          "Company Registration Information",
          "Branch Offices",
          "Corporate Brand Initiatives",
          "Administrative Sanctions",
          "Software Copyright Details",
          "Outbound Investments",
          "Financing Activities",
          "Competitor Analysis",
          "Subsidiary Companies",
          "Trademark Portfolio",
          "Patent Holdings",
          "Website Registrations",
          "Court Judgments",
          "Shareholder Structure",
          "Senior Management Team",
          "Administrative Permits",
          "Court Hearing Notices",
          "Court Notices",
          "Equity Pledges",
          "Mobile Applications",
          "Copyrighted Works",
          "Equity Freezes",
          "Chattel Mortgages",
          "WeChat Official Accounts",
          "Tendering and Bidding Activities",
          "Qualification Certificates",
          "Engineering Irregularities",
          "Major Regulatory Violations",
          "Compensation and Benefits",
          "Enforcement Targets",
          "Supplier Network",
          "Credit Ratings",
          "Tax Offenses",
          "Regulatory Spot Checks",
          "Import-Export Credit Records",
          "Regulatory Actions",
          "Granted Government Subsidies",
          "Eligible Government Subsidies",
          "Consolidated Statements of Operations",
          "Income Statement",
          "Statement of Cash Flows",
          "Consolidated Balance Sheets"
        ],
        "default": "ALL"
      }
    },
    "required": ["pid"]
  },
  "execution": {
    "taskSupport": "required"
  },
  "runtime": {
    "poll_interval_seconds": 30,
    "timeout_seconds": 600,
    "task_ttl_ms": 36000000
  }
}
```

> `chapter_name` is optional; omit to generate the full report (`ALL`). Production reports may run **1–10 h** — consider increasing `task_ttl_ms` accordingly.


## Integration

| Method | ID / Tool | Documentation |
|--------|-----------|---------------|
| **A2A** | `due_diligence_report` | [a2a.md](../a2a_mcp/a2a.md) |
| **MCP** | `search_company_candidates` + `due_diligence_report` | [mcp.md](../a2a_mcp/mcp.md) |
| **Agent API** | `due_diligence_report` | [agent-api.md](../agent-api/agent-api.md) |

Call pattern is identical across agents — only `agent_id` / tool name and input differ.

Quick examples: [A2A / MCP](../a2a_mcp/quick_example.md) · [Agent API](../agent-api/quick_example.md)


## Errors

Common codes → [Agent API §10](../agent-api/agent-api.md#10-error--status-codes).

| Situation | Code | Agent behavior |
|-----------|------|----------------|
| Ambiguous or unmatched company (A2A / Agent API) | `WAITING_USER` | Prompts for entity confirmation; resume with same `task_id` |
| Invalid `chapter_name` | `WAITING_USER` | Returns guidance to choose a valid chapter from the enum |
| Invalid `pid` (MCP) | `TASK_FAILED` | Returns guidance in `content` (string) — re-run `search_company_candidates` |
| Insufficient credits | `INSUFFICIENT_CREDITS` | Top up via [Console](https://supplygraph.ai/zk_chat_os/dashboard/dashboard.html) |
| Report still generating | `TASK_RUNNING` | Continue polling `status`; Production may take up to **10 hours** |

Maintainer: info@supplygraph.ai  
License: Proprietary / Internal  
© 2025–2026 SupplyGraph AI. All rights reserved.
