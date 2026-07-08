# Procurement & Inventory Risk – Key component Focus

## Overview

The **Procurement & Inventory Risk – Key component Focus** continuously monitors and analyzes upstream product or service risk events to determine whether, how, and to what extent the impact may propagate to a target company.

It transforms fragmented global supply chain signals into **structured, auditable, and continuously updated product-driven risk warnings**, enabling organizations to monitor upstream product exposure in near real-time.


## Pain

Enterprises need continuous visibility into critical upstream products and services — not only to know when key inputs at any tier are impacted by global supply chain events, but to determine whether, how, and to what extent that impact may propagate to their own business.

Without structured product-level risk propagation analysis, organizations often lack:

- Visibility into product-driven downstream risk  
- Clear dependency paths from upstream products or services to the target company  
- Quantified estimates of potential business impact  
- Continuously updated warning signals  
- Actionable recommendations before the risk reaches the enterprise  

Traditional monitoring tools may detect upstream events, but they usually cannot explain how disruptions affecting specific products or services propagate through dependency paths to the target company.


## Breakthrough

The Supply Chain Risk Prediction – Cross-Product Agent extends supply chain monitoring into **product-driven risk propagation**.

Behind the scenes, it:

- Monitors global events affecting upstream products or services  
- Quantifies how an original event impacts a specific product or service  
- Identifies product or service dependency paths between upstream inputs and the target company  
- Propagates the product-level affected state toward the target company  
- Estimates downstream enterprise risk, confidence, arrival timing, and key transmission logic  
- Returns structured warning events and actionable recommendations  

For the user, the workflow remains simple:

1. Provide a target company and an upstream product or service  
2. Confirm the matched entities  
3. Activate monitoring for the product under the target company  
4. Retrieve the latest product-driven risk warnings at any time  

Typical insights include:

- Product-level event impact  
- Risk transmission path  
- Enterprise risk value and change  
- Confidence and confidence change  
- Estimated arrival days  
- Key price or market signals  
- Actionable mitigation recommendations  


## Why SupplyGraph AI

Powered by:

- 100M+ global companies  
- 1M+ key products and components  
- Real-time global signal capture running 24/7  
- Enterprise-product dependency graph intelligence  
- Deep-tier product-level risk quantification engine  

SupplyGraph AI provides a **continuously updated, auditable product-driven supply chain risk intelligence layer** that connects upstream product disruptions to downstream enterprise exposure.

This makes the agent suitable for:

- Supply chain risk teams  
- Procurement and sourcing teams  
- Enterprise risk management teams  
- Investors and analysts  
- Global operating companies with critical upstream product dependencies  


# Agent Behavior Model


## Sandbox Key Support (for Development)

This agent supports **Sandbox API Keys**, enabling developers to test integrations without consuming credits.

When using a Sandbox Key:

- No credits are deducted  
- The agent returns predefined sample product-driven risk warning data  
- Data is static and does not update over time  
- Responses do not trigger live monitoring or live risk computation  
- Only validation and schema checks are executed  

Sandbox Keys are ideal for:

- UI prototyping  
- API integration testing  
- SDK development  
- CI/CD pipelines  

⚠️ **Sandbox data must not be used for production analytics.**

The full A2A workflow (`run → status → result`) behaves the same as production, enabling accurate integration testing.


## Initial Monitoring Setup

When a user initiates a new monitoring request through `/run` without specifying `mode`, or with `mode=run`, the agent starts the product-level monitoring customization process for a specific target company.

The request should provide both:

- Target Company  
- Upstream Product or Service  

The agent then identifies and returns the matched entities for user confirmation.

First-time workflow:

1. User calls `/run` with target company and upstream product or service information  
2. System creates a `task_id`  
3. Agent returns matched entities for confirmation  
4. User confirms the matched entities by replying `Yes`  
5. The product monitoring customization is activated  
6. The one-time customization fee is charged  
7. User retrieves the latest warning data via `mode=result`  

After activation:

- The agent runs continuously  
- Results are updated in real time  
- Each `result` request returns the latest available warning data  
- Status becomes completed after the initial setup is finished  


## Pricing & Billing Model

Pricing is based on **product-level customization under a target company**.

- Adding a Target Company is free  
- Customizing an upstream product or service under a target company is paid  
- The current price is **$4,999 per upstream product per target company**  
- This is a **one-time fee**, not a subscription  

The fee is charged only after the user confirms the matched target company and upstream product or service.

The fee covers:

- Product-level monitoring under the specified target company  
- Continuous event detection  
- Product-driven risk propagation analysis  
- Latest warning data retrieval through the `result` mode  
- Structured and auditable warning outputs  


## Understanding task_id and Monitoring Lifecycle

A `task_id` represents a product monitoring instance under a specific target company.

### How task_id is created and maintained

- `/run` without `task_id` creates a new task  
- After entity confirmation, the task becomes an active monitoring instance  
- The task remains valid for retrieving the latest warning data  
- The system recognizes the monitoring instance by `task_id`  

Implications:

- If a user loses the `task_id` and initiates a new `/run`, the system treats it as a new customization request  
- If the same product needs to be monitored under multiple target companies, each target-product pair is treated independently  
- Each upstream product is priced independently under each target company  


## Status Endpoint Behavior

The `status` mode is used to check whether the task setup has completed.

For this agent:

- Once the initial setup is completed, the task status remains completed  
- Ongoing risk updates should be retrieved through `mode=result`  
- Repeated status polling is usually unnecessary after completion  


## Result Endpoint Behavior

The `result` mode always returns the **latest available warning data** for the activated target-product monitoring instance.

- It may be called repeatedly  
- Each request returns the freshest available data  
- The agent continues running and updating warning events in the background  
- The response may contain one or more active warning events  


## Input Requirements

`agent_id`: `supply_chain_risk_prediction_cross_product` · MCP tool: `supply_chain_risk_prediction_cross_product`

| Field | Required | Description |
|-------|----------|-------------|
| `text` | Yes | Target company and supplier/component information |

### text Format

The `text` field should include both target company and upstream product or service information.

Recommended format:

```text
target company: company name, country/region, address(optional), website(optional); upstream product: product name (or MPN-XXXXX), company name, country/region, address(optional), website(optional)
```

Example:

```text
target company: Tesla, Inc. United States; upstream product: Lithium Carbonate Albemarle Corporation United States
```

### Example Response (WAITING_USER)

```json
{
  "success": false,
  "code": "WAITING_USER",
  "data": {
    "task_id": "<task-id>",
    "agent": "supply_chain_risk_prediction_cross_product",
    "stage": "interpreting",
    "content": "Here are the entities we’ve identified.\nTarget Company Name: Tesla, Inc.\nCountry: United States\nUpstream Product: Lithium Carbonate\nCompany Name: Albemarle Corporation\nCountry: United States\nPlease reply [Yes] or [No] to confirm."
  }
}
```

After the user replies `Yes`, the product monitoring customization is activated and the one-time fee is charged.

Confirm matched entities when prompted (`WAITING_USER`). Uses `mode=result` for latest data.


## Integration

| Method | ID / Tool | Documentation |
|--------|-----------|---------------|
| **A2A** | `supply_chain_risk_prediction_cross_product` | [a2a.md](../a2a_mcp/a2a.md) |
| **MCP** | `supply_chain_risk_prediction_cross_product` | [mcp.md](../a2a_mcp/mcp.md) |
| **Agent API** | `supply_chain_risk_prediction_cross_product` | [agent-api.md](../agent-api/agent-api.md) |

Quick examples: [A2A / MCP](../a2a_mcp/quick_example.md) · [Agent API](../agent-api/quick_example.md)

## Errors

Common codes → [Agent API §10](../agent-api/agent-api.md#10-error--status-codes). Returns `WAITING_USER` for company confirmation. Uses `mode=result`.

Maintainer: [info@supplygraph.ai](mailto:info@supplygraph.ai)
License: Proprietary / Internal
© 2025–2026 SupplyGraph AI. All rights reserved.
