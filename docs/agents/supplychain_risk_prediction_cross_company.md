# Procurement & Inventory Risk – Key Supplier Focus

## Overview

The **Procurement & Inventory Risk – Key Supplier Focus** continuously monitors and analyzes upstream supplier risk events to determine whether, how, and to what extent the impact may propagate to a target company.

It transforms fragmented global supply chain signals into **structured, auditable, and continuously updated company-to-company risk warnings**, enabling organizations to monitor upstream supplier exposure in near real-time.


## Pain

Enterprises need continuous visibility into critical upstream dependencies — not only to know when key suppliers are impacted by global supply chain events, but also to understand whether that impact may reach their own business.

Without structured cross-company risk propagation analysis, organizations often lack:

- Visibility into supplier-driven downstream risk
- Clear dependency paths from supplier to target company
- Quantified estimates of potential business impact
- Continuously updated warning signals
- Actionable recommendations before the risk reaches the enterprise

Traditional monitoring tools may detect supplier events, but they usually cannot explain how supplier impact propagates through product or service dependency paths to the target company.


## Breakthrough

The Supply Chain Risk Prediction – Cross-Company Agent extends supplier monitoring into **quantified company-to-company risk propagation**.

Behind the scenes, it:

- Monitors global events affecting upstream supplier companies
- Quantifies how an original event impacts the supplier company
- Identifies product or service dependency paths between supplier and target company
- Propagates the supplier’s affected state toward the target company
- Estimates downstream enterprise risk, confidence, arrival timing, and key transmission logic
- Returns structured warning events and actionable recommendations

For the user, the workflow remains simple:

1. Provide a target company and a supplier company
2. Confirm the matched entities
3. Activate monitoring for the supplier under the target company
4. Retrieve the latest cross-company risk warnings at any time

Typical insights include:

- Supplier-side event impact
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
- Deep-tier risk quantification engine

SupplyGraph AI provides a **continuously updated, auditable cross-company supply chain risk intelligence layer** that connects upstream supplier events to downstream enterprise exposure.

This makes the agent suitable for:

- Supply chain risk teams
- Procurement and sourcing teams
- Enterprise risk management teams
- Investors and analysts
- Global operating companies with critical upstream dependencies


# Agent Behavior Model

## Sandbox Key Support (for Development)

This agent supports **Sandbox API Keys**, enabling developers to test integrations without consuming credits.

When using a Sandbox Key:

- No credits are deducted
- The agent returns predefined sample supplier-to-target risk warning data
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

When a user initiates a new monitoring request through `/run` without specifying `mode`, or with `mode=run`, the agent starts the supplier customization process for a specific target company.

The request should provide both:

- Target Company
- Supplier Company

The agent then identifies and returns the matched companies for user confirmation.

First-time workflow:

1. User calls `/run` with target company and supplier company information
2. System creates a `task_id`
3. Agent returns matched target and supplier company candidates
4. User confirms the matched companies by replying `Yes`
5. The supplier monitoring customization is activated
6. The one-time supplier customization fee is charged
7. User retrieves the latest warning data via `mode=result`

After activation:

- The agent runs continuously
- Results are updated in real time
- Each `result` request returns the latest available warning data
- Status becomes completed after the initial setup is finished


## Pricing & Billing Model

Pricing is based on **supplier-level customization under a target company**.

- Adding a Target Company is free
- Customizing a Supplier Company under a Target Company is paid
- The current price is **$4,999 per supplier company per target company**
- This is a **one-time fee**, not a subscription

The fee is charged only after the user confirms the matched target company and supplier company.

The fee covers:

- Supplier monitoring under the specified target company
- Continuous event detection
- Cross-company risk propagation analysis
- Latest warning data retrieval through the `result` mode
- Structured and auditable warning outputs


## Understanding task_id and Monitoring Lifecycle

A `task_id` represents a supplier monitoring instance under a specific target company.

### How task_id is created and maintained

- `/run` without `task_id` creates a new task
- After company confirmation, the task becomes an active monitoring instance
- The task remains valid for retrieving the latest warning data
- The system recognizes the monitoring instance by `task_id`

Implications:

- If a user loses the `task_id` and initiates a new `/run`, the system treats it as a new customization request
- If the same supplier needs to be monitored under multiple target companies, each target-supplier pair is treated independently
- Each supplier company is priced independently under each target company


## Status Endpoint Behavior

The `status` mode is used to check whether the task setup has completed.

For this agent:

- Once the initial setup is completed, the task status remains completed
- Ongoing risk updates should be retrieved through `mode=result`
- Repeated status polling is usually unnecessary after completion


## Result Endpoint Behavior

The `result` mode always returns the **latest available warning data** for the activated target-supplier monitoring instance.

- It may be called repeatedly
- Each request returns the freshest available data
- The agent continues running and updating warning events in the background
- The response may contain one or more active warning events


# API Overview

This section provides an overview of the **A2A (Agent-to-Agent)** interface used to integrate this agent into other systems.


## Endpoints Summary

| Endpoint | Method | Description |
|---------|--------|-------------|
| `/api/v1/agents/supply_chain_risk_prediction_cross_company/manifest` | GET | Retrieve metadata, schema, pricing, and version info |
| `/api/v1/agents/supply_chain_risk_prediction_cross_company/run` | POST | Execute or manage tasks via `mode` |

Supported modes:

- `run` — start a new supplier monitoring customization task
- `status` — check task status
- `result` — retrieve the latest warning data


## Manifest

### Request

```bash
curl -X GET https://agent.supplygraph.ai/api/v1/agents/supply_chain_risk_prediction_cross_company/manifest \
  -H "Authorization: Bearer <YOUR_API_KEY>"
```

### Example Response

```json
{
  "agent_id": "supply_chain_risk_prediction_cross_company",
  "name": "Supply Chain Risk Prediction – Cross-Company Agent",
  "version": "1.0.0",
  "description": "Monitors supplier events and quantifies cross-company supply chain risk propagation to a target company",
  "input_schema": { ... },
  "output_schema": { ... },
  "pricing": {
    "unit": "USD",
    "one_time_fee_per_supplier_per_target": 4999
  },
  "status": "active"
}
```

## Run Endpoint

### Purpose

Start a new supplier monitoring customization task for a target company.

If `mode` is omitted, the request is treated as `mode=run`.

### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/supply_chain_risk_prediction_cross_company/run \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
        "text": "target company: Tesla, Inc. United States; supplier: Albemarle Corporation United States",
        "stream": false
      }'
```

### Request Body

| Field    | Type    | Required | Description                                     |
| -------- | ------- | -------- | ----------------------------------------------- |
| `mode`   | string  | No       | Execution mode. Defaults to `run`               |
| `text`   | string  | Yes      | Target company and supplier company information |
| `stream` | boolean | No       | Whether to enable streaming response            |

### text Format

The `text` field should include both target company and supplier company information.

Recommended format:

```text
target company: company name, country/region, address(optional), website(optional); supplier: company name, country/region, address(optional), website(optional)
```

Example:

```text
target company: Tesla, Inc. United States; supplier: Albemarle Corporation United States
```

### Example Response (WAITING_USER)

```json
{
  "success": false,
  "code": "WAITING_USER",
  "data": {
    "task_id": "<task-id>",
    "agent": "supply_chain_risk_prediction_cross_company",
    "stage": "interpreting",
    "content": "Here are the companies we’ve identified.\nTarget Company Name: Tesla, Inc.\nCountry: United States\nSupplier Company Name: Albemarle Corporation\nCountry: United States\nPlease reply [Yes] or [No] to confirm."
  }
}
```

After the user replies `Yes`, the supplier customization is activated and the one-time fee is charged.

## Status Endpoint

### Purpose

Check whether the monitoring customization task has completed.

For this agent, once the initial setup is completed, status will remain completed.

### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/supply_chain_risk_prediction_cross_company/run \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
        "mode": "status",
        "task_id": "<task-id>"
      }'
```

### Example Response

```json
{
  "success": true,
  "code": "TASK_COMPLETED",
  "data": {
    "task_id": "<task-id>",
    "agent": "supply_chain_risk_prediction_cross_company",
    "stage": "completed",
    "progress": 100
  }
}
```

## Result Endpoint

### Purpose

Retrieve the latest warning data for the target-supplier monitoring instance.

The agent runs continuously, and each `result` request returns the latest available data.

### Request

```bash
curl -X POST https://agent.supplygraph.ai/api/v1/agents/supply_chain_risk_prediction_cross_company/run \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
        "mode": "result",
        "task_id": "<task-id>"
      }'
```

### Example Response

```json
{
  "success": true,
  "code": "TASK_COMPLETED",
  "data": [
    {
      "event_id": "EVT_LITHIUM_20260418_001",
      "event_title": "Chile Lithium Supply Tightening Event",
      "event_type": "resource_supply",
      "event_status": "active",
      "warning_summary": "Upstream lithium supply is tightening, prices have risen, and the risk is expected to reach the enterprise within 6 days. It is recommended to lock in prices and increase inventory. Compared with the previous warning, the risk has increased and confidence has strengthened.",
      "warning_time": "2026-04-18",
      "warning_round": 3,
      "enterprise_risk": {
        "risk_value": 87,
        "risk_change": 12,
        "confidence": 82,
        "confidence_change": 9,
        "risk_position": "Risk has propagated to lithium salt processing → cathode materials, approaching the enterprise node",
        "estimated_arrival_days": 6
      },
      "key_prices": [
        {
          "product_name": "Battery-grade lithium carbonate",
          "price": 155000,
          "price_unit": "CNY/ton",
          "price_change": 8.4,
          "price_change_description": "Increased by 8.4% compared with the previous round",
          "previous_warning_time": "2026-04-11"
        }
      ],
      "reasoning": {
        "event_change_and_source": {
          "timeline": [
            "Late March 2026: Water resource disputes emerged in Chile’s Atacama Salt Flat, increasing expectations of stricter regulation",
            "Early April 2026: National lithium resource strategy advanced, regulatory policies began to be implemented",
            "Mid-April 2026: Enterprise expansion restricted, projects slowed, supply expectations tightened"
          ],
          "current_judgement": "The event has moved from the policy expectation stage to the stage of actual supply impact, and risk has significantly increased.",
          "risk_source": "Overseas lithium resource side, Chile, supply constraints"
        },
        "transmission_path": {
          "dominant_path": "Overseas lithium ore → lithium carbonate → cathode materials → battery manufacturing",
          "current_status": "Risk has propagated from the resource side to lithium salt processing and cathode materials, rapidly approaching the enterprise."
        },
        "key_node_impacts": [
          {
            "node_name": "Lithium Carbonate",
            "impacts": [
              "Price increased by approximately 8.4%",
              "Upstream quotations tightening, reduced bargaining space",
              "Supply expectations shifted from loose to tight"
            ]
          },
          {
            "node_name": "Cathode Materials",
            "impacts": [
              "Cost pressure beginning to transmit",
              "Some enterprises report shortened procurement cycles"
            ]
          }
        ],
        "data_and_market_signals": {
          "price_signal": "Battery-grade lithium carbonate spot price is around 155,000 CNY/ton, showing continuous increases.",
          "supply_signal": "Project approvals slowing, expansion restricted, supply expectations tightening.",
          "market_signal": "Multiple institutions have raised price expectations, and downstream feedback indicates tighter quotations.",
          "multi_source_consistency": "Policy, enterprise, and market price signals are consistent, increasing confidence."
        },
        "enterprise_impact": {
          "current_status": "Has not yet fully impacted enterprise procurement costs.",
          "estimated_arrival": "Expected to impact the enterprise within approximately 6 days.",
          "impact_type": "Primarily cost increase risk, accompanied by rising supply uncertainty."
        }
      },
      "actionable_recommendations": [
        {
          "recommendation_id": 1,
          "action": "While locking in lithium carbonate procurement prices for the next 1–2 weeks, moderately increase safety stock to cover 7–10 days of production demand.",
          "reason": "The risk has moved from policy expectation to actual supply impact. Prices are rising and supply uncertainty exists, requiring hedging against both price and supply risks.",
          "data_support": [
            "Risk value 87 (high level)",
            "Risk change +12 (increasing)",
            "Expected impact in ~6 days",
            "Lithium carbonate price increased by 8.4%",
            "A 10% price increase may compress profit margins by ~6%"
          ]
        }
      ]
    }
  ],
  "metadata": {
    "agent": "supply_chain_risk_prediction_cross_company",
    "timestamp": "2026-04-18T09:00:00Z"
  },
  "errors": null
}
```

## Make Your First A2A Call

Typical workflow:

1. Start with `mode=run`
2. Provide target company and supplier company information
3. Confirm matched companies if prompted
4. Retrieve latest warning data with `mode=result`
5. Use `mode=status` only when checking setup completion

## Integration Options

| Protocol | Description                     | Docs                      |
| -------- | ------------------------------- | ------------------------- |
| A2A      | Autonomous workflow integration | [A2A Protocol](../a2a.md) |
| MCP      | Multi-channel orchestration     | *(Coming Soon)*           |

Developer Interfaces:

| Interface  | Description         | Docs                                                                                                             |
| ---------- | ------------------- | ---------------------------------------------------------------------------------------------------------------- |
| Python SDK | Official A2A Client | [https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk](https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk) |

## Error Handling & Rate Limits

### Common Error Codes

| Code                 | Description                                     |
| -------------------- | ----------------------------------------------- |
| UNAUTHORIZED         | Invalid or missing API key                      |
| INSUFFICIENT_CREDITS | Not enough balance                              |
| RATE_LIMITED         | Too many requests                               |
| INVALID_REQUEST      | Input outside agent scope                       |
| WAITING_USER         | User confirmation is required before activation |

### Stage-Specific Codes

```text
interpreting:
  INTERPRETING
  INVALID_REQUEST
  UNAUTHORIZED
  WAITING_USER

executing:
  TASK_ACCEPTED
  TASK_RUNNING

completed:
  TASK_COMPLETED
  TASK_FAILED

cancelled:
  TASK_CANCELLED
```

Maintainer: [info@supplygraph.ai](mailto:info@supplygraph.ai)
License: Proprietary / Internal
© 2025 SupplyGraph AI. All rights reserved.
