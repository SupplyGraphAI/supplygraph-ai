# Geographic Concentration Analysis Agent

## Overview

The **Geographic Concentration Analysis Agent** performs a quantitative, multi-tier evaluation of an enterprise’s global supply chain to identify **country-level and regional over-concentration risks**.

By applying **Herfindahl–Hirschman Index (HHI)** calculations and deep-tier graph traversal, it exposes hidden dependency structures that are often invisible in traditional Tier-1 or Tier-2 analysis.

The agent delivers a **clear, decision-ready report** that highlights systemic vulnerabilities, enabling organizations to assess exposure, design mitigation strategies, and strengthen supply resilience before disruption occurs.


## Pain

Most companies believe their supply chain is diversified — but in reality, **hidden concentration often exists deep in Tier-3 through Tier-8 levels** of their network.

Even globally diversified enterprises can rely on a **single dominant geography** for critical upstream inputs without realizing it. In one analysis, Nvidia — widely regarded as having a highly diversified supply chain — was found to have **Tier-6 components with over 90% concentration in a single country** based on HHI analysis.

Such invisible dependencies introduce severe enterprise risks:

- Geopolitical shocks
- Export controls and sanctions
- Natural disasters
- Regional instability

Any one of these can trigger **cascading failures**: component shortages, production delays, revenue loss, and reputational damage.


## Breakthrough

Behind the scenes, SupplyGraph AI executes a node-by-node analysis of a company-centric supply graph, calculating geographic concentration at each tier using statistical benchmarks and real-world enterprise data.

User experience remains simple:

- Provide a company name
- Confirm the matched entity
- Receive a structured, fully documented concentration analysis

The final output includes:

- HHI-based concentration metrics
- Country and region dominance scores
- Dependency distribution charts
- Executive-ready risk summaries

Advanced network science, delivered through an intuitive and actionable format.


## Why SupplyGraph AI

Powered by a continuously updated knowledge graph spanning:

- 100M+ companies
- 1M+ key products
- 8,000+ industry benchmarks

SupplyGraph AI is the **first AI-native infrastructure** designed to perform deep-tier geographic dependency analysis at scale — **without requiring any proprietary data uploads from clients**.

Our platform transforms invisible structural risk into **auditable, explainable intelligence** that decision-makers can trust.


## Try the Geographic Concentration Analysis Agent (Live Chatbot)

Before integrating this agent via API, you can experience it directly through our interactive chatbot.

This live demo allows you to:

- Enter a company name
- Confirm the correct entity
- Analyze geographic concentration across the supply chain
- Identify single-country dependency and hidden exposure risks
- Preview the structure, clarity, and analytical depth of the final report

Launch the Geographic Concentration Analysis Agent  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=sg_chokepoint

To use the chatbot, you’ll first need to:

- Create a SupplyGraph AI account
- Top up your credit balance

The chatbot is powered by the **same Geographic Concentration Analysis Agent and A2A endpoints** described in this documentation.  
Credits consumed in the chatbot are deducted in the same way as API / A2A usage.

Everything you experience here can be fully embedded into your own systems through A2A integration.

## Sandbox Key Support (for Development)

This agent supports **Sandbox API Keys**, enabling developers to test without spending credits.

When using a Sandbox Key:
- No credits are deducted  
- The agent returns a **predefined real geographic concentration dataset for Tesla**  
- Returned content follows the agent’s final output schema  
- No real computation or statistical modeling is executed under Sandbox mode  
- Only structural and format validation is performed  

Suitable for:
- Backend integration testing  
- Visualizing regional-risk heatmaps  
- Building analytics dashboards  

⚠️ Sandbox datasets are static samples.  
Production Keys are required for real, dynamic computations.

For more information, see  
[Getting Started Guide → API Keys](../getting-started.md#3-generate-an-a2amcp-key).


## Input Requirements

`agent_id`: `sg_chokepoint` · MCP tool: `sg_chokepoint`

| Field | Required | Description |
|-------|----------|-------------|
| `text` | Yes | Target company name for geographic concentration analysis |

**Example:** "Analyze geographic concentration risk for Tesla, Inc."

## Integration

| Method | ID / Tool | Documentation |
|--------|-----------|---------------|
| **A2A** | `sg_chokepoint` | [a2a.md](../a2a_mcp/a2a.md) |
| **MCP** | `sg_chokepoint` | [mcp.md](../a2a_mcp/mcp.md) |
| **Agent API** | `sg_chokepoint` | [agent-api.md](../agent-api/agent-api.md) |

Quick examples: [A2A / MCP](../a2a_mcp/quick_example.md) · [Agent API](../agent-api/quick_example.md)

## Errors

Common codes → [Agent API §10](../agent-api/agent-api.md#10-error--status-codes).

Maintainer: info@supplygraph.ai  
License: Proprietary / Internal  
© 2025–2026 SupplyGraph AI. All rights reserved.
