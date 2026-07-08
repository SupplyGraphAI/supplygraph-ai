# Supply Chain Risk Prediction Agent

## Overview

The **Supply Chain Risk Prediction Agent** continuously monitors global supply chain risk events and evaluates whether, how, and to what extent those events may affect a target company.

It transforms fragmented global signals into **structured, auditable, and continuously updated enterprise risk warnings**, enabling users to understand event relevance, propagation logic, risk severity, impacted nodes, estimated timing, and recommended actions.

The agent supports three interpretation roles (via optional `role` on `mode=result`):

| Role | Audience |
|------|----------|
| `ceo` | Corporate executives — enterprise exposure, business impact, severity, timing |
| `hedgefund` | Hedge fund managers — operational impact, financial relevance, market signals |
| `riskexpert` | Supply chain risk professionals — nodes, paths, mechanisms, mitigation |

Each role receives the same underlying risk intelligence with adapted presentation.


## Pain

Organizations face an overwhelming volume of global supply chain signals every day. Most incidents are irrelevant to a given company, while the few that matter are buried in news, policy updates, market data, and fragmented operational information.

Without structured risk propagation analysis, users often lack:

- Clear visibility into whether an event is genuinely relevant to the target company
- Multi-tier dependency paths explaining how impact may reach the enterprise
- Quantified estimates of direct and indirect impacted nodes
- Business-level interpretation of risk status and severity
- Actionable recommendations before the risk fully materializes

Traditional monitoring tools detect global events but usually cannot explain how those events propagate through complex supply chain networks.


## Breakthrough

The Supply Chain Risk Prediction Agent extends event monitoring into **company-specific, multi-tier supply chain risk propagation analysis**.

Behind the scenes, it:

- Monitors global supply chain risk events in real time
- Filters incidents to surface genuinely relevant enterprise signals
- Identifies direct and indirect impacted supply chain nodes
- Traces multi-tier propagation paths between the event and the target company
- Quantifies enterprise-level risk status, severity, confidence, and timing
- Produces structured, auditable warning outputs

Typical insights include enterprise risk status, key paths and nodes, impact dates, risk summary, impact graph data, and analysis dossier text.


## Why SupplyGraph AI

Powered by:

- 100M+ global companies
- 1M+ key products and components
- Real-time global signal capture running 24/7
- Enterprise-product dependency graph intelligence
- Multi-hop dependency tracing up to 10 tiers
- Fully auditable propagation pathways

SupplyGraph AI connects global risk events to company-specific exposure — suitable for executives, investors, risk professionals, procurement teams, and enterprise risk management.


## Try the Supply Chain Risk Prediction Agent (Live Chatbot)

Three role-specific chatbot demos share the same agent API; each preview adapts output emphasis to the role:

| Role | Chatbot |
|------|---------|
| Corporate Executives (`ceo`) | https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=risk_propagation_detail_ceo |
| Hedge Fund Managers (`hedgefund`) | https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=risk_propagation_detail_hedgefund |
| Supply Chain Risk Professionals (`riskexpert`) | https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=risk_propagation_detail_riskexpert |

To use a chatbot or API:

- Create a SupplyGraph AI account
- Top up your credit balance

Credits are deducted the same way as API / A2A usage. Everything you experience here can be embedded into your own system.

---

## Sandbox Key Support (for Development)

This agent supports **Sandbox API Keys** — test integrations without consuming credits.

When using a Sandbox Key:

- No credits are deducted
- Returns predefined sample enterprise risk assessment data
- Data is static and does not update over time
- No live monitoring or live risk computation is triggered
- Only validation and schema checks are executed

⚠️ **Sandbox data must not be used for production analytics.**

The full workflow (`run → status → result`) behaves the same as production.

See [Getting Started → API Keys](../getting-started.md#3-generate-an-a2amcp-key).


## Input Requirements

`agent_id`: `supply_chain_risk_prediction` · MCP tool: `supply_chain_risk_prediction`

**Important:** This agent uses `mode=result` (not `results`) to retrieve output.

Supported top-level modes: `run` · `status` · `result`

### A2A & Agent API

Serialize the payload as JSON in the `text` field:

```python
text = json.dumps({"event_info": event_info})
```

| Field | Required | Description |
|-------|----------|-------------|
| `text` | Yes | JSON string containing `event_info` (see below) |
| `mode` | No | `run` (default), `status`, or `result` |
| `task_id` | Conditional | Required when continuing after `WAITING_USER`, or for `status` / `result` |
| `role` | No | On `mode=result` only: `ceo` \| `hedgefund` \| `riskexpert` |

### MCP

MCP does **not** use multi-turn company confirmation. Resolve `pid` first, then pass `pid` with `event_info` (use `pid` instead of `company_name`).

| Step | MCP tool | Input |
|------|----------|-------|
| 1 | `search_company_candidates` | `text` → `candidates[].pid` |
| 2 | `supply_chain_risk_prediction` | `pid` + `event_info` (no `company_name`) |

Full schema → `GET /api/v1/agents/supply_chain_risk_prediction/manifest` ([Agent API §3](../agent-api/agent-api.md#3-manifest)).

### Analysis mode (`event_info.analysis_mode`)

| Mode | Purpose | Pricing |
|------|---------|---------|
| `normal` | Live / forward-looking event impact analysis | **$49,999** per event per target company (one-time) |
| `backtest` | Historical case validation, benchmarking, model testing | **30%** of normal price; first **10** backtest cases free |

Fee is charged only after the user confirms the matched company (`confirm_company_name: "Yes"`).

### Event types (`event_info.event_type`)

| Type | Use case |
|------|----------|
| `news` | News event impact on target company |
| `policy` | Policy or regulatory event impact |
| `commodity_price` | Commodity price change impact |

### Event Info — common fields

| Field | Required | Description |
|-------|----------|-------------|
| `event_type` | Yes | `news` \| `policy` \| `commodity_price` |
| `analysis_mode` | Yes | `normal` \| `backtest` |
| `company_name` | Yes (A2A) | Target company — include country/ticker when possible, e.g. `Tesla, Inc. (United States, NASDAQ: TSLA)` |
| `confirm_company_name` | Conditional | `Yes` \| `No` — required when continuing from `WAITING_USER` |

### Event Info — `news` or `policy`

| Field | Required | Description |
|-------|----------|-------------|
| `title` | Yes | Event title |
| `content` | Yes | Full event content or description |
| `pub_date` | Yes | RFC 3339 date-time with timezone, e.g. `2026-03-17T00:00:00-04:00` |

### Event Info — `commodity_price`

| Field | Required | Description |
|-------|----------|-------------|
| `commodity_name` | Yes | Commodity name |
| `commodity_unit` | Yes | Price unit, e.g. `USD/Bbl` |
| `target_date` | Yes | `YYYY-MM-DD` |
| `target_price` | Yes | Price on `target_date` |
| `commodity_price_series` | Yes | ≥ 90 days of `{date, value}`; max date = `target_date` |

### Task lifecycle

- `/run` without `task_id` → creates a new assessment task
- After company confirmation → task activates; one-time fee charged
- `mode=result` + `task_id` → latest assessment snapshot (repeatable)
- `mode=status` → check setup completion; usually unnecessary after initial completion
- Lost `task_id` → new `/run` creates a new priced task


## Output Format

On success, `mode=result` returns structured assessment data in `data.content` (A2A) or the result payload (Agent API). Intermediate states return `content` as a **string** (Markdown).

**Top-level result structure (abbreviated):**

```json
{
  "success": true,
  "task_id": "<task-id>",
  "event": {
    "event_id": "<event-id>",
    "event_info": {
      "event_date": "2026-06-22",
      "event_type": "Supply Chain Disruption",
      "event_summary": "A supply chain event that may affect the target company."
    }
  },
  "company": {
    "company_id": "<company-id>",
    "company_name": "Tesla, Inc.",
    "company_info": { "industry": "...", "main_products": [] }
  },
  "assessment_result": {
    "assessment_time": "2026-06-22T00:00:00Z",
    "task_status": 2,
    "risk_summary": "...",
    "final_interpretation": "...",
    "impact_result": {},
    "path_risk_interpretation_list": [],
    "impact_graph_data": {},
    "analysis_dossier_text": ""
  },
  "node_assessment_list": [],
  "dossier_paragraph_list": []
}
```

| Field | Description |
|-------|-------------|
| `event` | Event metadata and summary |
| `company` | Target company identity and profile |
| `assessment_result.impact_result` | Enterprise-level impact — `overall_summary`, `risk_level`, `key_nodes`, `key_paths`, `management_actions`, etc. |
| `assessment_result.path_risk_interpretation_list` | Path-level propagation analysis |
| `assessment_result.impact_graph_data` | Graph nodes/edges/paths for visualization |
| `assessment_result.analysis_dossier_text` | Full narrative dossier |
| `node_assessment_list` | Per-node evaluation records |
| `dossier_paragraph_list` | Structured dossier paragraphs (`paragraph_title`, `paragraph_text`, `is_key_conclusion`, etc.) |

Optional `role` on `mode=result` adapts interpretation emphasis without changing underlying data.


## Sample Response (Sandbox)

> Sandbox returns static sample assessment data with the **same structure** as Production.

**Success — `mode=result`:**

```json
{
  "success": true,
  "code": "TASK_COMPLETED",
  "data": {
    "task_id": "<task-id>",
    "agent": "supply_chain_risk_prediction",
    "content": {
      "success": true,
      "task_id": "<task-id>",
      "event": {
        "event_id": "<event-id>",
        "event_info": {
          "event_date": "2026-06-22",
          "event_type": "news",
          "event_summary": "Sample supply chain event for Sandbox testing."
        }
      },
      "company": {
        "company_id": "a77828f060c866441f2403384b271e63",
        "company_name": "Tesla, Inc.",
        "company_info": { "industry": "Automotive", "main_products": [] }
      },
      "assessment_result": {
        "assessment_time": "2026-06-22T00:00:00Z",
        "task_status": 2,
        "risk_summary": "No material enterprise-level supply chain risk identified in Sandbox sample.",
        "final_interpretation": "Sandbox fixture — no live propagation analysis executed.",
        "impact_result": {},
        "path_risk_interpretation_list": [],
        "impact_graph_data": {},
        "analysis_dossier_text": "### Sandbox Note\nThis is predefined sample data for integration testing."
      },
      "node_assessment_list": [],
      "dossier_paragraph_list": []
    }
  },
  "metadata": { "credits_used": 0 },
  "errors": null
}
```


## Multi-turn Example (A2A)

> **A2A and Agent API only.** MCP passes `pid` directly — no company confirmation turn.

**Turn 1 — submit event with company name:**

```bash
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/supply_chain_risk_prediction/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "mode": "run",
    "text": "{\"event_info\": {\"event_type\": \"news\", \"analysis_mode\": \"normal\", \"title\": \"Sample Event\", \"content\": \"Event description...\", \"pub_date\": \"2026-03-17T00:00:00-04:00\", \"company_name\": \"Tesla, Inc.\"}}",
    "stream": false
  }'
```

Poll until `WAITING_USER`. Example content:

```json
{
  "success": true,
  "code": "WAITING_USER",
  "data": {
    "task_id": "<task-id>",
    "stage": "interpreting",
    "content": "Here are the companies we've identified.\nTarget Company Name: Tesla, Inc.\nCountry: United States\nPlease reply [Yes] or [No] to confirm."
  }
}
```

**Turn 2 — confirm company (same `task_id`, resend full `event_info` + confirmation):**

```bash
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/supply_chain_risk_prediction/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "mode": "run",
    "task_id": "<task-id>",
    "text": "{\"event_info\": {\"event_type\": \"news\", \"analysis_mode\": \"normal\", \"title\": \"Sample Event\", \"content\": \"Event description...\", \"pub_date\": \"2026-03-17T00:00:00-04:00\", \"company_name\": \"Tesla, Inc.\", \"confirm_company_name\": \"Yes\"}}",
    "stream": false
  }'
```

| `confirm_company_name` | Behavior |
|------------------------|----------|
| `Yes` | Activates assessment; one-time fee charged |
| `No` | Task not activated; no fee; start a new `/run` with corrected company info |

Retrieve results:

```bash
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/supply_chain_risk_prediction/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"mode": "result", "task_id": "<task-id>", "role": "ceo", "stream": false}'
```

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

### Step 2 — `supply_chain_risk_prediction`

Pass `pid` and `event_info` **without** `company_name`:

```python
event_info = {
    "event_type": "news",
    "analysis_mode": "normal",
    "title": "Sample Event",
    "content": "Event description...",
    "pub_date": "2026-03-17T00:00:00-04:00",
}

await session.experimental.call_tool_as_task(
    "supply_chain_risk_prediction",
    {
        "pid": "a77828f060c866441f2403384b271e63",
        "event_info": event_info,
    },
)
```


## Integration

| Method | ID / Tool | Documentation |
|--------|-----------|---------------|
| **A2A** | `supply_chain_risk_prediction` | [a2a.md](../a2a_mcp/a2a.md) |
| **MCP** | `search_company_candidates` + `supply_chain_risk_prediction` | [mcp.md](../a2a_mcp/mcp.md) |
| **Agent API** | `supply_chain_risk_prediction` | [agent-api.md](../agent-api/agent-api.md) |

Uses `mode=result` (not `results`). Quick examples: [A2A / MCP](../a2a_mcp/quick_example.md) · [Agent API](../agent-api/quick_example.md)


## Errors

Common codes → [Agent API §10](../agent-api/agent-api.md#10-error--status-codes).

| Situation | Code | Agent behavior |
|-----------|------|----------------|
| Company match needs confirmation (A2A) | `WAITING_USER` | Display `content`; continue with `confirm_company_name` in `event_info` |
| User rejects company match | — | Reply `No`; no fee; start new task |
| Invalid `event_info` or event type | `INVALID_REQUEST` | Returns guidance in `content` (string) |
| Insufficient credits | `INSUFFICIENT_CREDITS` | Top up via [Console](https://supplygraph.ai/zk_chat_os/dashboard/dashboard.html) |
| Invalid `pid` (MCP) | `TASK_FAILED` | Re-run `search_company_candidates` |

Maintainer: info@supplygraph.ai  
License: Proprietary / Internal  
© 2025–2026 SupplyGraph AI. All rights reserved.
