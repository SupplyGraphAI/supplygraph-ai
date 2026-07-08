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

`agent_id`: `tariff_calc` · MCP tool: `tariff_calc`

| Field | Required | Description |
|-------|----------|-------------|
| `text` | Yes | Product description **or** 10-digit HTS code, plus **country of origin** |

Optional in text: weight, quantity, declared value. Multi-turn via `WAITING_USER` + same `task_id` → [Agent API §8](../agent-api/agent-api.md#8-multi-turn-waiting_user).

**Example:** "Calculate import duties for 5601.21.0010, country of origin China, shipment value 200 USD, 50 kg."


## Integration

| Method | ID / Tool | Documentation |
|--------|-----------|---------------|
| **A2A** | `tariff_calc` | [a2a.md](../a2a_mcp/a2a.md) |
| **MCP** | `tariff_calc` | [mcp.md](../a2a_mcp/mcp.md) |
| **Agent API** | `tariff_calc` | [agent-api.md](../agent-api/agent-api.md) |

Quick examples: [A2A / MCP](../a2a_mcp/quick_example.md) · [Agent API](../agent-api/quick_example.md)

## Errors

Common codes → [Agent API §10](../agent-api/agent-api.md#10-error--status-codes). This agent may return `WAITING_USER` when input is incomplete.

Maintainer: info@supplygraph.ai  
License: Proprietary / Internal  
© 2025–2026 SupplyGraph AI. All rights reserved.
