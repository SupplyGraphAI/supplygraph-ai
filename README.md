
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

🤝 **A2A / MCP Protocol** – Standard interfaces for agent discovery, invocation, and interoperability  
👉 **A2A** (Agent-to-Agent): [`docs/a2a.md`](./docs/a2a.md)  
👉 **MCP** (Model Context Protocol): [`docs/mcp.md`](./docs/mcp.md)

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

### Group 1: Supply Chain Risk Prediction Data Engine

- **[Supply Chain Risk Prediction Agent](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/agents/supply_chain_risk_prediction.md)**
- **[Procurement & Inventory Risk Agent](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/agents/procurement_inventor.md)**
- **[Global Supply Dependency Visualization Agent](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/agents/sg_visualization.md)**  
- **[Geographic Concentration Analysis Agent](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/agents/sg_chokepoint.md)**  

### Group 2: Supply Chain Management Kits

- **[Customs Classification Agent](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/agents/tariff_classification.md)**
- **[U.S. Tariff Calculation Agent](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/agents/tariff_calc.md)**  
- **[Due Diligence Agent](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/agents/due_diligence_report.md)**
- **[Corporate Exception Agent](https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/docs/agents/corporate_exception_report.md)**

### Full agent descriptions here:  
https://github.com/SupplyGraphAI/supplygraph-ai/blob/main/agents/index.md

## Integration & Developer Experience

Designed for **fast, standards-based integration** using A2A and MCP.

### Quick Example (Python SDK — A2A Pattern)

Requires the official [A2A Python SDK](https://pypi.org/project/a2a-sdk/) (`pip install a2a-sdk httpx`, Python 3.10+).

```python
import asyncio

import httpx
from a2a.client import ClientConfig, create_client
from a2a.helpers import new_text_message
from a2a.types.a2a_pb2 import GetTaskRequest, Role, SendMessageRequest, TaskState

API_KEY = "YOUR_API_KEY"
AGENT_BASE = "https://agent.supplygraph.ai/a2a/agents/tariff_calc"
TEXT = (
    "Lithium-ion batteries for electric vehicles "
    "manufactured in China"
)

TERMINAL = {
    TaskState.TASK_STATE_COMPLETED,
    TaskState.TASK_STATE_FAILED,
    TaskState.TASK_STATE_CANCELED,
    TaskState.TASK_STATE_REJECTED,
}


async def main() -> None:
    headers = {"Authorization": f"Bearer {API_KEY}"}
    async with httpx.AsyncClient(headers=headers, timeout=60.0) as http:
        client = await create_client(
            AGENT_BASE,
            client_config=ClientConfig(streaming=False, httpx_client=http),
            resolver_http_kwargs={"headers": headers},
            relative_card_path="/",
        )
        try:
            request = SendMessageRequest(
                message=new_text_message(TEXT, role=Role.ROLE_USER),
            )
            task = None
            async for chunk in client.send_message(request):
                if chunk.HasField("task"):
                    task = chunk.task

            if task is None:
                print("No task returned.")
                return

            print(f"Task submitted: {task.id}")

            while task.status.state not in TERMINAL:
                await asyncio.sleep(3)
                task = await client.get_task(GetTaskRequest(id=task.id))
                print("Current status:", TaskState.Name(task.status.state))

            if task.status.state == TaskState.TASK_STATE_COMPLETED:
                print("Final result:")
                print(task.artifacts)
            else:
                print("Task failed or cancelled.")
        finally:
            await client.close()


asyncio.run(main())
```

### Quick Example (Python SDK — MCP Pattern)

```python
import asyncio

from mcp import ClientSession
from mcp.client.streamable_http import streamable_http_client
from mcp.shared.experimental.tasks.helpers import is_terminal
from mcp.types import CallToolResult

API_KEY = "YOUR_API_KEY"
MCP_URL = "https://mcp.supplygraph.ai/mcp"


async def main() -> None:
    headers = {"Authorization": f"Bearer {API_KEY}"}

    async with streamable_http_client(MCP_URL, headers=headers) as (read, write, _):
        async with ClientSession(read, write) as session:
            await session.initialize()

            create = await session.experimental.call_tool_as_task(
                "tariff_calc",
                {
                    "text": (
                        "Lithium-ion batteries for electric vehicles "
                        "manufactured in China"
                    ),
                },
            )
            task_id = create.task.taskId
            print(f"Task submitted: {task_id}")

            async for status in session.experimental.poll_task(task_id):
                print("Current status:", status.status)
                if is_terminal(status.status):
                    break

            result = await session.experimental.get_task_result(
                task_id,
                CallToolResult,
            )
            if result.isError:
                print("Task failed or cancelled.")
            else:
                print("Final result:")
                print(result.content)


asyncio.run(main())
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

