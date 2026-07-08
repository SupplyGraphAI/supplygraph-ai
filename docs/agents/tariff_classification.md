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

`agent_id`: `tariff_classification` · MCP tool: `tariff_classification`

| Field | Required | Description |
|-------|----------|-------------|
| `text` | Yes | Clear, detailed natural-language product description |

Country of origin may be included but is optional. No HTS knowledge is required from the user.

Multi-turn: respond to `WAITING_USER` with the same `task_id`. See [Agent API §8](../agent-api/agent-api.md#8-multi-turn-waiting_user).

## Output Format

On success, results include `classification_results` — an array of suggested HTS codes with `confidence_score`, `reasoning`, and `description`. Designed for direct handoff to the [U.S. Tariff Calculation Agent](./tariff_calc.md).

## Integration

| Method | ID / Tool | Documentation |
|--------|-----------|---------------|
| **A2A** | `tariff_classification` | [a2a.md](../a2a_mcp/a2a.md) |
| **MCP** | `tariff_classification` | [mcp.md](../a2a_mcp/mcp.md) |
| **Agent API** | `tariff_classification` | [agent-api.md](../agent-api/agent-api.md) |

Quick examples: [A2A / MCP](../a2a_mcp/quick_example.md) · [Agent API](../agent-api/quick_example.md)

## Errors

Common codes → [Agent API §10](../agent-api/agent-api.md#10-error--status-codes). This agent may return `WAITING_USER` for clarification.


Maintainer: info@supplygraph.ai  
License: Proprietary / Internal  
© 2025–2026 SupplyGraph AI. All rights reserved.
