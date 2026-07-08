# Enterprise Supply Graph Agent


## Overview

The **Enterprise Supply Graph Agent** generates a multi-tier, company-centric global supply chain graph for any enterprise in the world.

It automatically maps relationships across:

- Tier-1 suppliers
- Tier-N upstream entities
- Products and sub-components
- Regions and countries
- Raw materials and critical inputs

This agent transforms fragmented supply networks into a **living, explorable, and actionable graph** — enabling organizations to see, understand, and manage their real dependencies for the first time.


## Pain

Most companies operate with **severely limited visibility** into their true supply networks.

Without deep-tier transparency:

- Hidden single-country dependencies go undetected  
- Alternatives remain invisible  
- Regional concentration risks accumulate silently  
- Geopolitical or regulatory shocks cause sudden disruption  
- Incident response is reactive instead of strategic  

Traditional tools only reveal Tier-1 suppliers at best — leaving 80–90% of the real risk surface invisible.


## Breakthrough

The Enterprise Supply Graph Agent solves this by:

- Expanding a company-centered graph node-by-node
- Linking suppliers, products, regions, and materials across multiple tiers
- Structuring dependencies into analyzable paths and clusters
- Creating the foundation for:
  - Concentration risk analysis
  - Alternative supplier discovery
  - Impact simulation
  - Disruption forecasting

For the user, the experience is simple:

1. Enter a company name
2. Confirm the correct entity
3. Instantly visualize an interactive, multi-tier supply network

What was once invisible becomes navigable, measurable, and actionable.


## Why SupplyGraph AI

Powered by:

- 100M+ global companies
- 1M+ key products and components
- 8,000+ industry benchmarks
- Continuous monitoring, 24/7

SupplyGraph AI delivers the **first truly intelligent, real-time, multi-tier enterprise supply graph** in the world — turning hidden dependency structures into strategic advantage.


## Try the Enterprise Supply Graph Agent (Live Chatbot)

Before integrating this agent via API, you can experience it directly through our interactive visualization chatbot.

This live demo allows you to:

- Enter a company name
- Automatically generate a multi-tier supply graph (Tier-1 to deep-tier)
- Visualize product, supplier, and regional dependencies
- Identify potential concentration and exposure risks
- Explore alternative sourcing pathways
- Experience the depth and structure of the intelligence graph

Launch the Enterprise Supply Graph Chatbot  
https://supplygraph.ai/zk_chat_os/agentic/dialog.html?name=sg_visualization

To use the chatbot, you’ll first need to:

- Create a SupplyGraph AI account
- Top up your credit balance

This chatbot is powered by the **same Enterprise Supply Graph Agent and A2A endpoints** described in this documentation.  
Credits used in the chatbot are deducted in the same way as API / A2A usage.

Everything you experience here can be fully embedded into your own system through A2A integration.

## Sandbox Key Support (for Development)

This agent supports **Sandbox API Keys**, allowing testing without consuming credits.

When using a Sandbox Key:
- No credits are deducted  
- The agent returns a **predefined real supply-chain graph dataset for Tesla**  
- Graph nodes, edges, and attributes follow the final production schema  
- No live graph computation is executed; only the static sample dataset is returned  
- Input validation and structural checks are performed  

This is useful for:
- Front-end visualization testing  
- Building graph-based UI components  
- Verifying schema compatibility before running real analyses  

⚠️ Sandbox datasets are static and do not represent real-time updates.  
Use Production Keys for live graph computation.

For details, see  
[Getting Started Guide → API Keys](../getting-started.md#3-generate-an-a2amcp-key).


## Input Requirements

`agent_id`: `sg_visualization` · MCP tool: `sg_visualization`

| Field | Required | Description |
|-------|----------|-------------|
| `text` | Yes | Target company name (natural language) |

**Example:** "Build the supply graph for Tesla, Inc."


## Integration

| Method | ID / Tool | Documentation |
|--------|-----------|---------------|
| **A2A** | `sg_visualization` | [a2a.md](../a2a_mcp/a2a.md) |
| **MCP** | `sg_visualization` | [mcp.md](../a2a_mcp/mcp.md) |
| **Agent API** | `sg_visualization` | [agent-api.md](../agent-api/agent-api.md) |

Quick examples: [A2A / MCP](../a2a_mcp/quick_example.md) · [Agent API](../agent-api/quick_example.md)

## Errors

Common codes → [Agent API §10](../agent-api/agent-api.md#10-error--status-codes).

Maintainer: info@supplygraph.ai  
License: Proprietary / Internal  
© 2025–2026 SupplyGraph AI. All rights reserved.
