# Customs Classification Agent


## Overview

The **Customs Classification Agent** automatically maps product descriptions to the **most accurate HS / HTS codes** in seconds.

It eliminates manual lookup, reduces classification errors, and enables downstream **duty calculation, compliance analysis, and tariff optimization** at scale.


## Pain

For customs specialists and trade compliance teams, determining the correct HTS code is a critical but extremely demanding task:

- Manually comparing product descriptions to complex classification rules
- Navigating thousands of pages in tariff schedules
- Interpreting vague or ambiguous product language
- Handling cases where one product may match **multiple potential codes**

This process is:

- Time-consuming
- Mentally exhausting
- Highly error-prone
- Inconsistent across teams

A single incorrect classification can result in **penalties, shipment delays, revenue loss, or compliance risk**.


## Breakthrough

The Customs Classification Agent reduces manual work from **hours (or days) to seconds**.

By leveraging:

- Real-time global tariff databases
- Domain-specific classification knowledge
- Graph-based reasoning over product attributes

SupplyGraph AI delivers:

- Multiple candidate HTS codes
- Confidence scores for each option
- Traceable reasoning paths
- Evidence-backed decision support

This not only improves speed, but also **unlocks tariff optimization opportunities** by making alternative classifications transparent and comparable.


## Why SupplyGraph AI

SupplyGraph AI combines:

- Official HTS / HS source data
- Graph intelligence across products and industries
- Structured customs reasoning models
- Continuous regulatory updates

Every classification is:

- Explainable
- Auditable
- Consistent
- Enterprise-grade

This dramatically reduces compliance risk while creating a reliable foundation for tariff calculation, sourcing optimization, and trade strategy planning.


## Try the Customs Classification Agent (Live Chatbot)

Before integrating via API, you can experience this agent instantly through our interactive classification chatbot.

This live demo allows you to:

- Enter a product description in natural language
- Receive the most relevant HS / HTS code candidates
- Understand why each code is suggested
- Explore alternative classifications
- Reduce ambiguity and manual lookup work

Launch the Customs Classification Chatbot  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=tariff_classification

To use the chatbot, you’ll first need to:

- Create a SupplyGraph AI account
- Top up your credit balance

This chatbot is powered by the **same Customs Classification Agent and A2A endpoints** described below.  
Credits used in the chatbot are deducted in the same way as API / A2A usage.

Everything you experience here can be directly embedded into your own systems.

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

`agent_id`: `tariff_classification` · MCP tool: `tariff_classification` · Pricing: **2 credits / run**

| Field | Required | Description |
|-------|----------|-------------|
| `text` | Yes | Natural-language description of the product or item to classify. If country-of-origin information is included, the agent extracts it automatically. |

No HTS knowledge is required from the user. Country of origin is **optional** — when present, it is returned in the structured output for downstream use.

**Example:** `"Cotton T-shirts for women, 100% cotton, made in Mexico"`

Full machine-readable `input_schema` / `output_schema` → `GET /api/v1/agents/tariff_classification/manifest` (see [Agent API §3](../agent-api/agent-api.md#3-manifest)).


## Output Format

Primary output lives in `data.content` (Agent API) or task artifacts (A2A / MCP). The payload is a **oneOf**:

| State | `content` type | Description |
|-------|----------------|-------------|
| In progress, failed, cancelled, or invalid input | `string` | Text or Markdown — validation prompts, error messages, or out-of-scope notices |
| **Task completed successfully** | `object` | Structured HTS classification result (see below) |

**Structured success object:**

```json
{
  "type": "result",
  "data": {
    "country_of_origin": "Mexico",
    "classification_results": [
      {
        "hts_code": "6109.10.00.40",
        "confidence_score": 0.95,
        "description": "Official HTS category description",
        "reasoning": "AI-explained rationale for this classification"
      }
    ]
  }
}
```

| Field | Description |
|-------|-------------|
| `type` | Always `"result"` when classification completed successfully |
| `data.country_of_origin` | Country of origin extracted from input; `null` if not provided or cannot be determined |
| `data.classification_results` | Array of candidate HTS codes, ordered by relevance |
| `classification_results[].hts_code` | 10-digit Harmonized Tariff Schedule code |
| `classification_results[].confidence_score` | Confidence score between `0` and `1` |
| `classification_results[].description` | Official HTS category description |
| `classification_results[].reasoning` | AI-explained rationale for the classification choice |

Estimated task duration: **10–30 seconds** (Production). Sandbox returns instantly.

This agent is **single-turn** — each request is independent. Pass the top `hts_code` and `country_of_origin` to the [U.S. Tariff Calculation Agent](./tariff_calc.md) for duty analysis.


## Sample Response (Sandbox)

> Sandbox returns deterministic mock data with the **same structure** as Production. The example below uses the standard Sandbox fixture (abbreviated to two candidates).

**Success (`TASK_COMPLETED`) — Agent API `results`:**

```json
{
  "success": true,
  "code": "TASK_COMPLETED",
  "message": "Task completed successfully.",
  "data": {
    "task_id": "<task-id>",
    "agent": "tariff_classification",
    "stage": "completed",
    "progress": 100,
    "content": {
      "type": "result",
      "data": {
        "country_of_origin": "Mexico",
        "classification_results": [
          {
            "hts_code": "6109.10.00.40",
            "confidence_score": 0.95,
            "reasoning": "The product is a women's T-shirt made of 100% cotton, knitted or crocheted, which directly matches this HTS code description.",
            "description": "T-shirts, singlets, tank tops and similar garments, knitted or crocheted > Of cotton > Women's or girls' > Other > T-shirts > Women's (339)"
          },
          {
            "hts_code": "6109.10.00.70",
            "confidence_score": 0.85,
            "reasoning": "This code covers T-shirts and similar garments that are knitted or crocheted, made of cotton, for women or girls, specifically under the category 'Other'.",
            "description": "T-shirts, singlets, tank tops and similar garments, knitted or crocheted > Of cotton > Women's or girls' > Other > Other (339)"
          }
        ]
      }
    }
  },
  "metadata": { "credits_used": 0 },
  "errors": null
}
```

The full Sandbox fixture returns five candidate codes for the same input.


## Integration

| Method | ID / Tool | Documentation |
|--------|-----------|---------------|
| **A2A** | `tariff_classification` | [a2a.md](../a2a_mcp/a2a.md) |
| **MCP** | `tariff_classification` | [mcp.md](../a2a_mcp/mcp.md) |
| **Agent API** | `tariff_classification` | [agent-api.md](../agent-api/agent-api.md) |

Call pattern is identical across agents — only `agent_id` / tool name and input `text` differ.

Quick examples: [A2A / MCP](../a2a_mcp/quick_example.md) · [Agent API](../agent-api/quick_example.md)


## Errors

Common codes → [Agent API §10](../agent-api/agent-api.md#10-error--status-codes).

| Situation | Code | Agent behavior |
|-----------|------|----------------|
| Vague or unclassifiable product description | `TASK_FAILED` | Returns guidance in `content` (string) — provide a more detailed product description in a **new** request |
| Input outside agent scope (chitchat) | `INVALID_REQUEST` | Returns guidance in `content` (string) |
| Insufficient credits | `INSUFFICIENT_CREDITS` | Top up via [Console](https://supplygraph.ai/zk_chat_os/dashboard/dashboard.html) |

This agent does **not** support multi-turn clarification. Each `run` is a standalone classification task.

Maintainer: info@supplygraph.ai  
License: Proprietary / Internal  
© 2025–2026 SupplyGraph AI. All rights reserved.
