# Geographic Concentration Analysis Agent


## Overview

The **Geographic Concentration Analysis Agent** performs a quantitative, multi-tier evaluation of an enterprise’s global supply chain to identify **country-level and regional over-concentration risks**.

By applying **Herfindahl–Hirschman Index (HHI)** calculations and deep-tier graph traversal, it exposes hidden dependency structures that are often invisible in traditional Tier-1 or Tier-2 analysis.

The agent delivers a **clear, decision-ready report** that highlights systemic vulnerabilities, enabling organizations to assess exposure, design mitigation strategies, and strengthen supply resilience before disruption occurs.


## Pain

Most companies believe their supply chain is diversified — but in reality, **hidden concentration often exists deep in Tier-3 through Tier-8 levels** of their network.

Even globally diversified enterprises can rely on a **single dominant geography** for critical upstream inputs without realizing it. In one analysis, Nvidia — widely regarded as having a highly diversified supply chain — was found to have **Tier-6 components with over 90% concentration in a single country** based on HHI analysis.

Such invisible dependencies introduce severe enterprise risks:

- Geopolitical shocks
- Export controls and sanctions
- Natural disasters
- Regional instability

Any one of these can trigger **cascading failures**: component shortages, production delays, revenue loss, and reputational damage.


## Breakthrough

Behind the scenes, SupplyGraph AI executes a node-by-node analysis of a company-centric supply graph, calculating geographic concentration at each tier using statistical benchmarks and real-world enterprise data.

User experience remains simple:

- Provide a company name
- Confirm the matched entity
- Specify and confirm the target country or region
- Receive a structured, fully documented concentration analysis

The final output includes:

- HHI-based concentration metrics
- Country and region dominance scores
- Dependency distribution charts
- Executive-ready risk summaries

Advanced network science, delivered through an intuitive and actionable format.


## Why SupplyGraph AI

Powered by a continuously updated knowledge graph spanning:

- 100M+ companies
- 1M+ key products
- 8,000+ industry benchmarks

SupplyGraph AI is the **first AI-native infrastructure** designed to perform deep-tier geographic dependency analysis at scale — **without requiring any proprietary data uploads from clients**.

Our platform transforms invisible structural risk into **auditable, explainable intelligence** that decision-makers can trust.


## Try the Geographic Concentration Analysis Agent (Live Chatbot)

Before integrating this agent via API, you can experience it directly through our interactive chatbot.

This live demo allows you to:

- Enter a company name
- Confirm the correct entity
- Specify a country or region for concentration analysis
- Identify single-country dependency and hidden exposure risks
- Preview the structure, clarity, and analytical depth of the final report

Launch the Geographic Concentration Analysis Agent  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=sg_chokepoint

To use the chatbot, you’ll first need to:

- Create a SupplyGraph AI account
- Top up your credit balance

The chatbot is powered by the **same Geographic Concentration Analysis Agent and A2A endpoints** described in this documentation.  
Credits consumed in the chatbot are deducted in the same way as API / A2A usage.

Everything you experience here can be fully embedded into your own systems through A2A integration.

---

## Sandbox Key Support (for Development)

This agent supports **Sandbox API Keys**, enabling developers to test without consuming credits.

When using a Sandbox Key:

- No credits are deducted
- The agent returns a **predefined geographic concentration report for Tesla, Inc. in China**
- Returned content follows the agent’s final output schema
- No real computation or statistical modeling is executed under Sandbox mode
- Only structural and format validation is performed

Suitable for:

- Backend integration testing
- Visualizing regional-risk heatmaps
- Building analytics dashboards

⚠️ Sandbox datasets are static samples.  
Production Keys are required for real, dynamic computations.

For more information, see  
[Getting Started Guide → API Keys](../getting-started.md#3-generate-an-a2amcp-key).


## Input Requirements

`agent_id`: `sg_chokepoint` · MCP tool: `sg_chokepoint` · Pricing: **264590 credits / run**

Input differs by integration surface:

### A2A & Agent API

| Field | Required | Description |
|-------|----------|-------------|
| `text` | Yes | Natural-language input — company name, and later country or region when prompted |

**Example (initial):** `"Tesla, Inc."`

The agent resolves the company and country/region through **multi-turn confirmation** before report generation starts (see [Multi-turn Example (A2A)](#multi-turn-example-a2a)).

### MCP

MCP does **not** use multi-turn confirmation on `sg_chokepoint`. Resolve `pid` and `region_name` via helper tools first.

| Step | MCP tool | Input | Output |
|------|----------|-------|--------|
| 1 | `search_company_candidates` | `text` | `candidates[].pid` |
| 2 | `search_region_candidates` | `text` | `candidates[].region_name` |
| 3 | `sg_chokepoint` | `pid`, `region_name` | Concentration report |

**Step 1 example:** `{"text": "Tesla United States"}`  
**Step 2 example:** `{"text": "China"}`  
**Step 3 example:** `{"pid": "a77828f060c866441f2403384b271e63", "region_name": "China"}`

Helper tool pricing: **1 credit / run** each for `search_company_candidates` and `search_region_candidates`.

Full machine-readable schemas → `GET /api/v1/agents/sg_chokepoint/manifest` (see [Agent API §3](../agent-api/agent-api.md#3-manifest)).


## Output Format

Primary output lives in `data.content` (Agent API) or task artifacts (A2A / MCP). The payload is a **oneOf**:

| State | `content` type | Description |
|-------|----------------|-------------|
| In progress, failed, cancelled, or **waiting for user input** | `string` | Text or Markdown — validation prompts, confirmation requests, or error messages |
| **Task completed successfully** | `object` | Structured geographic concentration report (see below) |

**Structured success object:**

```json
{
  "type": "sg_chokepoint_results",
  "data": [
    {
      "content_type": "dir",
      "content": "Summary"
    },
    {
      "content_type": "paragraph",
      "content": "### Research Objective\nThis report applies the Herfindahl–Hirschman Index (HHI) methodology to evaluate the geographic concentration and risk exposure of Tesla, Inc.'s global supply chain."
    }
  ]
}
```

| Field | Description |
|-------|-------------|
| `type` | Always `"sg_chokepoint_results"` on success |
| `data` | Ordered array of report elements |
| `data[].content_type` | `"dir"` — title or heading; `"paragraph"` — body text (may include Markdown) |
| `data[].content` | Text content of the element |

Estimated task duration: **30 minutes – 7 hours** (Production). Sandbox returns instantly.

After entity and region confirmation (A2A) or `pid` + `region_name` submission (MCP), the task enters a long-running **executing** state — poll `status` until `TASK_COMPLETED`, then call `results`.


## Sample Response (Sandbox)

> Sandbox returns the Tesla, Inc. / China fixture with the **same structure** as Production. Report content below is abbreviated.

**Success (`TASK_COMPLETED`) — Agent API `results`:**

```json
{
  "success": true,
  "code": "TASK_COMPLETED",
  "message": "Task completed successfully.",
  "data": {
    "task_id": "<task-id>",
    "agent": "sg_chokepoint",
    "stage": "completed",
    "progress": 100,
    "content": {
      "type": "sg_chokepoint_results",
      "data": [
        {
          "content_type": "dir",
          "content": "Summary"
        },
        {
          "content_type": "paragraph",
          "content": "### Research Objective\nThis report applies the Herfindahl–Hirschman Index (HHI) methodology to evaluate the geographic concentration and risk exposure of Tesla, Inc.'s global supply chain in China."
        },
        {
          "content_type": "dir",
          "content": "HHI Analysis"
        },
        {
          "content_type": "paragraph",
          "content": "Tier-3 through Tier-6 components show elevated single-region dependency scores, with several nodes exceeding the 0.25 HHI threshold for high concentration."
        }
      ]
    }
  },
  "metadata": { "credits_used": 0 },
  "errors": null
}
```


## Multi-turn Example (A2A)

> **A2A and Agent API only.** MCP uses the three-tool workflow (`search_company_candidates` → `search_region_candidates` → `sg_chokepoint`) instead of multi-turn confirmation.

Each `WAITING_USER` response includes a Markdown prompt in `content` (string). Continue with the **same `task_id`**.

**Turn 1 — submit company name:**

```bash
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/sg_chokepoint/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"text": "Tesla, Inc.", "stream": false}'
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

**Turn 2 — confirm company (same `task_id`):**

```bash
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/sg_chokepoint/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"text": "Yes", "task_id": "<task-id>", "stream": false}'
```

Agent prompts for the target country or region.

**Turn 3 — submit country or region (same `task_id`):**

```bash
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/sg_chokepoint/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"text": "China", "task_id": "<task-id>", "stream": false}'
```

Poll until `WAITING_USER` with region confirmation, e.g.:

```json
{
  "success": true,
  "code": "WAITING_USER",
  "data": {
    "task_id": "<task-id>",
    "content": "Country or region identified: **China**.\nPlease reply [Yes] or [No] to confirm."
  }
}
```

**Turn 4 — confirm country or region (same `task_id`):**

```bash
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/sg_chokepoint/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"text": "Yes", "task_id": "<task-id>", "stream": false}'
```

The agent queues report generation. Poll `status` until `TASK_COMPLETED` (may take **30 min – 7 h** in Production), then retrieve `sg_chokepoint_results` via `results`.

See [Agent API §8](../agent-api/agent-api.md#8-multi-turn-waiting_user) for the general multi-turn pattern.


## MCP Resolution Workflow

MCP integrations resolve entities upfront — no confirmation turns on `sg_chokepoint`.

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

### Step 2 — `search_region_candidates`

```json
{
  "type": "region_candidate_search",
  "data": {
    "candidates": [
      { "region_name": "China" }
    ]
  }
}
```

### Step 3 — `sg_chokepoint`

```python
report = await session.experimental.call_tool_as_task(
    "sg_chokepoint",
    {
        "pid": "a77828f060c866441f2403384b271e63",
        "region_name": "China",
    },
)
# poll until complete; parse sg_chokepoint_results
```


## Integration

| Method | ID / Tool | Documentation |
|--------|-----------|---------------|
| **A2A** | `sg_chokepoint` | [a2a.md](../a2a_mcp/a2a.md) |
| **MCP** | `search_company_candidates` + `search_region_candidates` + `sg_chokepoint` | [mcp.md](../a2a_mcp/mcp.md) |
| **Agent API** | `sg_chokepoint` | [agent-api.md](../agent-api/agent-api.md) |

Call pattern is identical across agents — only `agent_id` / tool name and input differ.

Quick examples: [A2A / MCP](../a2a_mcp/quick_example.md) · [Agent API](../agent-api/quick_example.md)


## Errors

Common codes → [Agent API §10](../agent-api/agent-api.md#10-error--status-codes).

| Situation | Code | Agent behavior |
|-----------|------|----------------|
| Ambiguous or unmatched company (A2A / Agent API) | `WAITING_USER` | Prompts for entity confirmation; resume with same `task_id` |
| Country or region needs confirmation (A2A / Agent API) | `WAITING_USER` | Prompts for region confirmation after user supplies country/region name |
| Invalid `pid` or `region_name` (MCP) | `TASK_FAILED` | Returns guidance in `content` (string) — re-run helper tools |
| Insufficient credits | `INSUFFICIENT_CREDITS` | Top up via [Console](https://supplygraph.ai/zk_chat_os/dashboard/dashboard.html) |
| Report still generating | `TASK_RUNNING` | Continue polling `status`; Production may take up to **7 hours** |

Maintainer: info@supplygraph.ai  
License: Proprietary / Internal  
© 2025–2026 SupplyGraph AI. All rights reserved.
