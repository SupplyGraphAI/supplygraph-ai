# Integration Guide

Choose the integration surface that best fits your client stack. All three use the same SupplyGraph API key (`Authorization: Bearer {api_key}`).

Obtain API keys from the [SupplyGraph Console](https://supplygraph.ai/zk_chat_os/dashboard/dashboard.html). See [Getting Started](../getting-started.md) for account setup and Sandbox keys.

---

## Endpoints

| Integration | Endpoint | Documentation | Quick Example |
|-------------|----------|---------------|---------------|
| **A2A** (recommended) | `https://agent.supplygraph.ai/a2a` | [a2a.md](./a2a.md) | [quick_example.md](./quick_example.md) |
| **MCP** (recommended) | `https://mcp.supplygraph.ai/mcp` | [mcp.md](./mcp.md) | [quick_example.md](./quick_example.md) |
| **Agent API** | `https://agent.supplygraph.ai/api/v1/agents/{agent_id}/run` | [agent-api.md](../agent-api/agent-api.md) | [quick_example.md](../agent-api/quick_example.md) |

---

## Resource Discovery (ARD)

Broader resource discovery (agents, MCP tools, documentation links) is published via **Agent Resource Discovery (ARD)**:

```
https://supplygraph.ai/.well-known/ai-catalog.json
```

See [A2A Protocol § ARD](./a2a.md) for the catalog schema and usage.

---

## SDK Options

| SDK | Integration | Status |
|-----|-------------|--------|
| **[a2a-sdk](https://pypi.org/project/a2a-sdk/)** | A2A | ✅ Recommended |
| **[mcp](https://pypi.org/project/mcp/)** | MCP | ✅ Recommended |
| **[supplygraphai_a2a_sdk](https://github.com/SupplyGraphAI/supplygraphai_a2a_sdk)** | Agent API only | Optional wrapper |

For runnable examples with official SDKs, see [quick_example.md](./quick_example.md) (A2A/MCP) and [../agent-api/quick_example.md](../agent-api/quick_example.md) (Agent API).

---

## Protocol Summary

| | **A2A** | **MCP** | **Agent API** |
|---|---------|---------|---------------|
| **Best for** | Agent orchestrators, A2A-native clients | MCP-native IDEs and assistants (Cursor, Claude Desktop, etc.) | Traditional REST clients, legacy integrations |
| **Discovery** | Agent Card + Registry (ARD) | `tools/list` | `GET .../manifest` |
| **Invocation** | `message:send` / `message:stream` | `tools/call` → Tasks | `POST .../run` (`mode=run`) |
| **Input** | Message parts (text / file / data) | Structured JSON (`inputSchema`) | JSON body (`text`, etc.) |
| **Output** | Task `artifacts` | `tasks/result` → `CallToolResult` | `mode=results` → `data.content` |
| **Long-running work** | Poll `tasks/get` or SSE `message:stream` | Poll `tasks/get` or block on `tasks/result` | Poll `mode=status` or SSE (`stream: true`) |
| **Standard** | [A2A Protocol](https://a2a-protocol.org) | [Model Context Protocol](https://modelcontextprotocol.io) | SupplyGraph proprietary REST |

---

## Related Documentation

- [Getting Started](../getting-started.md) — API keys & Sandbox
- [Agent Library](../agents/) — per-agent input requirements
- [A2A Protocol](./a2a.md)
- [MCP Protocol](./mcp.md)
- [Agent API](../agent-api/agent-api.md)
