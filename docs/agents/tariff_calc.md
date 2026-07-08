# U.S. Tariff Calculation Agent


## Overview

The **U.S. Tariff Calculation Agent** automatically calculates **U.S. import duty rates** and all **applicable additional tariffs** based on:

- Product description or HTS code
- Country of origin
- Applicable trade measures (e.g. Chapter 99, Section 301 / 232 / 201)

It delivers **accurate, auditable, regulation-aware duty calculations** in seconds — enabling instant compliance validation, cost modeling, and sourcing optimization.


## Pain

For customs brokers, procurement teams, and trade compliance specialists, determining a product’s duty burden usually requires:

- Manually searching the HTSUS
- Parsing Chapter 99 notes
- Tracking trade measures and exemptions
- Interpreting complex legal classification rules
- Verifying country-of-origin impacts

This process can take **hours per product**, is highly **error-prone**, and often produces **inconsistent or incomplete results** — exposing companies to penalties, misclassification risks, and unexpected landed costs.


## Breakthrough

SupplyGraph AI transforms this process.

The U.S. Tariff Calculation Agent:

- Automatically determines the correct HS / HTS classification (if not provided)
- Applies real-time base duty rates
- Checks and applies all relevant Chapter 99 & special tariff measures
- Evaluates country-of-origin impacts
- Generates a clear, transparent duty analysis

Manual work drops from **hours to seconds**.

The result is not just a number — it is a **traceable, explainable, policy-backed tariff decision**.


## Why SupplyGraph AI

Powered by:

- Real-time global tariff & trade datasets
- HTS + Chapter 99 rule intelligence
- Graph-based product and country reasoning
- Continuously updated regulatory tracking

Every tariff result delivered by SupplyGraph AI is:

- Explainable
- Evidence-backed
- Policy-aware
- Enterprise-grade

This dramatically reduces compliance risk while enabling smarter sourcing and pricing decisions at scale.


## Try the U.S. Tariff Calculation Agent (Live Chatbot)

Before integrating via API, you can experience this agent through our interactive tariff analysis chatbot.

This live demo allows you to:

- Describe your product in natural language
- Identify the most relevant HTS codes
- Apply the correct country of origin
- Calculate U.S. base duties and applicable additional tariffs (including Chapter 99)
- See how duties impact landed cost in real time
- Compare alternative sourcing or classification scenarios

Launch the U.S. Tariff Calculation Chatbot  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=tariff_calc

To use the chatbot, you’ll need to:

- Create a SupplyGraph AI account
- Top up your credit balance

This chatbot is powered by the **same U.S. Tariff Calculation Agent and A2A endpoints** described below.  
Credits are deducted in the same way as API / A2A usage.

Everything you experience here can be embedded into your own system.

---

## Sandbox Key Support (for Development)

This agent fully supports **Sandbox API Keys**, which allow developers to test integrations without consuming credits.

When using a Sandbox Key:

- No credits are deducted
- The agent returns **mocked sample output** that follows the final response schema
- Returned content is deterministic and does not reflect real-world data
- Long-running computation is skipped and results are returned instantly
- Only input validation and request-format checks are executed

Sandbox Keys are recommended for:

- Local development
- SDK / API integration testing
- CI/CD automation

⚠️ Sandbox Keys do **not** produce real analytical results and must not be used in production systems.

For a full comparison of Production vs. Sandbox keys, see  
[Getting Started Guide → API Keys](../getting-started.md#3-generate-an-a2amcp-key).


## Input Requirements

`agent_id`: `tariff_calc` · MCP tool: `tariff_calc` · Pricing: **10 credits / run**

| Field | Required | Description |
|-------|----------|-------------|
| `text` | Yes | Natural-language input for tariff calculation. Must include a **10-digit HTS code** (e.g. `5601.21.0010`) **or** a product description (the agent will identify applicable HS/HTS codes), plus the **country of origin**. |

**Optional details in `text`:** product weight, quantity, declared merchandise value. If omitted, the agent uses reasonable defaults for a mock-up or scenario-based calculation.

**Example:** `"Calculate import duties for 5601.21.0010, country of origin China, shipment value 200 USD, 50 kg."`

Full machine-readable `input_schema` / `output_schema` → `GET /api/v1/agents/tariff_calc/manifest` (see [Agent API §3](../agent-api/agent-api.md#3-manifest)).


## Output Format

Primary output lives in `data.content` (Agent API) or task artifacts (A2A / MCP). The payload is a **oneOf**:

| State | `content` type | Description |
|-------|----------------|-------------|
| In progress, failed, cancelled, or **waiting for user input** | `string` | Text or Markdown — validation prompts, error messages, or clarification requests |
| **Task completed successfully** | `object` | Structured tariff result (see below) |

**Structured success object:**

```json
{
  "type": "result",
  "data": {
    "calculation_result": "<Markdown string>"
  }
}
```

| Field | Description |
|-------|-------------|
| `type` | Always `"result"` when the calculation completed successfully |
| `data.calculation_result` | Detailed tariff and duty analysis — duty rates, Chapter 99 measures, scenario comparisons, and estimated costs (Markdown) |

Estimated task duration: **1–3 minutes** (Production). Sandbox returns instantly.

Designed for direct consumption after [Customs Classification](./tariff_classification.md), or as a standalone calculation when HTS and origin are already known.


## Sample Response (Sandbox)

> Sandbox returns deterministic mock data with the **same structure** as Production. Content below is abbreviated.

**Success (`TASK_COMPLETED`) — Agent API `results`:**

```json
{
  "success": true,
  "code": "TASK_COMPLETED",
  "message": "Task completed successfully.",
  "data": {
    "task_id": "<task-id>",
    "agent": "tariff_calc",
    "stage": "completed",
    "progress": 100,
    "content": {
      "type": "result",
      "data": {
        "calculation_result": "### Summary of Input Information\n\n- **HTS Code:** 0803.90.00.35 (Bananas, fresh, other)\n- **Country of Origin:** Vietnam (VN)\n...\n\n### Scenario-Based Tariff Calculations\n\n| Scenario | Total Effective Rate | Estimated Total Duty (USD) |\n| Standard Import | 20% | $2,000 |\n| Donation / Humanitarian Aid | 0% | $0 |"
      }
    }
  },
  "metadata": { "credits_used": 0 },
  "errors": null
}
```

The full Sandbox `calculation_result` includes input summary, applicable tariff components (base MFN + Chapter 99), scenario tables, and compliance notes.


## Multi-turn Example

When required information is missing (e.g. **country of origin** or a resolvable **HTS code / product description**), the agent returns `WAITING_USER` with a Markdown prompt in `content` (string). Continue the **same task** with follow-up `text`.

**Turn 1 — missing country of origin:**

```bash
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/tariff_calc/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"text": "Calculate import duties for 5601.21.0010, shipment value 200 USD, 50 kg.", "stream": false}'
```

Poll `status` until `code` is `WAITING_USER`. Example `results` content:

```json
{
  "success": true,
  "code": "WAITING_USER",
  "data": {
    "task_id": "<task-id>",
    "content": "Sorry, we are unable to calculate the tariff. Please provide the **country of origin** of the product."
  }
}
```

**Turn 2 — supply missing information (same `task_id`):**

```bash
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/tariff_calc/run" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"text": "Country of origin is China.", "task_id": "<task-id>", "stream": false}'
```

Poll `status` → `results` until `TASK_COMPLETED`. The agent merges prior context with the follow-up and returns the structured `calculation_result`.

Same `task_id` behavior applies to **A2A** and **MCP** — see [Agent API §8](../agent-api/agent-api.md#8-multi-turn-waiting_user) and [A2A / MCP Quick Example](../a2a_mcp/quick_example.md) (uses `tariff_calc`).


## Integration

| Method | ID / Tool | Documentation |
|--------|-----------|---------------|
| **A2A** | `tariff_calc` | [a2a.md](../a2a_mcp/a2a.md) |
| **MCP** | `tariff_calc` | [mcp.md](../a2a_mcp/mcp.md) |
| **Agent API** | `tariff_calc` | [agent-api.md](../agent-api/agent-api.md) |

Call pattern is identical across agents — only `agent_id` / tool name and input `text` differ.

Quick examples: [A2A / MCP](../a2a_mcp/quick_example.md) · [Agent API](../agent-api/quick_example.md)


## Errors

Common codes → [Agent API §10](../agent-api/agent-api.md#10-error--status-codes).

| Situation | Code | Agent behavior |
|-----------|------|----------------|
| Missing country of origin | `WAITING_USER` | Prompts user to provide origin; resume with same `task_id` |
| Unresolvable HTS / vague product description | `WAITING_USER` | Prompts for 10-digit HTS code or a more detailed product description |
| Insufficient credits | `INSUFFICIENT_CREDITS` | Top up via [Console](https://supplygraph.ai/zk_chat_os/dashboard/dashboard.html) |
| Invalid or out-of-scope input | `INVALID_REQUEST` | Returns guidance in `content` (string) |

Maintainer: info@supplygraph.ai  
License: Proprietary / Internal  
© 2025–2026 SupplyGraph AI. All rights reserved.
