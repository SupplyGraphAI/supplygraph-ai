# Enterprise Supply Graph Agent


## Overview

The **Enterprise Supply Graph Agent** generates a multi-tier, company-centric global supply chain graph for any enterprise in the world.

It automatically maps relationships across:

- Tier-1 suppliers
- Tier-N upstream entities
- Products and sub-components
- Regions and countries
- Raw materials and critical inputs

This agent transforms fragmented supply networks into a **living, explorable, and actionable graph** — enabling organizations to see, understand, and manage their real dependencies for the first time.


## Pain

Most companies operate with **severely limited visibility** into their true supply networks.

Without deep-tier transparency:

- Hidden single-country dependencies go undetected
- Alternatives remain invisible
- Regional concentration risks accumulate silently
- Geopolitical or regulatory shocks cause sudden disruption
- Incident response is reactive instead of strategic

Traditional tools only reveal Tier-1 suppliers at best — leaving 80–90% of the real risk surface invisible.


## Breakthrough

The Enterprise Supply Graph Agent solves this by:

- Expanding a company-centered graph node-by-node
- Linking suppliers, products, regions, and materials across multiple tiers
- Structuring dependencies into analyzable paths and clusters
- Creating the foundation for:
  - Concentration risk analysis
  - Alternative supplier discovery
  - Impact simulation
  - Disruption forecasting

For the user, the experience is simple:

1. Enter a company name
2. Confirm the correct entity
3. Instantly visualize an interactive, multi-tier supply network

What was once invisible becomes navigable, measurable, and actionable.


## Why SupplyGraph AI

Powered by:

- 100M+ global companies
- 1M+ key products and components
- 8,000+ industry benchmarks
- Continuous monitoring, 24/7

SupplyGraph AI delivers the **first truly intelligent, real-time, multi-tier enterprise supply graph** in the world — turning hidden dependency structures into strategic advantage.


## Try the Enterprise Supply Graph Agent (Live Chatbot)

Before integrating this agent via API, you can experience it directly through our interactive visualization chatbot.

This live demo allows you to:

- Enter a company name
- Automatically generate a multi-tier supply graph (Tier-1 to deep-tier)
- Visualize product, supplier, and regional dependencies
- Identify potential concentration and exposure risks
- Explore alternative sourcing pathways
- Experience the depth and structure of the intelligence graph

Launch the Enterprise Supply Graph Chatbot  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=sg_visualization

To use the chatbot, you’ll first need to:

- Create a SupplyGraph AI account
- Top up your credit balance

This chatbot is powered by the **same Enterprise Supply Graph Agent and A2A endpoints** described in this documentation.  
Credits used in the chatbot are deducted in the same way as API / A2A usage.

Everything you experience here can be fully embedded into your own system through A2A integration.

---

## Sandbox Key Support (for Development)

This agent supports **Sandbox API Keys**, allowing testing without consuming credits.

When using a Sandbox Key:

- No credits are deducted
- The agent returns a **predefined supply-chain graph dataset for Tesla, Inc.**
- Graph nodes, edges, and attributes follow the final production schema
- No live graph computation is executed; only the static sample dataset is returned
- Input validation and structural checks are performed

This is useful for:

- Front-end visualization testing
- Building graph-based UI components
- Verifying schema compatibility before running real analyses

⚠️ Sandbox datasets are static and do not represent real-time updates.  
Use Production Keys for live graph computation.

For details, see  
[Getting Started Guide → API Keys](../getting-started.md#3-generate-an-a2amcp-key).


## Input Requirements

`agent_id`: `sg_visualization` · MCP tool: `sg_visualization` · Pricing: **264590 credits / run**

Input differs by integration surface:

### A2A & Agent API

| Field | Required | Description |
|-------|----------|-------------|
| `text` | Yes | Natural-language company name (e.g. `"Tesla, Inc."`) |

**Example:** `"Tesla, Inc."`

The agent resolves the company name and may ask you to **confirm the matched entity** before graph computation starts (see [Multi-turn Example (A2A)](#multi-turn-example-a2a)).

### MCP

MCP does **not** use multi-turn company confirmation on `sg_visualization`. Resolve the company ID first, then pass `pid` directly.

| Step | MCP tool | Input | Description |
|------|----------|-------|-------------|
| 1 | `search_company_candidates` | `text` | Company name with optional country/region (e.g. `"Tesla United States"`) |
| 2 | `sg_visualization` | `pid` | Internal company ID from step 1 (`candidates[].pid`) |

**Step 1 example:** `{"text": "Tesla United States"}`  
**Step 2 example:** `{"pid": "a77828f060c866441f2403384b271e63"}`

`search_company_candidates` pricing: **1 credit / run**. See [MCP search tool output](#mcp-company-resolution-search_company_candidates) below.

Full machine-readable schemas → `GET /api/v1/agents/sg_visualization/manifest` (see [Agent API §3](../agent-api/agent-api.md#3-manifest)).


## Output Format

Primary output lives in `data.content` (Agent API) or task artifacts (A2A / MCP). The payload is a **oneOf**:

| State | `content` type | Description |
|-------|----------------|-------------|
| In progress, failed, cancelled, or **waiting for user input** | `string` | Text or Markdown — validation prompts, confirmation requests, or error messages |
| **Task completed successfully** | `object` | Structured supply chain graph (see below) |

**Structured success object:**

```json
{
  "type": "sg_visualization_results",
  "data": {
    "pid": "a77828f060c866441f2403384b271e63",
    "company_name": "Tesla, Inc.",
    "max_depth": 3,
    "dependencies": [
      { "from": "0->-1", "to": "1->1384787", "dep_depth": 1 }
    ],
    "node_info": {
      "1->1384787": {
        "name": "HBM Memory",
        "index_values": [
          { "index_name": "Number of enterprises", "value": 13893.0 }
        ],
        "company_distribution": [
          { "country": "China", "count": 1213 }
        ]
      }
    }
  }
}
```

| Field | Description |
|-------|-------------|
| `type` | Always `"sg_visualization_results"` on success |
| `data.pid` | Unique ID of the company-centered supply chain graph |
| `data.company_name` | Name of the target company |
| `data.max_depth` | Maximum dependency depth (longest path in the hierarchy) |
| `data.dependencies[]` | Graph edges — `from` / `to` use `"level->nodeID"` format; `dep_depth` is edge depth |
| `data.node_info` | Map of node ID → `{ name, index_values[], company_distribution[] }` |

Estimated task duration: **30 minutes – 7 hours** (Production, live graph build). Sandbox returns instantly.

After entity confirmation (A2A) or `pid` submission (MCP), the task enters a long-running **executing** state — poll `status` until `TASK_COMPLETED`, then call `results`.


## Sample Response (Sandbox)

> Sandbox returns the Tesla, Inc. fixture with the **same structure** as Production. Graph data below is abbreviated.

**Success (`TASK_COMPLETED`) — Agent API `results`:**

```json
{
  "success": true,
  "code": "TASK_COMPLETED",
  "message": "Task completed successfully.",
  "data": {
    "task_id": "<task-id>",
    "agent": "sg_visualization",
    "stage": "completed",
    "progress": 100,
    "content": {
      "type": "sg_visualization_results",
      "data": {
        "pid": "a77828f060c866441f2403384b271e63",
        "company_name": "Tesla, Inc.",
        "max_depth": 3,
        "dependencies": [
          { "from": "0->-1", "to": "1->1384787", "dep_depth": 1 },
          { "from": "0->-1", "to": "1->1683397", "dep_depth": 1 },
          { "from": "0->-1", "to": "1->1657001", "dep_depth": 1 }
        ],
        "node_info": {
          "1->1384787": {
            "name": "HBM Memory",
            "index_values": [
              { "index_name": "Number of enterprises", "value": 13893.0 },
              { "index_name": "Number of enterprise software copyrights", "value": 2728.0 }
            ],
            "company_distribution": [
              { "country": "China", "count": 1213 },
              { "country": "Japan", "count": 14 }
            ]
          }
        }
      }
    }
  },
  "metadata": { "credits_used": 0 },
  "errors": null
}
```


## Multi-turn Example (A2A)

> **A2A and Agent API only.** MCP uses the two-tool workflow (`search_company_candidates` → `sg_visualization`) instead of multi-turn confirmation.

When the agent cannot unambiguously match a company from `text`, it returns `WAITING_USER` with a confirmation prompt in `content` (string). Continue with the **same `task_id`**.

**Turn 1 — submit company name:**

```bash
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/sg_visualization/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"text": "Tesla, Inc.", "stream": false}'
```

Poll `status` until `code` is `WAITING_USER`. Example `results` content:

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
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/sg_visualization/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"text": "Yes", "task_id": "<task-id>", "stream": false}'
```

The agent accepts the match, deducts credits, and queues graph computation. Poll `status` until `TASK_COMPLETED` (may take **30 min – 7 h** in Production), then retrieve structured `sg_visualization_results` via `results`.

See [Agent API §8](../agent-api/agent-api.md#8-multi-turn-waiting_user) for the general multi-turn pattern.


## MCP Company Resolution (`search_company_candidates`)

Use this MCP tool **before** `sg_visualization` to resolve a company name into a `pid`. This tool is **single-turn** (no confirmation step).

| Field | Required | Description |
|-------|----------|-------------|
| `text` | Yes | Company name with optional country/region |

**Success — structured output:**

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

Select the appropriate `pid`, then call `sg_visualization`:

```python
# 1. Resolve company ID
search_result = await session.experimental.call_tool_as_task(
    "search_company_candidates",
    {"text": "Tesla United States"},
)
# ... poll until complete, parse candidates[0]["pid"]

# 2. Build supply graph (no multi-turn)
graph_result = await session.experimental.call_tool_as_task(
    "sg_visualization",
    {"pid": "a77828f060c866441f2403384b271e63"},
)
```

Estimated duration for `search_company_candidates`: **5–8 seconds**.


## Integration

| Method | ID / Tool | Documentation |
|--------|-----------|---------------|
| **A2A** | `sg_visualization` | [a2a.md](../a2a_mcp/a2a.md) |
| **MCP** | `search_company_candidates` + `sg_visualization` | [mcp.md](../a2a_mcp/mcp.md) |
| **Agent API** | `sg_visualization` | [agent-api.md](../agent-api/agent-api.md) |

Call pattern is identical across agents — only `agent_id` / tool name and input differ.

Quick examples: [A2A / MCP](../a2a_mcp/quick_example.md) · [Agent API](../agent-api/quick_example.md)


## Errors

Common codes → [Agent API §10](../agent-api/agent-api.md#10-error--status-codes).

| Situation | Code | Agent behavior |
|-----------|------|----------------|
| Ambiguous or unmatched company (A2A / Agent API) | `WAITING_USER` | Prompts for entity confirmation; resume with same `task_id` |
| Invalid or unknown `pid` (MCP) | `TASK_FAILED` | Returns guidance in `content` (string) — re-run `search_company_candidates` |
| Insufficient credits | `INSUFFICIENT_CREDITS` | Top up via [Console](https://supplygraph.ai/zk_chat_os/dashboard/dashboard.html) |
| Graph still building | `TASK_RUNNING` | Continue polling `status`; Production builds may take up to **7 hours** |

Maintainer: info@supplygraph.ai  
License: Proprietary / Internal  
© 2025–2026 SupplyGraph AI. All rights reserved.
