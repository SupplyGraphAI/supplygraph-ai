# Supply Chain Risk Prediction Agent

## Overview

The **Supply Chain Risk Prediction Agent** continuously monitors global supply chain risk events and evaluates whether, how, and to what extent those events may affect a target company.

It transforms fragmented global signals into **structured, auditable, and continuously updated enterprise risk warnings**, enabling users to understand event relevance, propagation logic, risk severity, impacted nodes, estimated timing, and recommended actions.

The agent supports three user roles:

* `ceo` — Corporate Executives
* `hedgefund` — Hedge Fund Managers
* `riskexpert` — Supply Chain Risk Professionals

Each role receives the same underlying risk intelligence, but the interpretation, emphasis, and presentation style are adapted to the user’s decision context.

## Pain

Organizations face an overwhelming volume of global supply chain signals every day. Most incidents are irrelevant to a given company, while the few that matter are often buried inside news, technical reports, supplier updates, government announcements, market signals, and fragmented operational information.

For corporate executives, the core challenge is quickly understanding whether a disruption affects the enterprise, which business areas are exposed, and how severe the operational impact may be.

For hedge fund managers, the challenge is determining whether global events create meaningful company-level impacts that may affect revenue, cost structure, operations, market expectations, or investment decisions.

For supply chain risk professionals, the challenge is continuously tracing how disruptions propagate across suppliers, products, materials, logistics routes, and multi-tier dependency paths.

Without structured risk propagation analysis, users often lack:

* Clear visibility into whether an event is genuinely relevant to the target company
* Multi-tier dependency paths explaining how the impact may reach the enterprise
* Quantified estimates of direct and indirect impacted nodes
* Business-level interpretation of risk status and severity
* Continuously updated warning signals
* Actionable recommendations before the risk fully reaches the enterprise

Traditional monitoring tools may detect global events, but they usually cannot explain how those events propagate through complex supply chain networks to create enterprise-level exposure.

## Breakthrough

The Supply Chain Risk Prediction Agent extends event monitoring into **company-specific, multi-tier supply chain risk propagation analysis**.

Behind the scenes, it:

* Monitors global supply chain risk events in real time
* Filters millions of daily incidents to surface only genuinely relevant enterprise signals
* Identifies direct and indirect impacted supply chain nodes
* Traces multi-tier propagation paths between the event and the target company
* Quantifies enterprise-level risk status, severity, confidence, timing, and transmission logic
* Visualizes event distribution, trends, impacted nodes, and propagation paths
* Produces structured, auditable warning outputs
* Delivers proactive email notifications when a meaningful signal requires attention or action

For the user, the workflow remains simple:

1. Provide a target company and event-related context
2. Confirm the matched company and task setup
3. Activate supply chain risk monitoring and analysis
4. Retrieve the latest risk assessment through the result endpoint
5. Optionally specify a role to receive role-adapted interpretation

Typical insights include:

* Enterprise risk status: high, low, or none
* Direct and indirect impacted nodes
* Multi-tier risk transmission paths
* Key path and key node explanations
* Estimated impact start and end dates
* Risk summary and final interpretation
* Event distribution and trend visualization
* Actionable mitigation recommendations

## Why SupplyGraph AI

Powered by:

* 100M+ global companies
* 1M+ key products and components
* Real-time global signal capture running 24/7
* Enterprise-product dependency graph intelligence
* Multi-hop dependency tracing and modeling up to 10 tiers
* Fully auditable propagation pathways showing exactly how risks move
* Deep-tier risk quantification engine
* Proactive email notifications when a genuinely relevant signal occurs

SupplyGraph AI provides a **continuously updated, auditable enterprise supply chain risk intelligence layer** that connects global risk events to company-specific exposure.

This makes the agent suitable for:

* Corporate executives
* Hedge fund managers
* Supply chain risk professionals
* Procurement and sourcing teams
* Enterprise risk management teams
* Investors and analysts
* Global operating companies with critical upstream dependencies

Open the Supply Chain Risk Prediction Chatbot for Corporate Executives
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=risk_propagation_detail_ceo

Open the Supply Chain Risk Prediction Chatbot for Hedge Fund Managers
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=risk_propagation_detail_hedgefund

Open the Supply Chain Risk Prediction Chatbot for Supply Chain Risk Professionals
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=risk_propagation_detail_riskexpert

To use the chatbot or API, you’ll first need to:

* Create a SupplyGraph AI account
* Top up your credit balance

These chatbots are powered by the **same Supply Chain Risk Prediction Agent and A2A endpoints** described in this documentation.
Credits used in the chatbot are deducted in the same way as API / A2A usage.

Everything you experience here can be fully embedded into your own system through A2A integration.

# Agent Behavior Model

## Sandbox Key Support (for Development)

This agent supports **Sandbox API Keys**, enabling developers to test integrations without consuming credits.

When using a Sandbox Key:

* No credits are deducted
* The agent returns predefined sample enterprise risk assessment data
* Data is static and does not update over time
* Responses do not trigger live monitoring or live risk computation
* Only validation and schema checks are executed

Sandbox Keys are ideal for:

* UI prototyping
* API integration testing
* SDK development
* CI/CD pipelines

⚠️ **Sandbox data must not be used for production analytics.**

The full A2A workflow (`run → status → result`) behaves the same as production, enabling accurate integration testing.

## Initial Risk Assessment Setup

When a user initiates a new risk assessment request through `/run` without specifying `mode`, or with `mode=run`, the agent starts the company-level supply chain risk assessment process.

The request should provide the target company and the event-related context required for the assessment.

The agent then identifies the matched company and prepares the risk propagation task for user confirmation.

First-time workflow:

1. User calls `/run` with target company and event-related information
2. System creates a `task_id`
3. Agent returns matched company candidates and task setup information
4. User confirms the matched company and task setup by replying `Yes`
5. The risk assessment task is activated
6. The applicable fee is charged
7. User retrieves the latest assessment result via `mode=result`

After activation:

* The agent runs the company-level supply chain risk assessment
* Results are updated as new information becomes available
* Each `result` request returns the latest available assessment data
* Status becomes completed after the initial setup and assessment process is finished

## Analysis Mode

The agent supports two assessment modes through the `analysis_mode` parameter:

* `normal` — Standard live supply chain risk assessment mode
* `backtest` — Historical risk case evaluation mode

### Normal Mode

In `normal` mode, the agent performs a standard company-level supply chain risk assessment based on the provided target company and event-related context.

This mode is intended for live or current risk monitoring, enterprise warning generation, and continuously updated result retrieval.

Normal mode follows the standard pricing and billing model described below.

### Backtest Mode

In `backtest` mode, the agent evaluates historical or predefined risk cases for validation, benchmarking, research, model testing, or retrospective analysis.

Backtest mode is suitable for:

* Testing historical risk propagation scenarios
* Validating whether past supply chain events would have affected a target company
* Comparing historical risk signals with known outcomes
* Demonstrating risk propagation logic using prior cases
* Internal evaluation, benchmarking, and model validation

Backtest mode uses the same `run → status → result` workflow as normal mode.

### Backtest Pricing

Backtest mode is priced at **30% of the standard assessment fee**.

The first **10 risk cases** in backtest mode are provided free of charge.

After the first 10 free backtest risk cases are used, additional backtest assessments are charged at the discounted backtest rate.

Backtest mode is intended for historical analysis and validation. It should not be used as a substitute for live enterprise risk monitoring when current or continuously updated risk intelligence is required.

## Pricing & Billing Model

Pricing is based on the activated supply chain risk assessment task and the selected `analysis_mode`.

* Company matching and task preparation may be performed before confirmation
* The assessment fee is charged only after the user confirms the matched company and task setup
* Each activated task is associated with a `task_id`
* Result retrieval uses the same activated task
* In `normal` mode, the current price is **$49,999 per event per target company**
* In `backtest` mode, the assessment is charged at **30% of the normal mode price**
* The first **10 backtest risk cases** are free of charge
* This is a **one-time fee**, not a subscription

The fee covers:

* Company-level event impact assessment
* Supply chain node filtering and evaluation
* Multi-tier propagation path analysis
* Enterprise-level risk synthesis
* Latest assessment result retrieval through the `result` mode
* Structured and auditable warning outputs

## Understanding task_id and Assessment Lifecycle

A `task_id` represents a company-level supply chain risk assessment task.

### How task_id is created and maintained

* `/run` without `task_id` creates a new task
* After user confirmation, the task becomes an active assessment instance
* The task remains valid for retrieving the latest available assessment result
* The system recognizes the assessment instance by `task_id`

Implications:

* If a user loses the `task_id` and initiates a new `/run`, the system treats it as a new assessment request
* Different target companies or event contexts are treated as independent tasks
* Each task is priced and processed independently

## Status Endpoint Behavior

The `status` mode is used to check whether the task setup and assessment process has completed.

For this agent:

* Once the initial setup and assessment process is completed, the task status remains completed
* Ongoing or latest risk assessment data should be retrieved through `mode=result`
* Repeated status polling is usually unnecessary after completion

## Result Endpoint Behavior

The `result` mode always returns the **latest available supply chain risk assessment data** for the activated task.

It may be called repeatedly using the corresponding `task_id`.

When calling `mode=result`, users may provide an optional `role` parameter to receive role-adapted interpretation.

Supported `role` values:

* `ceo` — Returns an executive-oriented interpretation focused on enterprise exposure, business impact, severity, timing, and recommended management actions.
* `hedgefund` — Returns an investor-oriented interpretation focused on company-level operational impact, risk transmission logic, potential financial relevance, market signals, and investment-facing implications.
* `riskexpert` — Returns a professional supply chain risk interpretation focused on impacted nodes, propagation paths, key paths, key nodes, risk mechanisms, and mitigation actions.

If `role` is not provided, the agent returns the default enterprise risk assessment result without any role-specific interpretation.

The `result` response include:

* Event information
* Target company information
* Task-level assessment status and progress
* Enterprise-level risk summary
* Final impact interpretation
* Impact dates
* Key paths and key nodes
* Node-level assessment details
* Path-level risk interpretation
* Impact graph data for visualization
* Analysis dossier text and structured dossier paragraphs

## Input Requirements

`agent_id`: `supply_chain_risk_prediction` · MCP tool: `supply_chain_risk_prediction`

Serialize `event_info` as JSON and pass via the `text` field: `text = json.dumps({"event_info": {...}})`.

Optional on `mode=result`: `role` = `ceo` | `hedgefund` | `riskexpert` for role-adapted interpretation.

## Event Info Schema

The `event_info` object is required.

The `event_type` field is required and must be one of:

```text
news, policy, commodity_price
```

The `analysis_mode` field is required and must be one of:

```text
normal, backtest
```

### Common Required Fields

The following fields are required for all event types:

| Field           | Type   | Required | Format / Description                                                 |
| --------------- | ------ | -------: | -------------------------------------------------------------------- |
| `event_type`    | string |      Yes | Must be one of `news`, `policy`, or `commodity_price`                |
| `analysis_mode` | string |      Yes | Must be one of `normal` or `backtest`                                |
| `company_name`  | string |      Yes | Target company name to be assessed. Provide the most specific legal or commonly recognized company name. To improve matching accuracy, include country, region, ticker, exchange, or other identifying information when available, for example: `Tesla, Inc. (United States, NASDAQ: TSLA)` or `BYD Company Limited (China, HKEX: 1211)`. |

---

## Event Type: `news` or `policy`

When `event_type` is `news` or `policy`, the input should use the following schema.

### Required Fields

| Field           | Type   | Required | Format / Description                                                       |
| --------------- | ------ | -------: | -------------------------------------------------------------------------- |
| `event_type`    | string |      Yes | Must be `news` or `policy`                                                 |
| `analysis_mode` | string |      Yes | Must be `normal` or `backtest`                                             |
| `title`         | string |      Yes | Title of the event                                                         |
| `content`       | string |      Yes | Full event content or detailed description                                 |
| `pub_date`      | string |      Yes | Publication time. Must be RFC 3339 date-time with required timezone offset |
| `company_name`  | string |      Yes | Target company name to be assessed. Provide the most specific legal or commonly recognized company name. To improve matching accuracy, include country, region, ticker, exchange, or other identifying information when available, for example: `Tesla, Inc. (United States, NASDAQ: TSLA)` or `BYD Company Limited (China, HKEX: 1211)`.      |

### `pub_date` Format Requirement

`pub_date` must be an RFC 3339 date-time string with a timezone offset.

Valid examples:

```text
2026-03-17T00:00:00-04:00
2026-03-17T13:30:00+08:00
2026-03-17T17:30:00Z
```

---

### Request Example: News / Policy Event

#### Python Input

```python
import json
import requests

YOUR_API_KEY = "YOUR_API_KEY"

url = "https://agent.supplygraph.ai/api/v1/agents/supply_chain_risk_prediction/run"

event_info = {
    "event_type": "news",
    "analysis_mode": "normal",
    "title": "U.S. Government Confirms Tesla and LG Energy Solution's $4.3 Billion LFP Battery Deal",
    "content": (
        "In March 2026, the U.S. government confirmed a $4.3 billion agreement between Tesla "
        "and South Korea's LG Energy Solution to support lithium iron phosphate (LFP) battery "
        "cell production in Lansing, Michigan. The facility is expected to begin production in "
        "2027 and supply batteries for Tesla's Megapack 3 energy storage systems manufactured "
        "near Houston. The deal could strengthen Tesla's domestic battery supply chain, reduce "
        "its reliance on Chinese battery imports amid tariff pressure, and support growth in its "
        "energy storage business. However, the financial impact will depend on execution at the "
        "Michigan plant, battery cost competitiveness, demand for Megapack systems, and whether "
        "U.S.-based LFP production can scale efficiently."
    ),
    "pub_date": "2026-03-17T00:00:00-04:00",
    "company_name": "Tesla, Inc."
}

payload = {
    "mode": "run",
    "text": json.dumps({
        "event_info": event_info
    }),
    "stream": False
}

headers = {
    "Authorization": f"Bearer {YOUR_API_KEY}",
    "Content-Type": "application/json"
}

response = requests.post(
    url,
    headers=headers,
    json=payload,
    timeout=60
)
```

---

## Event Type: `commodity_price`

When `event_type` is `commodity_price`, the input should use the following schema.

### Required Fields

| Field                    | Type   | Required | Format / Description                                                            |
| ------------------------ | ------ | -------: | ------------------------------------------------------------------------------- |
| `event_type`             | string |      Yes | Must be `commodity_price`                                                       |
| `analysis_mode`          | string |      Yes | Must be `normal` or `backtest`                                                  |
| `company_name`           | string |      Yes | Target company name to be assessed. Provide the most specific legal or commonly recognized company name. To improve matching accuracy, include country, region, ticker, exchange, or other identifying information when available, for example: `Tesla, Inc. (United States, NASDAQ: TSLA)` or `BYD Company Limited (China, HKEX: 1211)`. |
| `commodity_name`         | string |      Yes | Name of the commodity                                                           |
| `commodity_unit`         | string |      Yes | Unit of the commodity price                                                     |
| `target_date`            | string |      Yes | Target analysis date. Format: `YYYY-MM-DD`                                      |
| `target_price`           | number |      Yes | Commodity price on `target_date`                                                |
| `commodity_price_series` | array  |      Yes | Historical commodity price time series used for analysis                        |

### `commodity_price_series` Requirements

`commodity_price_series` is required and must contain historical trading data for the commodity.

Each item in the array must include:

| Field   | Type   | Required | Format / Description               |
| ------- | ------ | -------: | ---------------------------------- |
| `date`  | string |      Yes | Trading date. Format: `YYYY-MM-DD` |
| `value` | number |      Yes | Commodity price on that date       |

The analysis requires at least 90 calendar days of historical trading data.

The maximum date in `commodity_price_series` should be equal to `target_date`.

Clients should ensure that the time series is complete enough for analysis and that the latest available data point in the series corresponds to the `target_date`.

Example:

```json
{
  "date": "2026-06-12",
  "value": 88.71
}
```

---

### Request Example: Commodity Price Event

#### Python Input

```python
import json
import requests

YOUR_API_KEY = "YOUR_API_KEY"

url = "https://agent.supplygraph.ai/api/v1/agents/supply_chain_risk_prediction/run"

event_info = {
    "event_type": "commodity_price",
    "analysis_mode": "normal",
    "company_name": "Tesla, Inc.",
    "commodity_name": "Crude Oil",
    "commodity_unit": "USD/Bbl",
    "target_date": "2026-06-12",
    "target_price": 88.71,
    "commodity_price_series": [
        {
            "date": "2026-03-01",
            "value": 96.02
        },
        {
            "date": "2026-03-02",
            "value": 93.04
        },
		...,
        {
            "date": "2026-06-12",
            "value": 88.71
        }
    ]
}

payload = {
    "mode": "run",
    "text": json.dumps({
        "event_info": event_info
    }),
    "stream": False
}

headers = {
    "Authorization": f"Bearer {YOUR_API_KEY}",
    "Content-Type": "application/json"
}

response = requests.post(
    url,
    headers=headers,
    json=payload,
    timeout=60
)
```

---

### Example Response (WAITING_USER)

```json
{
  "success": false,
  "code": "WAITING_USER",
  "data": {
    "task_id": "<task-id>",
    "agent": "supply_chain_risk_prediction",
    "stage": "interpreting",
    "content": "Here are the companies we’ve identified.\nTarget Company Name: Tesla, Inc.\nCountry: United States\nSupplier Company Name: Albemarle Corporation\nCountry: United States\nPlease reply [Yes] or [No] to confirm."
  }
}
```

---

### Continuing After `WAITING_USER`

When the initial `run` request returns `code=WAITING_USER`, the analysis task is paused and waiting for user confirmation.

The client application should display the returned `content` to the user and ask the user to confirm whether the identified company information is correct.

After the user replies with `Yes` or `No`, the client application should submit another request to the same `run` endpoint to continue the existing task.

In this follow-up request:

* `task_id` must be provided in the top-level request body, using the `task_id` returned in the previous `WAITING_USER` response.
* `confirm_company_name` must be added inside the `event_info` object.
* `confirm_company_name` must contain the user's confirmation result.

Valid values:

| Value | Description                                                     |
| ----- | --------------------------------------------------------------- |
| `Yes` | The user confirms the identified company information is correct |
| `No`  | The user rejects the identified company information             |

This field is only required when continuing a task from a `WAITING_USER` response.

The client application must resend the full original `event_info` and add `confirm_company_name`.

After the user replies “Yes”, the event analysis service will be activated, and the one-time fee will be charged.

If the user replies “No”, the current task will not proceed to activation and no fee will be charged. The user may initiate a new `/run` request with more accurate company information, which will create a new assessment task.

---

### Request Body When Continuing From `WAITING_USER`

| Field     | Type    | Required | Description                                                     |
| --------- | ------- | -------: | --------------------------------------------------------------- |
| `mode`    | string  |       No | Execution mode. Defaults to `run` if omitted                    |
| `task_id` | string  |      Yes | Task ID returned by the previous `WAITING_USER` response        |
| `text`    | string  |      Yes | A JSON-dumped string containing the updated `event_info` object |
| `stream`  | boolean |       No | Whether to enable streaming response                            |

The `text` field must still be a JSON string generated from the following structure:

```json
{
  "event_info": {
    "...": "...",
    "confirm_company_name": "Yes"
  }
}
```

#### Python Input

```python
import json
import requests

YOUR_API_KEY = "YOUR_API_KEY"

url = "https://agent.supplygraph.ai/api/v1/agents/supply_chain_risk_prediction/run"

event_info = {
    "event_type": "news",
    "analysis_mode": "normal",
    "title": "U.S. Government Confirms Tesla and LG Energy Solution's $4.3 Billion LFP Battery Deal",
    "content": (
        "In March 2026, the U.S. government confirmed a $4.3 billion agreement between Tesla "
        "and South Korea's LG Energy Solution to support lithium iron phosphate (LFP) battery "
        "cell production in Lansing, Michigan. The facility is expected to begin production in "
        "2027 and supply batteries for Tesla's Megapack 3 energy storage systems manufactured "
        "near Houston. The deal could strengthen Tesla's domestic battery supply chain, reduce "
        "its reliance on Chinese battery imports amid tariff pressure, and support growth in its "
        "energy storage business. However, the financial impact will depend on execution at the "
        "Michigan plant, battery cost competitiveness, demand for Megapack systems, and whether "
        "U.S.-based LFP production can scale efficiently."
    ),
    "pub_date": "2026-03-17T00:00:00-04:00",
    "company_name": "Tesla, Inc.",
    "confirm_company_name": "Yes"
}

payload = {
    "mode": "run",
    "task_id": "<task-id>",
    "text": json.dumps({
        "event_info": event_info
    }),
    "stream": False
}

headers = {
    "Authorization": f"Bearer {YOUR_API_KEY}",
    "Content-Type": "application/json"
}

response = requests.post(
    url,
    headers=headers,
    json=payload,
    timeout=60
)
```


## Integration

| Method | ID / Tool | Documentation |
|--------|-----------|---------------|
| **A2A** | `supply_chain_risk_prediction` | [a2a.md](../a2a_mcp/a2a.md) |
| **MCP** | `supply_chain_risk_prediction` | [mcp.md](../a2a_mcp/mcp.md) |
| **Agent API** | `supply_chain_risk_prediction` | [agent-api.md](../agent-api/agent-api.md) |

Quick examples: [A2A / MCP](../a2a_mcp/quick_example.md) · [Agent API](../agent-api/quick_example.md)

## Errors

Common codes → [Agent API §10](../agent-api/agent-api.md#10-error--status-codes). Returns `WAITING_USER` before task activation. Uses `mode=result` (not `results`).

Maintainer: [info@supplygraph.ai](mailto:info@supplygraph.ai)

License: Proprietary / Internal

© 2025–2026 SupplyGraph AI. All rights reserved.

