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
