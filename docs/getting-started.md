# Getting Started

Welcome to the **SupplyGraph AI Developer Platform** — the gateway to accessing autonomous supply-chain intelligence through our API, A2A, and MCP interfaces.

This guide walks you through the essentials: creating an account, managing credits, generating an API key, and making your first request.



## 1. Create an Account
1. Visit [https://www.supplygraph.ai](https://www.supplygraph.ai)
2. Click **Sign Up** and complete registration.
3. Verify your email address and log in to the dashboard.

Once signed in, you’ll have access to:
- Account and billing settings  
- A2A/MCP key management  
- Usage analytics and credit balance  



## 2. Recharge Credits
SupplyGraph AI uses a **credit-based pay-as-you-go** model.  
Each A2A or MCP request deducts a small number of credits, depending on the agent used.

1. Navigate to **Billing → Recharge** in the dashboard.  
2. Choose your preferred payment method.  
3. After recharge, your balance updates instantly.  

💡 *You only pay for what you use — there are no recurring fees unless you subscribe to enterprise plans.*



## 3. Generate an A2A/MCP Key
All A2A and MCP integrations require authentication via an API key.

1. Go to **Developer Settings → A2A/MCP Keys**  
2. Click **Create New Key**  
3. Copy your key and store it securely  

⚠️ A2A/MCP keys are personal credentials.  
- Never expose them in client-side code or public repositories.  
- Rotate them regularly through the dashboard.



## 4. Make Your First A2A Call

Below is a minimal example using the **Tariff classification Agent**:

### Step 1: Submit the Initial Request
**Purpose:** Start a new task by providing a product description.
```bash
curl -X POST https://agent.supplygraph.ai/v1/agents/tariff_classification/run \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{"text":"Cotton T-shirts for women, 100%cotton, made in Mexico", "stream": true}'
```

**Example Response:**
```
data: {"event": "stream", "data": {"task_id": "<system-generated-task-id>", "agent": "tariff_classification", "stage": "interpreting", "code": "THINKING", "progress": 0, "reasoning": ["Attempting to extract the HTS code and country of origin from the input text."], "timestamp": "2025-11-10T09:51:58.457382+00:00", "is_final": false}}

data: {"event": "stream", "data": {"task_id": "<system-generated-task-id>", "agent": "tariff_classification", "stage": "interpreting", "code": "THINKING", "progress": 0, "reasoning": ["Matched 20 related HTS codes. Assessing their relevance and matching accuracy."], "timestamp": "2025-11-10T09:52:06.518056+00:00", "is_final": false}}

data: {"event": "stream", "data": {"task_id": "<system-generated-task-id>", "agent": "tariff_classification", "stage": "interpreting", "code": "THINKING", "progress": 0, "reasoning": ["Processing in progress — please hang tight."], "timestamp": "2025-11-10T09:52:14.077191+00:00", "is_final": false}}

data: {"success": true, "code": "TASK_ACCEPTED", "message": "Task accepted and queued for execution.", "data": {"task_id": "<system-generated-task-id>", "agent": "tariff_classification", "stage": "executing", "code": "TASK_ACCEPTED", "progress": 0, "reasoning": [], "timestamp": "2025-11-10T09:52:34.557890+00:00", "is_final": true, "content": "task accepted"}, "metadata": {"agent": "tariff_classification", "timestamp": "2025-11-10T09:52:34.557922+00:00"}, "errors": null}

event: end
data: [DONE]
```

### Step 2: Check Task Status
**Purpose:** Query the current status of the task using the same `task_id`.
```bash
curl -N -X POST https://agent.supplygraph.ai/api/v1/agents/tariff_classification/run \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer <YOUR_API_KEY>" \
     -d '{"text": "", "stream": true, "task_id": "<system-generated-task-id>", "mode": "status"}'
```

**Example Response:**
```text
data: {"success": true, "code": "TASK_COMPLETED", "message": "Task completed successfully.", "data": {"task_id": "<system-generated-task-id>", "agent": "tariff_classification", "stage": "completed", "code": "TASK_COMPLETED", "progress": 100, "reasoning": [], "timestamp": "2025-11-10T10:03:50.442559+00:00", "is_final": true, "content": ""}, "metadata": {"agent": "tariff_classification", "timestamp": "2025-11-10T10:03:50.442601+00:00", "credits_used": 0.0}, "errors": null}

event: end
data: [DONE]
```

### Step 3: Retrieve the Final Results
**Purpose:** Once the task is complete, request the final content.
```bash
curl -N -X POST https://agent.supplygraph.ai/api/v1/agents/tariff_classification/run \
     -H "Content-Type: application/json" \
     -H "Authorization: Bearer <YOUR_API_KEY>" \
     -d '{"text": "", "stream": true, "task_id": "<system-generated-task-id>", "mode": "results"}'
```

**Example Response:**
```text
data: {"success": true, "code": "TASK_COMPLETED", "message": "Task completed successfully.", "data": {"task_id": "<system-generated-task-id>", "agent": "tariff_classification", "stage": "completed", "code": "TASK_COMPLETED", "progress": 100, "reasoning": [], "timestamp": "2025-11-10T10:04:49.748779+00:00", "is_final": true, "content": "##Best match:\nHTS code: 6109.10.00.40\n\n##Other possible HTS codes include:\n-6109.10.00.70: This code is for 'T-shirts, singlets, tank tops and similar garments, knitted or crocheted > Of cotton > Women's or girls' > Other > Other'. While the primary description is for T-shirts, this code could apply if the T-shirts have features that do not fit the more specific T-shirt category, but it is less likely given the straightforward description.\n-6114.20.00.10: This code is for 'Other garments, knitted or crocheted > Of cotton > Tops > Women's or girls''. While the product is specifically a T-shirt, this code could be considered if the T-shirts are categorized more broadly as tops, but it is less specific than the primary code."}, "metadata": {"agent": "tariff_classification", "timestamp": "2025-11-10T10:04:49.748823+00:00", "credits_used": 10}, "errors": null}

event: end
data: [DONE]
```


## 5. Integration Options

SupplyGraph AI provides three integration modes:

| Mode | Description | Docs |
|------|--------------|------|
| **A2A (Agent-to-Agent)** | For autonomous workflows and inter-agent communication. | [A2A Protocol](./a2a.md) |
| **MCP (Multi-Channel Protocol)** | For large-scale orchestration across enterprise systems. | *(Coming Soon)* |



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



## 7. Next Steps

- Explore the available [AI Agents](../README.md#two-groups-of-ai-agents)  
- Learn about [Agent-to-Agent Integration](./a2a.md)  



<p align="center">
  © 2025 <b>SupplyGraph AI, Inc.</b> All rights reserved.
</p>
