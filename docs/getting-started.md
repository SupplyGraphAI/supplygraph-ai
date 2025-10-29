# Getting Started

Welcome to the **SupplyGraph AI Developer Platform** — the gateway to accessing autonomous supply-chain intelligence through our API, A2A, and MCP interfaces.

This guide walks you through the essentials: creating an account, managing credits, generating an API key, and making your first request.

---

## 1. Create an Account
1. Visit [https://www.supplygraph.ai](https://www.supplygraph.ai)
2. Click **Sign Up** and complete registration.
3. Verify your email address and log in to the dashboard.

Once signed in, you’ll have access to:
- Account and billing settings  
- API key management  
- Usage analytics and credit balance  

---

## 2. Recharge Credits
SupplyGraph AI uses a **credit-based pay-as-you-go** model.  
Each API or A2A request deducts a small number of credits, depending on the agent used.

1. Navigate to **Billing → Recharge** in the dashboard.  
2. Choose your preferred payment method.  
3. After recharge, your balance updates instantly.  

💡 *You only pay for what you use — there are no recurring fees unless you subscribe to enterprise plans.*

---

## 3. Generate an API Key
All API and A2A integrations require authentication via an API key.

1. Go to **Developer Settings → API Keys**  
2. Click **Create New Key**  
3. Copy your key and store it securely  

⚠️ API keys are personal credentials.  
- Never expose them in client-side code or public repositories.  
- Rotate them regularly through the dashboard.

---

## 4. Make Your First API Call

Below is a minimal RESTful example using the **Tariff Calculation Agent**:

```
curl -X POST https://api.supplygraph.ai/v1/agents/tariff-calculation \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "hts_code": "2105.00.20.00",
    "country_of_origin": "CN"
  }'
```

**Response Example:**
```
{
  "status": "success",
  "data": {
    "base_duty": "17%",
    "additional_tariff": "25%",
    "total_effective_rate": "42%"
  },
  "credits_used": 3
}
```

---

## 5. Integration Options

SupplyGraph AI provides three integration modes:

| Mode | Description | Docs |
|------|--------------|------|
| **RESTful API** | Standard HTTPS endpoints for all agents. | [API Reference](./api.md) |
| **A2A (Agent-to-Agent)** | For autonomous workflows and inter-agent communication. | [A2A Protocol](./a2a.md) |
| **MCP (Multi-Channel Protocol)** | For large-scale orchestration across enterprise systems. | *(Coming Soon)* |

---

## 6. Error Handling & Rate Limits

All responses include a unified status field and optional `credits_used` key.  
Common errors:

| Code | Description |
|------|--------------|
| `INVALID_AUTH` | API key missing or expired |
| `INSUFFICIENT_CREDITS` | Not enough credits for this request |
| `RATE_LIMITED` | Too many requests — try again later |
| `INVALID_PAYLOAD` | Missing or malformed parameters |

See [API Reference](./api.md) for the full error schema.

---

## 7. Next Steps

- Explore the available [AI Agents](../README.md#two-groups-of-ai-agents)  
- Learn about [Agent-to-Agent Integration](./a2a.md)  
- Review full API specs in [API Reference](./api.md)

---

<p align="center">
  © 2025 <b>SupplyGraph AI, Inc.</b> All rights reserved.
</p>
