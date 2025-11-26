<p align="center">
  <img src="./docs/sgai_4_black.png" alt="SupplyGraph AI Logo" width="120" height="120"/>
</p>

<h1 align="center">SupplyGraph AI</h1>
<h3 align="center">AI-Native Supply Graph Intelligence Platform</h3>

<p align="center">
  <a href="https://www.supplygraph.ai"><img src="https://img.shields.io/badge/Website-supplygraph.ai-blue?style=flat-square" alt="Website"/></a>
  <a href="#integration--developer-experience"><img src="https://img.shields.io/badge/A2A%2FMCP-Docs-green?style=flat-square" alt="A2A/MCP Docs"/></a>
  <a href="#integration--developer-experience"><img src="https://img.shields.io/badge/Integration-Ready-orange?style=flat-square" alt="Integration Ready"/></a>
  <a href="#security--privacy"><img src="https://img.shields.io/badge/Security-Auditable-critical?style=flat-square" alt="Security"/></a>
  <a href="LICENSE"><img src="https://img.shields.io/badge/License-Proprietary-lightgrey?style=flat-square" alt="License"/></a>
</p>


<p align="center">
Autonomous, auditable, and real-time supply-chain intelligence that maps how companies, products, and geographies connect across the global economy — enabling visibility, resilience, and efficiency <b>without requiring any customer-provided data</b>.
</p>

<p align="center">
<strong>Keywords:</strong> AI-powered supply chain intelligence, tariff calculation, HTS classification, trade compliance, multi-tier supply chain risk analysis, A2A agent platform
</p>

## Documentation Map

This repository contains the official documentation of **SupplyGraph AI** and is structured as follows:

📘 **Getting Started** – Setup, authentication and first request  
👉 [`docs/getting-started.md`](./docs/getting-started.md)

🤝 **A2A / MCP Protocol** – Agent-to-Agent interface & interoperability  
👉 [`docs/a2a.md`](./docs/a2a.md)

🤖 **Agent Library Overview** – All available AI agents  
👉 [`docs/agents/`](./docs/agents/)

📦 **Developer SDK** – Programmatic access for integration  
👉 https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk

🌐 **Official Website** – Product, use cases & demo  
👉 https://www.supplygraph.ai


## Table of Contents
1. [Introduction](#introduction)  
2. [What We Do](#what-we-do)  
3. [Why It Matters](#why-it-matters)  
4. [How It Works](#how-it-works)  
5. [Two Groups of AI Agents](#two-groups-of-ai-agents)  
6. [Integration & Developer Experience](#integration--developer-experience)  
7. [Who Uses SupplyGraph AI](#who-uses-supplygraph-ai)  
8. [Why We’re Different](#why-were-different)  
9. [Real Results Across Industries](#real-results-across-industries)  
10. [Security & Privacy](#security--privacy)  
11. [About SupplyGraph AI](#about-supplygraph-ai)  
12. [Contact](#contact)



## Introduction
**SupplyGraph AI** delivers real-time, AI-powered global supply graph intelligence for **multi-tier supply chain risk analysis, tariff calculation, HTS classification, and trade compliance** — an AI-native risk infrastructure that reveals multi-hop visibility with auditable analytics for enterprises, financial institutions, and public stakeholders.



## What We Do
We map how products, companies, and geographies connect across extended supply networks, surfacing risks and validated alternatives in real time.  
Our AI-native graph infrastructure powers multi-hop visibility so teams can see, simulate, and secure complex value chains without adding tooling overhead.



## Why It Matters
Legacy solutions rarely reach beyond Tier 1–2 and depend heavily on static, probabilistic data or user-uploaded supplier lists.  
SupplyGraph AI eliminates these blind spots, exposing 10+ tiers of verified enterprise-product relationships with explainable, source-linked evidence.



## How It Works
We maintain a continuously updated graph of hundreds of millions of enterprise records and millions of product nodes.  
Each relationship is tied to live signals and an auditable evidence chain.  

✅ **No customer data required** — our enterprise-centric design removes the need for supplier uploads, minimizing disclosure risk while accelerating time-to-value.



## Two Groups of AI Agents

> 📚 Full agent descriptions and usage examples are available here:  
> 👉 [Browse all agents](./docs/agents/)

### Group 1: Automation & Efficiency Agents
Automate repetitive and time-consuming workflows:
- **[Customs Classification Agent](./docs/agents/tariff_classification.md)** — Maps products to correct HS/HTS codes.  
- **[Tariff Calculation Agent](./docs/agents/tariff_calc.md)** — Calculates U.S. duty rates and additional tariffs.  
- **Tariff Monitoring Agent** — Detects tariff changes and sends actionable alerts.  
- **[Due Diligence Agent](./docs/agents/due_diligence_report.md)** — Generates multi-industry company intelligence in minutes.
- **[Corporate Exception Agent](./docs/agents/corporate_exception_report.md)** — Real-time automated corporate exception monitoring.

### Group 2: Data Intelligence & Supply Graph Agents
Deliver enterprise-centered global supply–product graph intelligence:
- **[Global Supply Dependency Visualization Agent](./docs/agents/sg_visualization.md)** — Visualizes multi-tier supply dependencies across global companies and products.  
- **[Single-Country Concentration Risk Agent](./docs/agents/sg_chokepoint.md)** — Identifies hidden geographic dependencies and quantifies country-level exposure.  
- **Tier-1 Supplier Discovery Agent** — Finds qualified, low-risk Tier-1 suppliers worldwide within minutes.  
- **Regional Substitution Recommendation Agent** — Recommends practical regional supplier alternatives to strengthen resilience.



## Integration & Developer Experience
Our platform is designed for **fast, standards-based integration**:
- **Agent-to-Agent (A2A) and MCP Interfaces** — Built on industry standards for interoperability.  
- **Integration Time:** Typically **15–30 minutes** for most enterprise environments.  

### Quick Example (Python SDK — A2A Pattern)

```python
from supplygraphai_a2a_sdk import AgentClient
import time

client = AgentClient(api_key="YOUR_API_KEY")

# 1. Submit a task to the Tariff Calculation Agent
run_response = client.run(
    agent_id="tariff_calc",
    text="Lithium-ion batteries for electric vehicles manufactured in China"
)

task_id = run_response.get("task_id")
print(f"Task submitted: {task_id}")

# 2. Check task status
status = "PENDING"
while status not in ("COMPLETED", "FAILED"):
    time.sleep(3)
    status_response = client.status(
        agent_id="tariff_calc",
        task_id=task_id
    )
    status = status_response.get("status")
    print("Current status:", status)

# 3. Retrieve final results
if status == "COMPLETED":
    result = client.results(
        agent_id="tariff_calc",
        task_id=task_id
    )
    print("Final result:")
    print(result)
else:
    print("Task failed or cancelled.")
```
> This example demonstrates the complete A2A (Agent-to-Agent) lifecycle
> — **run** → **status** → **results** — for **HTS-based tariff calculation and U.S. trade compliance analysis**
> using SupplyGraph AI.

**Ideal for:**  
AI engineers, data-science teams, supply-chain developers, and risk-automation architects building intelligent enterprise workflows.

For full setup, authentication, and A2A/MCP key usage, see the [Getting Started Guide](./docs/getting-started.md).  
For endpoint specifications, refer to the [A2A Protocol](./docs/a2a.md).

📦 For programmatic integration, SDKs and sample code, visit:  
👉 **[SupplyGraphAI/supplygraphai_a2a_sdk](https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk)**

## Who Uses SupplyGraph AI
**SupplyGraph AI** is built for organizations and developers who depend on complex, global supply networks:  

- **Manufacturing & Automotive Enterprises** — Manage supplier dependencies, sourcing diversification, and tariff impact.  
- **Energy, Electronics & Industrial Companies** — Monitor upstream material risks and regional concentration exposure.  
- **Retail & Consumer Goods Brands** — Evaluate supply resilience, compliance, and substitution opportunities.  
- **Consulting & Risk Advisory Firms** — Embed automated due diligence and risk intelligence into client workflows.  
- **Financial Institutions & Investors** — Analyze network-based exposure for portfolio or credit risk modeling.  
- **Public Sector & Research Institutes** — Model supply interdependencies, concentration trends, and policy scenarios.  



## Why We’re Different
- **10+ tier visibility** — Exposes deep-tier dependencies unseen by legacy tools.  
- **Real-time updates** — Continuously refreshed global graph data.  
- **Auditable analytics** — Every output backed by verifiable evidence.  
- **Zero data dependency** — No customer-provided data required.  
- **Zero hallucination** — Fully explainable, policy-aware AI reasoning.



## Real Results Across Industries

| Sector | Impact Example |
|:-------|:----------------|
| **Supply Chain Management** | Global 500 client reduced tariff filing time from hours to minutes with our Tariff Monitoring Suite. |
| **Consulting** | Embedded our Due Diligence Suite into advisory workflows, cutting analysis time by 90%. |
| **Financial Services** | Extended visibility from Tier-3 to Tier-10 suppliers through A2A integration. |
| **Research** | Enabled new empirical studies with verified supply-chain datasets. |
| **Government** | Supported policy simulations and concentration analytics for regulatory planning. |



## Security & Privacy
Insights are built from enterprise–product relationships and open signals.  
Customer data is never required nor stored.  
Every analytic output maintains a **verifiable evidence trail**, ensuring transparency, compliance, and audit readiness.



## About SupplyGraph AI
**SupplyGraph AI** redefines how the world understands and secures supply chains.  
Through autonomous graph intelligence, we empower organizations to predict risks, optimize operations, and act with confidence — all without sharing private data.



## Contact
For partnerships, integrations, or research collaborations:  
📧 **info@supplygraph.ai**  
🌐 [https://www.supplygraph.ai](https://www.supplygraph.ai)  



## More about SupplyGraph AI

📘 Main Documentation: https://github.com/SupplyGraphAI/supplygraph-ai  
🚀 Getting Started Guide: ./docs/getting-started.md  
🤝 A2A / MCP Protocol: ./docs/a2a.md  
🤖 Agents Library: ./docs/agents/  
📦 Developer SDK: https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk  
🌐 Official Website: https://www.supplygraph.ai  

**Key Focus Areas:**  
AI-powered supply chain, tariff calculation, HTS classification, trade compliance, multi-tier risk analysis, agent-to-agent systems.



<p align="center">
  © 2025 <b>SupplyGraph AI, Inc.</b> All rights reserved.  
</p>
