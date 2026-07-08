## Integration & Developer Experience

Designed for **fast integration** using the **Agent API** (traditional REST).

See the full specification: [agent-api.md](./agent-api.md)

### Quick Example (curl — Agent API)

```bash
# 1. Start task
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/tariff_classification/run" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"text": "Cotton T-shirts for women, 100% cotton, made in Mexico", "stream": false}'

# 2. Poll status (replace <task-id>)
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/tariff_classification/run" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"mode": "status", "task_id": "<task-id>", "stream": false}'

# 3. Get results
curl -X POST "https://agent.supplygraph.ai/api/v1/agents/tariff_classification/run" \
  -H "Authorization: Bearer YOUR_API_KEY" \
  -H "Content-Type: application/json" \
  -d '{"mode": "results", "task_id": "<task-id>", "stream": false}'
```

### Quick Example (Python — Agent API SDK, optional)

Requires [supplygraphai_a2a_sdk](https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk).

```python
import time

from supplygraphai_a2a_sdk import AgentClient

client = AgentClient(api_key="YOUR_API_KEY")

run_response = client.run(
    agent_id="tariff_calc",
    text="Lithium-ion batteries for electric vehicles manufactured in China",
)
task_id = run_response.get("task_id")
print(f"Task submitted: {task_id}")

status = "PENDING"
while status not in ("COMPLETED", "FAILED"):
    time.sleep(3)
    status_response = client.status(agent_id="tariff_calc", task_id=task_id)
    status = status_response.get("status")
    print("Current status:", status)

if status == "COMPLETED":
    result = client.results(agent_id="tariff_calc", task_id=task_id)
    print("Final result:")
    print(result)
else:
    print("Task failed or cancelled.")
```

> This SDK wraps **Agent API** only. For standard A2A or MCP, see [../a2a_mcp/quick_example.md](../a2a_mcp/quick_example.md).
