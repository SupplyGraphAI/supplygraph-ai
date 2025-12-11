
<p align="center">
  <img src="./docs/sgai_4_black.png" alt="SupplyGraph AI Logo" width="120" height="120"/>
</p>

<h1 align="center">SupplyGraph AI</h1>
<h3 align="center">AI-Native Supply Graph Intelligence Platform</h3>

An Agentic AI platform for **customs classification, tariff calculation, trade compliance, and deep-tier supply chain risk intelligence**.

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
<p align="center">
👉 <a href="https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/agents/index.md">Explore all SupplyGraph AI Agents</a>
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

🤖 **Agent Library Overview**  
👉 https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/agents/index.md

📦 **Developer SDK** – Programmatic access for integration  
👉 https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk

📄 **Live Documentation Site (GitHub Pages)** – Fast-loading, shareable, searchable docs  
👉 https://supplygraphai.github.io/supplygraph-ai/

🌐 **Official Website** – Product, use cases & demo  
👉 https://www.supplygraph.ai

## Table of Contents
1. Introduction  
2. What We Do  
3. Why It Matters  
4. How It Works  
5. Two Groups of AI Agents  
6. Integration & Developer Experience  
7. Who Uses SupplyGraph AI  
8. Why We’re Different  
9. Real Results Across Industries  
10. Security & Privacy  
11. About SupplyGraph AI  
12. Contact  

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

### Group 1: Automation & Efficiency Agents

- **[Customs Classification Agent](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/agents/tariff_classification.md)** – Maps products to correct HS/HTS codes  
- **[U.S. Tariff Calculation Agent](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/agents/tariff_calc.md)** – Calculates U.S. duty rates and additional tariffs  
- **[Due Diligence Agent](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/agents/due_diligence_report.md)** – Generates structured company intelligence  
- **[Corporate Exception Agent](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/agents/corporate_exception_report.md)** – Real-time automated corporate exception monitoring  

👉 Full agent descriptions here:  
https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/agents/index.md

### Group 2: Data Intelligence & Supply Graph Agents

- **[Global Supply Dependency Visualization Agent](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/agents/sg_visualization.md)**  
- **[Geographic Concentration Analysis Agent](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/agents/sg_chokepoint.md)**  

👉 Details & demos available in the Agent Hub:  
https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/agents/index.md

## Integration & Developer Experience

Designed for **fast, standards-based integration** using A2A and MCP.

### Quick Example (Python SDK — A2A Pattern)

```python
from supplygraphai_a2a_sdk import AgentClient
import time

client = AgentClient(api_key="YOUR_API_KEY")

run_response = client.run(
    agent_id="tariff_calc",
    text="Lithium-ion batteries for electric vehicles manufactured in China"
)

task_id = run_response.get("task_id")
print(f"Task submitted: {task_id}")

status = "PENDING"
while status not in ("COMPLETED", "FAILED"):
    time.sleep(3)
    status_response = client.status(
        agent_id="tariff_calc",
        task_id=task_id
    )
    status = status_response.get("status")
    print("Current status:", status)

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

## Who Uses SupplyGraph AI

- Manufacturing & Automotive companies  
- Energy, Electronics & Industrial groups  
- Retail & Consumer Brands  
- Consulting & Risk Advisory firms  
- Financial Institutions & Investors  
- Public Sector & Research organizations  

## Why We’re Different

- 10+ tier visibility  
- Real-time updates  
- Auditable evidence  
- Zero customer data required  
- Explainable, policy-aware AI  

## Real Results Across Industries

Supply Chain – Reduced tariff filing time from hours to minutes  
Consulting – Cut analysis time by up to 90%  
Finance – Extended visibility from Tier-3 to Tier-10  
Research – Enabled empirical network modeling  
Government – Supported policy and risk simulations  

## Security & Privacy

No customer data is required or stored.  
All output is backed by a verifiable evidence chain.

## About SupplyGraph AI

SupplyGraph AI redefines how the world understands and secures supply chains.  
Through autonomous graph intelligence, organizations can predict risk and act with confidence.

## Contact

info@supplygraph.ai  
https://www.supplygraph.ai

## More about SupplyGraph AI

Getting Started  
https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/getting-started.md

Agent Hub  
https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/agents/index.md

Python A2A SDK  
https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk

Website  
https://www.supplygraph.ai


© 2025 SupplyGraph AI, Inc. All rights reserved.

