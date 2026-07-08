# Getting Started

Welcome to the **SupplyGraph AI Developer Platform** — the gateway to accessing autonomous supply-chain intelligence through A2A, MCP, or Agent API.

This guide walks you through the essentials: creating an account, managing credits, generating an API key, and choosing an integration path.



## 1. Create an Account
1. Visit [https://www.supplygraph.ai](https://www.supplygraph.ai)
2. Click **Sign Up** and complete registration.  
   If an invitation code is required, please contact us at info@supplygraph.ai.
3. Verify your email address and sign in to the dashboard to begin.

Once signed in, you'll have access to:
- Account and billing settings  
- A2A/MCP key management  
- Usage analytics and credit balance  



## 2. Recharge Credits
SupplyGraph AI uses a **credit-based pay-as-you-go** model.  
Each agent invocation deducts credits depending on the agent used.

1. Navigate to **Billing → Recharge** in the dashboard.  
2. Choose your preferred payment method.  
3. After recharge, your balance updates instantly.  

💡 *You only pay for what you use — there are no recurring fees unless you subscribe to enterprise plans.*



## 3. Generate an A2A/MCP Key
All integrations require authentication via an API key.

1. Go to **Developer Settings → A2A/MCP Keys**  
2. Click **Create New Key**  
3. Copy your key and store it securely  

⚠️ API keys are personal credentials.  
- Never expose them in client-side code or public repositories.  
- Rotate them regularly through the dashboard.

### 3.1 Sandbox API Keys (For Testing & Development)

SupplyGraph AI provides **Sandbox API Keys** that allow developers to test integrations safely — **without consuming credits** — while still returning valid, schema-compliant responses.

A Sandbox Key behaves like a Production Key in terms of request format, authentication, and task lifecycle (`run → status → results`).  
However, **the returned data does not come from real-time agent computation**. Instead, each agent returns preconfigured **mock data** or **sample real data**, depending on the agent type.

#### What Does a Sandbox Key Return?

Sandbox responses fall into two categories depending on the agent:

**A. Agents Returning Mock Data**  
These agents return **fully simulated outputs**, designed to match the official output schema but **not based on real data**:
- **HTS Code Classification Agent**
- **Tariff Calculation Agent**

These responses allow developers to validate:
- Request/response structure
- Task status polling logic
- Rendering logic on the client side

**B. Agents Returning Predefined *Real* Sample Data**  
These agents return **fixed, realistic outputs** derived from **preset sample companies** (e.g., Tesla or BYD).  
These are not user-specific and do not reflect real-time queries, but they are **authentic examples of actual agent outputs**.

Agents using predefined real sample data:
- **Corporate Due Diligence Report Agent** → returns a preset **BYD or Tesla** due-diligence report
- **Corporate Exception Report Agent** → returns a preset **BYD or Tesla** exception report
- **Supply Chain Graph Visualization Agent** → returns a preset **Tesla supply-chain graph**
- **Geographic Concentration Analysis Agent** → returns a preset **Tesla regional concentration dataset**

These allow developers to:
- Preview the real structure of enterprise-level reports  
- Test integration with full-severity risk outputs  
- Validate UI rendering for complex, nested datasets  


#### Key Properties of Sandbox Keys

- **No credit consumption**  
- **No real agent computation is performed**  
- Responses are **deterministic**, ensuring consistent integration testing  
- Predefined real-data samples reflect **true report formats**, not synthetic hallucinations  
- Switching to a Production Key enables full, real-time agent capabilities


#### How to Create a Sandbox Key

1. Navigate to **Developer Settings → A2A/MCP Keys**  
2. Click **Create Sandbox Key**  
3. Give it a name such as `local-dev`, `ci-test`, or `mock-environment`  

**Recommended Usage**
- **Sandbox Key** → local development, CI/CD pipelines, front-end integration  
- **Production Key** → staging environments, production workloads  




## 4. Choose Your Integration

SupplyGraph AI exposes three integration surfaces. Pick the one that matches your client stack:

| Integration | Endpoint | Best for | Documentation |
|-------------|----------|----------|---------------|
| **A2A** (recommended) | `https://agent.supplygraph.ai/a2a` | Agent orchestrators, A2A-native clients | [A2A Protocol](./a2a_mcp/a2a.md) · [Quick Example](./a2a_mcp/quick_example.md) |
| **MCP** (recommended) | `https://mcp.supplygraph.ai/mcp` | Cursor, Claude Desktop, MCP-native IDEs | [MCP Protocol](./a2a_mcp/mcp.md) · [Quick Example](./a2a_mcp/quick_example.md) |
| **Agent API** | `https://agent.supplygraph.ai/api/v1/agents/{agent_id}/run` | Traditional REST (`run` / `status` / `results`) | [Agent API](./agent-api/agent-api.md) · [Quick Example](./agent-api/quick_example.md) |

All three use the same API key: `Authorization: Bearer {api_key}`.

For a detailed comparison and SDK options, see the [Integration Guide](./a2a_mcp/integration.md).

### Resource Discovery (ARD)

Broader resource discovery (agents, MCP tools, documentation links) is published via **Agent Resource Discovery (ARD)**:

```
https://supplygraph.ai/.well-known/ai-catalog.json
```

See [A2A Protocol § ARD](./a2a_mcp/a2a.md) for details.



## 5. Error Handling & Rate Limits

All responses include a unified status field and optional `credits_used` key.  
Common errors:

| Code | Description |
|------|--------------|
| `UNAUTHORIZED` | API key missing or expired |
| `INSUFFICIENT_CREDITS` | Not enough credits for this request |
| `RATE_LIMITED` | Too many requests — try again later |
| `INVALID_REQUEST` | Request is outside the current agent's scope |

Protocol-specific error handling: [A2A](./a2a_mcp/a2a.md) · [MCP](./a2a_mcp/mcp.md) · [Agent API](./agent-api/agent-api.md)


## 6. Next Steps

- Explore the available [AI Agents](../README.md#two-groups-of-ai-agents)  
- Run a quick example: [A2A / MCP](./a2a_mcp/quick_example.md) or [Agent API](./agent-api/quick_example.md)  
- Read the [Integration Guide](./a2a_mcp/integration.md) to choose the right protocol


## 7. Related Documentation

| Resource | Link |
|----------|------|
| Live Docs (GitHub Pages) | https://supplygraphai.github.io/supplygraph-ai/ |
| Agent Library | https://supplygraphai.github.io/supplygraph-ai/agents/ |
| ARD Catalog | https://supplygraph.ai/.well-known/ai-catalog.json |
| Client SDKs | [a2a-sdk](https://pypi.org/project/a2a-sdk/) · [mcp](https://pypi.org/project/mcp/) · [supplygraphai_a2a_sdk](https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk) (Agent API, optional) |
| Official Website | https://www.supplygraph.ai |


<p align="center">
  © 2025–2026 <b>SupplyGraph AI, Inc.</b> All rights reserved.
</p>
