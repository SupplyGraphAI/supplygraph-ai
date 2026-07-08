
<p align="center">
  <img src="./docs/sgai_4_black.png" alt="SupplyGraph AI Logo" width="120" height="120"/>
</p>

<h1 align="center">SupplyGraph AI</h1>
<h3 align="center">AI-Native Supply Graph Intelligence Platform</h3>

<p align="center">
  <a href="https://www.supplygraph.ai"><img src="https://img.shields.io/badge/Website-supplygraph.ai-blue?style=flat-square" alt="Website"/></a>
  <a href="https://supplygraphai.github.io/supplygraph-ai/"><img src="https://img.shields.io/badge/GitHub%20Pages-Live-brightgreen?style=flat-square" alt="GitHub Pages"/></a>
  <a href="#integration--developer-experience"><img src="https://img.shields.io/badge/A2A%2FMCP-Docs-green?style=flat-square" alt="A2A/MCP Docs"/></a>
  <a href="#integration--developer-experience"><img src="https://img.shields.io/badge/Integration-Ready-orange?style=flat-square" alt="Integration Ready"/></a>
  <a href="#security--privacy"><img src="https://img.shields.io/badge/Security-Auditable-critical?style=flat-square" alt="Security"/></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-Proprietary-lightgrey?style=flat-square" alt="License"/></a>
</p>

<p align="center">
📢 <a href="https://github.com/SupplyGraphAI/supplygraph-ai/issues/4">Launch, Roadmap & Live Agent Demos</a>
</p>

**SupplyGraph AI** is an agentic supply-chain intelligence platform. It exposes autonomous agents for **HTS/HS classification**, **U.S. tariff calculation**, **trade compliance**, **due diligence**, and **multi-tier supply chain risk analysis** — via standard **A2A**, **MCP**, and a traditional **Agent API**. Outputs are explainable and auditable; **no customer supplier lists or proprietary uploads are required**.

> **Organization hub:** [SupplyGraphAI/SupplyGraphAI](https://github.com/SupplyGraphAI/SupplyGraphAI) · **Product & use cases:** [supplygraph.ai](https://www.supplygraph.ai)

## At a Glance

| Resource | URL |
|----------|-----|
| **A2A service** | `https://agent.supplygraph.ai/a2a` |
| **A2A agent base** | `https://agent.supplygraph.ai/a2a/agents/{agent_id}` |
| **MCP server** | `https://mcp.supplygraph.ai/mcp` |
| **Agent API** | `https://agent.supplygraph.ai/api/v1/agents/{agent_id}/run` |
| **API keys & Console** | [supplygraph.ai/zk_chat_os/dashboard/dashboard.html](https://supplygraph.ai/zk_chat_os/dashboard/dashboard.html) |
| **ARD catalog** (machine discovery) | `https://supplygraph.ai/.well-known/ai-catalog.json` |
| **Live docs (GitHub Pages)** | [supplygraphai.github.io/supplygraph-ai/](https://supplygraphai.github.io/supplygraph-ai/) |

Authentication for all surfaces: `Authorization: Bearer {api_key}`.

## Agent Catalog

Each agent uses the same **`agent_id`** for A2A and MCP tool invocation. Per-agent inputs, demos, and behavior → [`docs/agents/`](./docs/agents/index.md).

| Agent | `agent_id` | Capability |
|-------|------------|------------|
| Customs Classification | `tariff_classification` | Map product descriptions to HS / HTS codes |
| U.S. Tariff Calculation | `tariff_calc` | Calculate U.S. duties, Chapter 99, and trade measures |
| Due Diligence | `due_diligence_report` | Structured company intelligence reports |
| Corporate Exception Report | `corporate_exception_report` | Continuous corporate anomaly monitoring |
| Supply Chain Risk Prediction | `supply_chain_risk_prediction` | Multi-tier event impact and risk propagation |
| Enterprise Supply Graph | `sg_visualization` | Multi-tier company-centric supply graph |
| Geographic Concentration | `sg_chokepoint` | Country / regional over-concentration risk |
| Procurement Risk (Supplier) | `supply_chain_risk_prediction_cross_company` | Key supplier dependency monitoring |
| Procurement Risk (Component) | `supply_chain_risk_prediction_cross_product` | Key component dependency monitoring |

## Documentation Map

| Topic | Link |
|-------|------|
| Getting Started — account, API keys, Sandbox | [`docs/getting-started.md`](./docs/getting-started.md) |
| Integration Guide — A2A vs MCP vs Agent API | [`docs/a2a_mcp/integration.md`](./docs/a2a_mcp/integration.md) |
| A2A Protocol | [`docs/a2a_mcp/a2a.md`](./docs/a2a_mcp/a2a.md) |
| MCP Protocol | [`docs/a2a_mcp/mcp.md`](./docs/a2a_mcp/mcp.md) |
| Agent API (traditional REST) | [`docs/agent-api/agent-api.md`](./docs/agent-api/agent-api.md) |
| Quick Examples | [A2A / MCP](./docs/a2a_mcp/quick_example.md) · [Agent API](./docs/agent-api/quick_example.md) |
| Agent Library & live demos | [`docs/agents/index.md`](./docs/agents/index.md) |
| Client SDKs | [a2a-sdk](https://pypi.org/project/a2a-sdk/) · [mcp](https://pypi.org/project/mcp/) · [supplygraphai_a2a_sdk](https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk) (Agent API, optional) |

## Integration & Developer Experience

| Integration | Endpoint | Docs | Quick Example |
|-------------|----------|------|---------------|
| **A2A** (recommended) | `https://agent.supplygraph.ai/a2a` | [`a2a.md`](./docs/a2a_mcp/a2a.md) | [`quick_example.md`](./docs/a2a_mcp/quick_example.md) |
| **MCP** (recommended) | `https://mcp.supplygraph.ai/mcp` | [`mcp.md`](./docs/a2a_mcp/mcp.md) | [`quick_example.md`](./docs/a2a_mcp/quick_example.md) |
| **Agent API** | `.../api/v1/agents/{agent_id}/run` | [`agent-api.md`](./docs/agent-api/agent-api.md) | [`quick_example.md`](./docs/agent-api/quick_example.md) |

New integrations: [`docs/getting-started.md`](./docs/getting-started.md) → [`docs/a2a_mcp/integration.md`](./docs/a2a_mcp/integration.md) → run a [quick example](./docs/a2a_mcp/quick_example.md).

## Developer FAQ

**What is SupplyGraph AI?**  
An AI-native supply graph platform that connects companies, products, and geographies for trade compliance and multi-tier risk intelligence — exposed as discoverable agents, not static REST endpoints only.

**How do I integrate?**  
Choose **A2A** (agent orchestrators), **MCP** (Cursor, Claude Desktop, MCP-native IDEs), or **Agent API** (traditional `run` / `status` / `results`). All three use the same API key from the [Console](https://supplygraph.ai/zk_chat_os/dashboard/dashboard.html).

**Which SDK should I use?**  
**[a2a-sdk](https://pypi.org/project/a2a-sdk/)** for A2A · **[mcp](https://pypi.org/project/mcp/)** for MCP · **[supplygraphai_a2a_sdk](https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk)** only if you want the Agent API REST wrapper.

**How do AI systems discover your agents?**  
Publish ARD at `https://supplygraph.ai/.well-known/ai-catalog.json` — lists agents, MCP server, docs, and A2A registry URLs. See [A2A § ARD](./docs/a2a_mcp/a2a.md).

**Do I need to upload supplier lists?**  
No. Agents operate on SupplyGraph's continuously updated global supply graph. Sandbox keys return mock or sample data for integration testing — see [Getting Started § Sandbox](./docs/getting-started.md#31-sandbox-api-keys-for-testing--development).

## Security & Privacy

No customer data is required or stored. All output is backed by a verifiable evidence chain.

## Contact

info@supplygraph.ai · https://www.supplygraph.ai

© 2025–2026 SupplyGraph AI, Inc. All rights reserved.
