# Due Diligence Agent


## Overview

The **Due Diligence Agent** generates a comprehensive, structured, and continually updated intelligence report for any company worldwide.

It consolidates fragmented public data — including corporate registration, legal records, subsidiaries, IP assets, compliance, financial disclosures, workforce, and regulatory signals — into a **standardized, enterprise-grade due diligence dossier**.

This agent is designed for:

- M&A screening
- Supplier onboarding
- Investment research
- Risk and compliance reviews
- Strategic partner evaluation


## Pain

Traditional due diligence is slow, fragmented, and expensive.

Analysts often spend **10+ hours per company** collecting unstructured information from dozens of sources — and by the time the report is finished, much of the data is already outdated.

Key challenges include:

- Manual data aggregation
- Inconsistent formats
- Incomplete coverage
- Lack of continuous monitoring
- High research costs

This creates blind spots in strategic decision-making and risk management.


## Breakthrough

The Due Diligence Agent automates a process that previously required multiple teams and tools.

Behind the scenes, it:

- Aggregates data from a global corporate graph
- Resolves entities and normalizes records
- Structures information into standardized modules
- Continuously refreshes and updates changes

For users, the workflow is simple:

1. Provide a company name
2. Confirm the matched entity
3. Receive a complete or chapter-specific due diligence report

Results are:

- Structured
- Auditable
- Enterprise-ready
- Continuously updated


## Why SupplyGraph AI

Powered by:

- 100M+ global companies
- 8,000+ industry benchmarks
- 1M+ key products and entities

SupplyGraph AI delivers **institution-grade due diligence intelligence** without requiring you to upload any proprietary internal data.

Every insight is:

- Evidence-linked
- Contextualized
- Continuously monitored
- Built for compliance and risk teams


## Try the Due Diligence Agent (Live Chatbot)

Before integrating via API, you can experience this agent directly through our interactive chatbot.

This live demo allows you to:

- Enter a company name
- Confirm the correct legal entity
- Generate a full or chapter-specific report
- Preview the structure and quality of the final output

Launch the Due Diligence Chatbot  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=due_diligence_report

To use the chatbot, you’ll first need to:

- Create a SupplyGraph AI account
- Top up your credit balance

The chatbot is powered by the **same Due Diligence Agent and A2A endpoints** described in this documentation.  
Credits consumed in the chatbot are deducted in the same way as API / A2A usage.

Everything you experience here can be fully embedded into your own system through A2A integration.


## Sandbox Key Support (for Development)

This agent supports **Sandbox API Keys**, enabling developers to test integrations without consuming credits.

When using a Sandbox Key:
- No credits are deducted  
- The agent returns **predefined real sample data** for companies such as **Tesla** or **BYD**, depending on your input  
- Returned data is real and structured, but limited to our predefined sample set  
- Processing is accelerated, and results are returned instantly  
- Only input validation and structural checks are performed  

Sandbox Keys are ideal for:
- UI development  
- Validating your processing logic against realistic datasets  
- SDK / API behavior testing  

⚠️ Sandbox results are not dynamically generated — they come from a predefined dataset and must not be used in production analytics.

For a full comparison of Production vs. Sandbox keys, see  
[Getting Started Guide → API Keys](../getting-started.md#3-generate-an-a2amcp-key).


## Input Requirements

`agent_id`: `due_diligence_report` · MCP tool: `due_diligence_report`

| Field | Required | Description |
|-------|----------|-------------|
| `text` | Yes | Company name and country (e.g. "Tesla, Inc. United States") |
| `chapter_name` | No | Report section to generate; default `ALL` |

**`chapter_name` values:** `ALL`, `Company Registration Information`, `Branch Offices`, `Corporate Brand Initiatives`, `Administrative Sanctions`, `Software Copyright Details`, `Outbound Investments`, `Financing Activities`, `Competitor Analysis`, `Subsidiary Companies`, `Trademark Portfolio`, `Patent Holdings`, `Website Registrations`, `Court Judgments`, `Shareholder Structure`, `Senior Management Team`, `Administrative Permits`, `Court Hearing Notices`, `Court Notices`, `Equity Pledges`, `Mobile Applications`, `Copyrighted Works`, `Equity Freezes`, `Chattel Mortgages`, `WeChat Official Accounts`, `Tendering and Bidding Activities`, `Qualification Certificates`, `Engineering Irregularities`, `Major Regulatory Violations`, `Compensation and Benefits`, `Enforcement Targets`, `Supplier Network`, `Credit Ratings`, `Tax Offenses`, `Regulatory Spot Checks`, `Import-Export Credit Records`, `Regulatory Actions`, `Granted Government Subsidies`, `Eligible Government Subsidies`, `Consolidated Statements of Operations`, `Income Statement`, `Statement of Cash Flows`, `Consolidated Balance Sheets`

Confirm matched company when prompted (`WAITING_USER`).


## Integration

| Method | ID / Tool | Documentation |
|--------|-----------|---------------|
| **A2A** | `due_diligence_report` | [a2a.md](../a2a_mcp/a2a.md) |
| **MCP** | `due_diligence_report` | [mcp.md](../a2a_mcp/mcp.md) |
| **Agent API** | `due_diligence_report` | [agent-api.md](../agent-api/agent-api.md) |

Quick examples: [A2A / MCP](../a2a_mcp/quick_example.md) · [Agent API](../agent-api/quick_example.md)

## Errors

Common codes → [Agent API §10](../agent-api/agent-api.md#10-error--status-codes). This agent returns `WAITING_USER` for company confirmation.

Maintainer: info@supplygraph.ai  
License: Proprietary / Internal  
© 2025–2026 SupplyGraph AI. All rights reserved.
