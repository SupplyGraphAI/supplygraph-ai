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

## Try the Supply Chain Risk Prediction Agent (Live Chatbot)

Before integrating this agent via API, you can experience it directly through our interactive risk prediction chatbots.

This live demo allows you to:

- Submit a target company and a risk event (news, policy, or commodity price)
- See how the event may propagate through multi-tier supply chains
- Review company-level impact, key paths, and key nodes
- Explore role-adapted interpretations for executives, investors, and risk professionals
- Experience the same assessment workflow used by the A2A / API integration

Launch the Supply Chain Risk Prediction Chatbot for Corporate Executives  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=risk_propagation_detail_ceo

Launch the Supply Chain Risk Prediction Chatbot for Hedge Fund Managers  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=risk_propagation_detail_hedgefund

Launch the Supply Chain Risk Prediction Chatbot for Supply Chain Risk Professionals  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=risk_propagation_detail_riskexpert

To use the chatbot, you’ll first need to:

- Create a SupplyGraph AI account
- Top up your credit balance

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

The full A2A workflow (`message:send → tasks/get`) behaves the same as production, enabling accurate integration testing.

## Initial Risk Assessment Setup

When a user initiates a new risk assessment request through **`message:send`** (without `message.taskId`), the agent starts the company-level supply chain risk assessment process.

The request should provide the target company and the event-related context required for the assessment.

The agent then identifies the matched company and prepares the risk propagation task for user confirmation.

First-time workflow:

1. User sends **`message:send`** with target company and event-related information
2. System creates a Task and returns a `taskId`
3. Agent returns matched company candidates and task setup information (`status.state: "input-required"`)
4. User confirms the matched company and task setup by sending another **`message:send`** with the same `taskId` / `contextId` and replying `Yes`
5. The risk assessment task is activated
6. The applicable fee is charged
7. User retrieves the latest assessment result via **`tasks/get`** (read `artifacts` when `status.state` is `completed`)

After activation:

* The agent runs the company-level supply chain risk assessment
* Results are updated as new information becomes available
* Each **`tasks/get`** poll returns the latest available assessment data in `artifacts` when completed
* Task `status.state` becomes `completed` after the initial setup and assessment process is finished

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

Backtest mode uses the same `message:send → tasks/get` workflow as normal mode.

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
* In `normal` mode, the current price is **264,590 credits ($49,999) per event per target company**
* In `backtest` mode, the assessment is charged at **30% of the normal mode price**
* The first **10 backtest risk cases** are free of charge
* This is a **one-time fee**, not a subscription

The fee covers:

* Company-level event impact assessment
* Supply chain node filtering and evaluation
* Multi-tier propagation path analysis
* Enterprise-level risk synthesis
* Latest assessment result retrieval through completed Task **artifacts** (`tasks/get`)
* Structured and auditable warning outputs

## Understanding `taskId` and Assessment Lifecycle

A Task `id` (`taskId`) represents a company-level supply chain risk assessment Task.

### How `taskId` is created and maintained

* **`message:send`** without `message.taskId` creates a new Task
* After user confirmation, the Task becomes an active assessment instance
* The Task remains valid for retrieving the latest available assessment result via **`tasks/get`**
* The system recognizes the assessment instance by Task `id` (`taskId`)

Implications:

* If a user loses the `taskId` and sends a new **`message:send`** without `message.taskId`, the system treats it as a new assessment request
* Different target companies or event contexts are treated as independent Tasks
* Each Task is priced and processed independently

## Task Status (`tasks/get`)

Use **`GET {agent_base}/tasks/{task_id}`** (`tasks/get`) to check whether task setup and assessment have completed, and to retrieve the latest Task snapshot.

For this agent:

* Poll until `status.state` reaches a terminal value (`completed`, `failed`, or `canceled`)
* Once the initial setup and assessment process is completed, `status.state` remains `completed`
* When `completed`, the latest available risk assessment data is returned in **`artifacts`**
* Repeated `tasks/get` after completion continues to return the latest available result
* Intermediate states such as `working` and `input-required` are observed via polling (or `message:stream`); they are not pushed by webhook

## Push Notification Webhook (A2A v1.0)

This agent supports A2A **push notifications** (`capabilities.pushNotifications = true`).

On the **first** `message:send` / `message:stream` for a new Task (when `message.taskId` is omitted), clients may register:

```json
"configuration": {
  "pushNotificationConfig": {
    "url": "https://client.example.com/webhook/a2a",
    "token": "optional-client-verification-token"
  }
}
```

When the Task reaches a **terminal state**, SupplyGraph AI sends **one POST** to the registered `url` to notify the caller that the Task has finished.

After receiving the webhook, clients SHOULD call **`tasks/get`** to fetch the full Task (including `artifacts`).

Notes:

* Webhooks notify **completion only** — not intermediate progress
* `pushNotificationConfig` is accepted **only on the first request** for a new Task; follow-up turns with an existing `taskId` do not update the webhook
* Full webhook payload, JWT verification, and JWKS details → [a2a.md §6](../a2a_mcp/a2a.md#6-push-notifications-webhooks)

## Result Retrieval (`tasks/get` artifacts)

When **`tasks/get`** returns `status.state: "completed"`, the **latest available supply chain risk assessment data** is in `artifacts[].parts[].data`.

You may call `tasks/get` repeatedly using the same `taskId` to refresh the result.

The completed Task `artifacts` include:

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

# A2A Integration Overview

This section provides an overview of the **A2A (Agent-to-Agent)** interface used to integrate this agent into other systems.

## Endpoints Summary

**Agent Base URL:**

```
https://agent.supplygraph.ai/a2a/agents/supply_chain_risk_prediction
```

All execution endpoints below are relative to `{agent_base}`.

| Endpoint | Method | A2A operation | Description |
|----------|--------|---------------|-------------|
| `{agent_base}` | GET | Agent Card | Discover agent metadata, capabilities, and skills |
| `{agent_base}/message:send` | POST | `message/send` | Submit or continue an assessment; returns a **Task** |
| `{agent_base}/message:stream` | POST | `message/stream` | Same as send, with SSE progress updates |
| `{agent_base}/tasks/{task_id}` | GET | `tasks/get` | Poll Task status; when `completed`, read result **artifacts** |

Typical workflow: **`message:send` → (optional confirm via another `message:send`) → `tasks/get`**.

Optional: register `configuration.pushNotificationConfig` on the first `message:send` to receive a completion webhook (see Push Notification Webhook above).

## Agent Card

Retrieve the agent's capabilities, input/output modes, and skills (A2A discovery):

### Request

```bash
curl -X GET "https://agent.supplygraph.ai/a2a/agents/supply_chain_risk_prediction" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "A2A-Version: 1.0"
```

### Example Response (abbreviated)

```json
{
  "name": "Supply Chain Risk Prediction Agent",
  "description": "Evaluates how a supply chain risk event may affect a target company through multi-tier supply chain propagation analysis.",
  "url": "https://agent.supplygraph.ai/a2a/agents/supply_chain_risk_prediction",
  "version": "1.0.0",
  "protocolVersion": "1.0",
  "capabilities": {
    "streaming": true,
    "extendedAgentCard": true,
    "pushNotifications": true
  },
  "defaultInputModes": ["text/plain", "application/json"],
  "defaultOutputModes": ["text/plain", "application/json"],
  "skills": [
    {
      "id": "supply_chain_risk_prediction",
      "name": "Supply Chain Risk Prediction",
      "description": "Assess company-level impact of a news, policy, or commodity-price event via multi-tier supply chain propagation.",
      "tags": ["supply chain", "risk", "propagation"],
      "examples": [
        "Assess supply chain risk impact of a news event on Tesla, Inc."
      ]
    }
  ],
  "supportedInterfaces": [
    {
      "protocolBinding": "HTTP+JSON",
      "url": "https://agent.supplygraph.ai/a2a/agents/supply_chain_risk_prediction",
      "protocolVersion": "1.0"
    },
    {
      "protocolBinding": "JSONRPC",
      "url": "https://agent.supplygraph.ai/a2a/agents/supply_chain_risk_prediction",
      "protocolVersion": "1.0"
    }
  ]
}
```

The response is an **Agent Card** (`application/a2a+json`). For authenticated extended metadata (e.g. pricing), use `GET {agent_base}/extendedAgentCard` when `capabilities.extendedAgentCard` is `true`. Full discovery flow → [a2a.md](../a2a_mcp/a2a.md).

## Supported Assessment Options

This agent supports two **analysis modes** (via `event_info.analysis_mode`):

| Analysis Mode | Description                                                                  |
| ------------- | ---------------------------------------------------------------------------- |
| `normal`      | Run normal forward-looking or current event impact analysis                  |
| `backtest`    | Run backtest analysis based on historical event or historical market context |

It currently supports three **event types** (via `event_info.event_type`):

| Event Type        | Description                                                              |
| ----------------- | ------------------------------------------------------------------------ |
| `news`            | Analyze the impact of a news event on the target company                 |
| `policy`          | Analyze the impact of a policy or regulatory event on the target company |
| `commodity_price` | Analyze the impact of commodity price changes on the target company      |

It also supports three optional **roles** for role-adapted interpretation (via DataPart JSON field `role` when submitting the Task):

| Role         | Description                                                                 |
| ------------ | --------------------------------------------------------------------------- |
| `ceo`        | Executive-oriented view: enterprise exposure, business impact, severity, timing, management actions |
| `hedgefund`  | Investor-oriented view: operational impact, financial relevance, market signals, investment implications |
| `riskexpert` | Risk-professional view: impacted nodes, propagation paths, key paths/nodes, mechanisms, mitigation |

If `role` is omitted, the agent returns the default enterprise risk assessment result without role-specific interpretation.

---

### Endpoint

```http
POST https://agent.supplygraph.ai/a2a/agents/supply_chain_risk_prediction/message:send
```

---

### Request Body

Submit a standard A2A `message:send` request. Put natural-language intent in a **TextPart**, and structured `event_info` in a **DataPart**:

```json
{
  "message": {
    "messageId": "msg-uuid",
    "role": "user",
    "parts": [
      {
        "kind": "text",
        "text": "Please analyze the following data"
      },
      {
        "kind": "data",
        "data": {
          "event_info": {
            "...": "..."
          }
        },
        "mediaType": "application/json"
      }
    ],
    "contextId": "ctx-uuid"
  }
}
```

| Part | Type | Required | Description |
|------|------|----------|-------------|
| TextPart | `{ "kind": "text", "text": "..." }` | Recommended | Natural-language instruction, e.g. `"Please analyze the following data"` |
| DataPart | `{ "kind": "data", "data": { "event_info": { ... } }, "mediaType": "application/json" }` | Yes | Structured assessment input; see [Event Info Schema](#event-info-schema) |

Optional: include `role` inside the DataPart JSON (e.g. `"role": "ceo"`) when submitting or continuing the Task. Optional: set `message.taskId` to continue an existing Task after `input-required`.

---

## Event Info Schema

The `event_info` object is required.

### Common Required Fields

The following fields are required for all event types:

| Field           | Type   | Required | Format / Description                                                 |
| --------------- | ------ | -------: | -------------------------------------------------------------------- |
| `event_type`    | string |      Yes | Must be one of `news`, `policy`, or `commodity_price`                |
| `analysis_mode` | string |      Yes | Must be one of `normal` or `backtest`                                |
| `company_name`  | string |      Yes | Target company name to be assessed. Provide the most specific legal or commonly recognized company name. To improve matching accuracy, include country, region, ticker, exchange, or other identifying information when available, for example: `Tesla, Inc. (United States, NASDAQ: TSLA)` or `BYD Company Limited (China, HKEX: 1211)`. |
| `role`          | string |       No | Optional role-adapted interpretation. Must be one of `ceo`, `hedgefund`, or `riskexpert`. If omitted, the default enterprise risk assessment result is returned. |

---

## Event Type: `news` or `policy`

When `event_type` is `news` or `policy`, include the [Common Required Fields](#common-required-fields), and add the event-specific fields below.

### Event-Specific Fields

These fields vary by event type. For `news` / `policy`, provide:

| Field      | Type   | Required | Format / Description                                                                                                                                                          |
| ---------- | ------ | -------: | ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `title`    | string |      Yes | Title of the event                                                                                                                                                            |
| `content`  | string |      Yes | Full event content or detailed description                                                                                                                                    |
| `pub_date` | string |      Yes | Publication time. Must be an RFC 3339 date-time string with a timezone offset, for example: `2026-03-17T00:00:00-04:00`, `2026-03-17T13:30:00+08:00`, or `2026-03-17T17:30:00Z`. |

---

### Request Example: News / Policy Event

#### Python Input

```python
import uuid
import requests

YOUR_API_KEY = "YOUR_API_KEY"

url = "https://agent.supplygraph.ai/a2a/agents/supply_chain_risk_prediction/message:send"

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
    "message": {
        "messageId": str(uuid.uuid4()),
        "role": "user",
        "parts": [
            {
                "kind": "text",
                "text": "Please analyze the following data"
            },
            {
                "kind": "data",
                "data": {
                    "event_info": event_info
                },
                "mediaType": "application/json"
            }
        ],
        "contextId": str(uuid.uuid4())
    }
}

headers = {
    "Authorization": f"Bearer {YOUR_API_KEY}",
    "Content-Type": "application/a2a+json",
    "A2A-Version": "1.0"
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

When `event_type` is `commodity_price`, include the [Common Required Fields](#common-required-fields), and add the event-specific fields below.

### Event-Specific Fields

These fields vary by event type. For `commodity_price`, provide:

| Field                    | Type   | Required | Format / Description                                                                                                                                                                                                 |
| ------------------------ | ------ | -------: | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `commodity_name`         | string |      Yes | Name of the commodity                                                                                                                                                                                                |
| `commodity_unit`         | string |      Yes | Unit of the commodity price                                                                                                                                                                                          |
| `target_date`            | string |      Yes | Target analysis date. Format: `YYYY-MM-DD`                                                                                                                                                                           |
| `target_price`           | number |      Yes | Commodity price on `target_date`                                                                                                                                                                                     |
| `commodity_price_series` | array  |      Yes | Historical commodity price time series. Each item is `{ "date": "YYYY-MM-DD", "value": <number> }`. Provide at least 90 calendar days of data. The maximum `date` in the series must equal `target_date`. Example item: `{ "date": "2026-06-12", "value": 88.71 }`. |

---

### Request Example: Commodity Price Event

#### Python Input

```python
import uuid
import requests

YOUR_API_KEY = "YOUR_API_KEY"

url = "https://agent.supplygraph.ai/a2a/agents/supply_chain_risk_prediction/message:send"

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
        # ... additional daily points ...
        {
            "date": "2026-06-12",
            "value": 88.71
        }
    ]
}

payload = {
    "message": {
        "messageId": str(uuid.uuid4()),
        "role": "user",
        "parts": [
            {
                "kind": "text",
                "text": "Please analyze the following data"
            },
            {
                "kind": "data",
                "data": {
                    "event_info": event_info
                },
                "mediaType": "application/json"
            }
        ],
        "contextId": str(uuid.uuid4())
    }
}

headers = {
    "Authorization": f"Bearer {YOUR_API_KEY}",
    "Content-Type": "application/a2a+json",
    "A2A-Version": "1.0"
}

response = requests.post(
    url,
    headers=headers,
    json=payload,
    timeout=60
)
```

---

### Example Response (`input-required`)

When company matching needs user confirmation, `message:send` returns a Task with `status.state: "input-required"`. The confirmation prompt is in `status.message.parts`:

```json
{
  "id": "<task-id>",
  "contextId": "<context-id>",
  "status": {
    "state": "input-required",
    "timestamp": "2026-03-17T12:00:00Z",
    "message": {
      "messageId": "<agent-message-id>",
      "role": "agent",
      "parts": [
        {
          "kind": "text",
          "text": "Here are the companies we’ve identified.\nTarget Company Name: Tesla, Inc.\nCountry: United States\nSupplier Company Name: Albemarle Corporation\nCountry: United States\nPlease reply [Yes] or [No] to confirm."
        }
      ]
    }
  },
  "artifacts": [],
  "history": []
}
```

You may also observe the same `input-required` state by polling **`tasks/get`**.

---

### Continuing After `input-required`

When the Task is in `input-required`, the assessment is paused and waiting for user confirmation.

The client application should display `status.message` to the user and ask whether the identified company information is correct.

After the user replies with `Yes` or `No`, submit another **`message:send`** to the same agent base URL to continue the existing Task.

In this follow-up request:

* `message.taskId` must be the Task `id` returned previously
* `message.contextId` must be the same `contextId`
* Resend the full original `event_info` in a DataPart
* Add `confirm_company_name` at the **same level as** `event_info` inside the DataPart `data` object

Valid values:

| Value | Description                                                     |
| ----- | --------------------------------------------------------------- |
| `Yes` | The user confirms the identified company information is correct |
| `No`  | The user rejects the identified company information             |

`confirm_company_name` is only required when continuing a Task from `input-required`.

After the user replies `Yes`, the event analysis service is activated and the one-time fee is charged.

If the user replies `No`, the current Task does not proceed to activation and no fee is charged. The user may start a new `message:send` (without `message.taskId`) with more accurate company information, which creates a new assessment Task.

---

### Request Body When Continuing From `input-required`

DataPart `data` structure:

```json
{
  "event_info": {
    "...": "..."
  },
  "confirm_company_name": "Yes"
}
```

#### Python Input

```python
import uuid
import requests

YOUR_API_KEY = "YOUR_API_KEY"
TASK_ID = "<task-id>"
CONTEXT_ID = "<context-id>"

url = "https://agent.supplygraph.ai/a2a/agents/supply_chain_risk_prediction/message:send"

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
    "message": {
        "messageId": str(uuid.uuid4()),
        "role": "user",
        "parts": [
            {
                "kind": "text",
                "text": "Please confirm the matched company"
            },
            {
                "kind": "data",
                "data": {
                    "event_info": event_info,
                    "confirm_company_name": "Yes"
                },
                "mediaType": "application/json"
            }
        ],
        "contextId": CONTEXT_ID,
        "taskId": TASK_ID
    }
}

headers = {
    "Authorization": f"Bearer {YOUR_API_KEY}",
    "Content-Type": "application/a2a+json",
    "A2A-Version": "1.0"
}

response = requests.post(
    url,
    headers=headers,
    json=payload,
    timeout=60
)
```

## Task Status & Result (`tasks/get`)

### Purpose

Use **`tasks/get`** both to check Task progress and to retrieve the assessment result.

There is no separate status or result endpoint in A2A. Poll:

```text
GET {agent_base}/tasks/{task_id}
```

and inspect `status.state`:

| `status.state` | Meaning | What to do |
| ------------- | ------- | ---------- |
| `working` | Assessment is in progress | Keep polling |
| `input-required` | User confirmation is needed | Continue with another `message:send` |
| `completed` | Assessment finished | Read the result from `artifacts` |
| `failed` / `canceled` | Terminal failure or cancellation | Stop; inspect `status.message` if present |

For this agent, once the initial setup and assessment process is completed, `status.state` remains `completed`. Repeated `tasks/get` after completion continues to return the latest available result in `artifacts`.

When `status.state` is `completed`, `artifacts[].parts[].data` contains company-specific risk assessment data, including event information, target company information, enterprise-level risk interpretation, impacted nodes, propagation paths, impact graph data, and analysis dossier content.

### Request

```bash
curl -X GET "https://agent.supplygraph.ai/a2a/agents/supply_chain_risk_prediction/tasks/<task-id>" \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "A2A-Version: 1.0"
```

Optional query parameters: `historyLength`, `contextId` (see [a2a.md §5.3](../a2a_mcp/a2a.md#53-get-task-tasksget)).

### Example Response (`working`)

```json
{
  "id": "<task-id>",
  "contextId": "<context-id>",
  "status": {
    "state": "working",
    "timestamp": "2026-03-17T12:01:00Z"
  },
  "artifacts": [],
  "history": []
}
```

### Example Response (`completed`)

```json
{
  "id": "<task-id>",
  "contextId": "<context-id>",
  "status": {
    "state": "completed",
    "timestamp": "2026-03-17T12:05:00Z"
  },
  "artifacts": [
    {
      "artifactId": "<artifact-id>",
      "name": "result",
      "parts": [
        {
          "kind": "data",
          "data": {
            "success": true,
            "task_id": "<task-id>",
            "event_id": "<event-id>",
            "company_id": "<company-id>",
            "company_name": "<company-name>",
            "event_summary": "<event-summary>",
            "event_date": "2026-06-22",
            "event_type": "Supply Chain Disruption",
            "impact_score": 0,
            "assessment_confidence_score": 75,
            "impact_date": null,
            "impact_end_date": null,
            "risk_brief": {},
            "impact_result": {},
            "path_risk_interpretation_list": []
          }
        }
      ]
    }
  ],
  "history": []
}
```

The response payload inside `artifacts[].parts[].data` contains the company-specific ECRA (Enterprise Chain Risk Assessment) result.

The output format is **JSONL-compatible structured JSON**, where each assessment record represents the evaluation result of **one event against one target company**.

#### Top-Level Response Fields

| Field                           | Type           | Description                                               |
| ------------------------------- | -------------- | --------------------------------------------------------- |
| `task_id`                       | string         | ECRA task ID                                              |
| `event_id`                      | string         | Event master ID (`master_event_id`)                       |
| `company_id`                    | string         | Target company ID (`pid`)                                 |
| `company_name`                  | string         | Target company name                                       |
| `event_summary`                 | string         | Event summary                                             |
| `event_date`                    | string / null  | Event occurrence date or first appearance date            |
| `event_type`                    | string         | Event type                                                |
| `impact_score`                  | number / null  | Overall impact score. See scoring definition below        |
| `assessment_confidence_score`   | integer / null | Assessment confidence score. See scoring definition below |
| `impact_date`                   | string / null  | Expected impact start date                                |
| `impact_end_date`               | string / null  | Expected impact end date                                  |
| `risk_brief`                    | object / null  | Human-readable risk brief structure                       |
| `impact_result`                 | object / null  | Complete enterprise-level structured assessment result    |
| `path_risk_interpretation_list` | array          | Complete path-level risk interpretation list              |

#### Impact Scoring Definitions

##### `impact_score` (Impact Severity)

* **Range:** `[-20, +20]`
* **Meaning:**

  * **Positive value:** adverse impact to the target company
  * **Negative value:** beneficial impact to the target company
  * **Larger absolute value:** stronger impact intensity

##### `assessment_confidence_score` (Assessment Confidence)

* **Range:** `[0, 100]`
* **Meaning:** Confidence level of the assessment conclusion.
* This score is independent from `impact_score` and does **not** represent impact severity.
* Higher scores indicate stronger evidence and higher confidence.
* Lower scores indicate insufficient evidence, uncertain transmission mechanisms, or more assumptions.

##### `impact_date` / `impact_end_date`

* Defines the expected impact window.
* Indicates when the impact may emerge or be mitigated.
* Does **not** represent the event publication date.

#### `risk_brief` (Risk Brief)

`risk_brief` is projected from the complete assessment result and provides a simplified structure for business users.

It is designed for quick interpretation without requiring direct parsing of the full `impact_result` or path-level analysis data.

##### First-Level Structure

| Field            | Description                                               |
| ---------------- | --------------------------------------------------------- |
| `schema_version` | Risk brief schema version                                 |
| `meta`           | Basic task, event, and company information                |
| `what_impact`    | What impact the target company receives                   |
| `why_impact`     | Why the target company is affected (paths and mechanisms) |
| `evidence_chain` | Signal chain and evidence chain                           |
| `price_data`     | Commodity price data                                      |

##### `risk_brief.meta`

| Field          | Type   | Description                          |
| -------------- | ------ | ------------------------------------ |
| `task_id`      | string | ECRA task ID                         |
| `event_id`     | string | Event ID                             |
| `event_title`  | string | Event title or short display summary |
| `company_id`   | string | Company ID                           |
| `company_name` | string | Company name                         |


#### `risk_brief.what_impact` (What Impact the Target Company Receives)

This section describes **what the target company is affected by**, including impacted products, impact direction, risk level, and supply chain impact mapping.

| Field                         | Type           | Description                                                                                                                       |
| ----------------------------- | -------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| `affected_products`           | array          | List of affected Tier0 products or primary business products                                                                      |
| `impact_score`                | number / null  | Same as top-level field. Range: `[-20, +20]`; positive = adverse impact, negative = beneficial impact                             |
| `impact_direction`            | string / null  | Impact direction, such as `positive` / `negative`                                                                                 |
| `impact_is_beneficial`        | boolean        | Whether the impact is beneficial to the target company                                                                            |
| `risk_level`                  | string / null  | Adverse risk level, such as `high` / `medium` / `low`. This represents risk severity only and does not indicate benefit magnitude |
| `impact_date`                 | string / null  | Expected impact start date (`YYYY-MM-DD`)                                                                                         |
| `impact_end_date`             | string / null  | Expected impact end date                                                                                                          |
| `assessment_confidence_score` | integer / null | Assessment confidence score (`0-100`)                                                                                             |
| `assessment_confidence_level` | string / null  | Confidence level category, such as `high` / `medium` / `low`                                                                      |
| `supply_chain_impact_map`     | object         | Impact mapping across six major supply chain business areas                                                                       |
| `summary`                     | string         | Company-level impact summary                                                                                                      |

##### `what_impact.affected_products[]`

| Field                   | Type          | Description                                                                |
| ----------------------- | ------------- | -------------------------------------------------------------------------- |
| `product`               | string        | Affected product or business segment name, typically a Tier0 product label |
| `path_ids`              | string[]      | List of path IDs associated with this product                              |
| `max_path_impact_score` | number / null | Maximum absolute `path_impact_score` among associated paths                |

##### `what_impact.supply_chain_impact_map`

The keys typically represent supply chain business stages, including:

* `procurement`
* `production`
* `cost`
* `delivery`
* `inventory`
* `continuity`

Each stage contains the following fields:

| Field         | Type          | Description                              |
| ------------- | ------------- | ---------------------------------------- |
| `direction`   | string / null | Impact direction for this business stage |
| `intensity`   | string / null | Qualitative impact intensity             |
| `description` | string        | Brief description of the impact          |

---

#### `risk_brief.why_impact` (Why the Target Company Is Affected)

This section explains the transmission logic from the event to the target company through supply chain paths and impact mechanisms.

| Field        | Type   | Description                              |
| ------------ | ------ | ---------------------------------------- |
| `title`      | string | Section title: "Why Impact"              |
| `paths`      | array  | Summary list of valid transmission paths |
| `mechanisms` | array  | Deduplicated impact mechanism summaries  |

##### `why_impact.paths[]`

| Field                   | Type          | Description                                                                                                  |
| ----------------------- | ------------- | ------------------------------------------------------------------------------------------------------------ |
| `path_id`               | string        | Transmission path ID                                                                                         |
| `node_chain`            | string        | Node chain describing the transmission path                                                                  |
| `affected_product`      | string        | Target product or Tier0 label associated with the path                                                       |
| `path_impact_score`     | number / null | Path-level impact score. Same semantics as enterprise-level score: positive = adverse, negative = beneficial |
| `path_impact_direction` | string / null | Path-level impact direction                                                                                  |
| `mechanism_type`        | string / null | Impact mechanism type, such as supply shortage transmission                                                  |
| `event_exposed_node`    | string / null | Upstream node directly exposed to the event                                                                  |
| `business_impact`       | string        | Brief business impact description                                                                            |
| `final_interpretation`  | string        | Final interpretation of the path impact                                                                      |
| `risk_visibility`       | string / null | Whether the impact has become visible, such as `visible`, `not_visible`, or `uncertain`                      |

**Note:**
`not_visible` does not mean that there is no impact. It indicates that the impact has not yet been observed or confirmed at the company level.

##### `why_impact.mechanisms[]`

| Field                   | Type          | Description                                                                                     |
| ----------------------- | ------------- | ----------------------------------------------------------------------------------------------- |
| `path_id`               | string        | Representative path ID                                                                          |
| `affected_product`      | string        | Target product label                                                                            |
| `mechanism_type`        | string / null | Impact mechanism type                                                                           |
| `event_exposed_node`    | string / null | Node directly exposed to the event                                                              |
| `event_effect_on_node`  | string / null | Event impact on the node, such as supply tightening or production interruption                  |
| `company_exposure_role` | string / null | Role of the target company in the transmission chain                                            |
| `direction_to_company`  | string / null | Impact direction transmitted to the company, such as `adverse`                                  |
| `path_validity`         | string / null | Path validity assessment                                                                        |
| `evidence_level`        | string / null | Evidence level, such as confirmed, reasonable inference, weak assumption, or pending validation |
| `reasoning`             | string        | Mechanism reasoning chain                                                                       |
| `requires_conditions`   | array         | Preconditions required for the impact mechanism to take effect                                  |
| `node_chain`            | string        | Corresponding node chain                                                                        |

---

#### `risk_brief.evidence_chain` (Signal Chain / Evidence Chain)

This section provides supporting evidence explaining upstream exposure and observed impact manifestation.

| Field                       | Type     | Description                                                           |
| --------------------------- | -------- | --------------------------------------------------------------------- |
| `title`                     | string   | Display title                                                         |
| `summary`                   | string   | Overall evidence summary                                              |
| `exposure_paragraph`        | string   | Narrative description of upstream exposure                            |
| `manifestation_paragraph`   | string   | Narrative description of impact manifestation                         |
| `exposure_evidence`         | array    | Evidence items related to upstream exposure                           |
| `manifestation_evidence`    | array    | Evidence items showing observed impact                                |
| `supporting_evidence_other` | array    | Other supporting evidence, including price or market-related evidence |
| `evidence_gaps`             | string[] | Description of current evidence gaps                                  |

##### Evidence Item Structure

The following structure applies to:

* `exposure_evidence[]`
* `manifestation_evidence[]`
* `supporting_evidence_other[]`

| Field                 | Type          | Description                                             |
| --------------------- | ------------- | ------------------------------------------------------- |
| `evidence_id`         | string / null | Evidence ID                                             |
| `category`            | string / null | Evidence category, such as `exposure` or `price_market` |
| `evidence_type`       | string / null | Evidence subtype                                        |
| `claim`               | string        | Evidence statement or display text                      |
| `verification_status` | string / null | Verification status, such as `verified`                 |
| `provenance`          | string / null | Evidence source description                             |
| `path_id`             | string / null | Associated transmission path                            |
| `tier0_products`      | array         | Associated Tier0 product labels                         |
| `urls`                | string[]      | Related URLs                                            |

---

#### `risk_brief.price_data` (Commodity Price Data)

This section provides commodity price-related information associated with affected nodes and transmission paths.

| Field   | Type   | Description                                    |
| ------- | ------ | ---------------------------------------------- |
| `title` | string | Display title: "Commodity Price Data Involved" |
| `items` | array  | List of price-related data entries             |

##### `price_data.items[]`

| Field                     | Type          | Description                                                                                                       |
| ------------------------- | ------------- | ----------------------------------------------------------------------------------------------------------------- |
| `node_name`               | string / null | Node name with available price data (material or product)                                                         |
| `path_id`                 | string / null | Associated transmission path ID                                                                                   |
| `affected_product`        | string / null | Associated target product                                                                                         |
| `has_price_data`          | boolean       | Whether usable price data is available                                                                            |
| `as_of_date`              | string / null | Price data reference date                                                                                         |
| `price_evidence_summary`  | string        | Summary of price evidence, including price increase, decrease, or volatility                                      |
| `related_commodity_price` | array         | Related commodity price details (subset of original structure; item format may vary depending on the data source) |

---

#### Original Full-Detail Fields

The following fields contain complete structured assessment details for downstream processing or advanced analysis.

##### `impact_result`

`impact_result` contains the complete enterprise-level structured assessment result.

It may include:

| Field                          | Description                                                 |
| ------------------------------ | ----------------------------------------------------------- |
| `overall_summary`              | Overall enterprise impact summary                           |
| `supply_chain_impact_map`      | Detailed supply chain impact mapping                        |
| `assessment_evidence`          | Assessment evidence and supporting information              |
| Time-based integrated analysis | Temporal impact analysis and related assessment information |

The structure may contain additional fields depending on the assessment scenario and data availability.

---

##### `path_risk_interpretation_list`

`path_risk_interpretation_list` contains detailed risk interpretation results for each transmission path.

Each path-level record may include:

* Path validity assessment
* Impact mechanism classification
* Business impact interpretation
* Supporting evidence
* Related materials and nodes
* Path-level risk reasoning

**Path validity rule:**

* `is_valid_path = true` indicates a valid transmission path included in the assessment.
* `is_valid_path = false` indicates an invalid or unsupported transmission path and should not be considered as an effective risk transmission route.

---

## Make Your First A2A Call

Typical workflow:

1. Send `message:send` with the target company and event information
2. Confirm matched companies if the Task returns `input-required`
3. Poll `tasks/get` until `status.state` is `completed`
4. Read the assessment result from `artifacts[].parts[].data`

## Integration

| Method | ID / Tool | Documentation |
|--------|-----------|---------------|
| **A2A** | `supply_chain_risk_prediction` | [a2a.md](../a2a_mcp/a2a.md) |
| **MCP** | `supply_chain_risk_prediction` | [mcp.md](../a2a_mcp/mcp.md) |
| **Agent API** | `supply_chain_risk_prediction` | [agent-api.md](../agent-api/agent-api.md) |

Call pattern is identical across agents — only `agent_id` / tool name and input differ.

Quick examples: [A2A / MCP](../a2a_mcp/quick_example.md) · [Agent API](../agent-api/quick_example.md)


## Errors

Common codes → [Agent API §10](../agent-api/agent-api.md#10-error--status-codes).

| Situation | Code | Agent behavior |
|-----------|------|----------------|
| Ambiguous or unmatched company (A2A / Agent API) | `WAITING_USER` | Prompts for entity confirmation; resume with same `taskId` / `task_id` (`input-required` in A2A) |
| Insufficient credits | `INSUFFICIENT_CREDITS` | Top up via [Console](https://supplygraph.ai/zk_chat_os/dashboard/dashboard.html) |
| Assessment still running | `TASK_RUNNING` | Continue polling `tasks/get` until `status.state` is `completed` |
| Invalid or incomplete `event_info` | `INVALID_REQUEST` | Fix required fields and resubmit |

Maintainer: [info@supplygraph.ai](mailto:info@supplygraph.ai)

License: Proprietary / Internal

© 2025-2026 SupplyGraph AI. All rights reserved.
