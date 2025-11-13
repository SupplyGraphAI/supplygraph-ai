# Global Supply Dependency Visualization Agent

### Overview
Constructs and visualizes an **enterprise-centered global supply–product graph**, mapping dependencies across multiple tiers — from Tier-1 suppliers to raw material origins — to reveal systemic risks and hidden vulnerabilities.

### Challenge
Global supply chains are vast, opaque, and interconnected.  
Most enterprises only see their Tier-1 suppliers, leaving deep-tier dependencies and concentration risks invisible.  
When a disruption occurs upstream — a factory closure, a logistics bottleneck, or a policy change — its cascading impact often catches companies off guard.

### Value
This agent builds a **dynamic company-centered supply graph**, allowing users to explore supplier relationships, trace dependencies, and simulate potential disruptions across 10+ tiers.  
It provides the visibility and analytical depth needed to **anticipate, quantify, and mitigate** risk before it affects operations.

### Why Us
SupplyGraph AI harnesses **100M+ enterprise records**, **8,000+ industry benchmarks**, and **1M+ product nodes** to construct an always-updated, evidence-based graph.  
Unlike static databases, our visualization engine delivers **live, explainable, and auditable** insights that reflect the real structure of the global economy.

### API Endpoint
`POST /v1/agents/supply-visualization`

#### Example Request
```bash
curl -X POST https://api.supplygraph.ai/v1/agents/supply-visualization \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "company_name": "Tesla, Inc.",
    "country": "US",
    "depth": 5
  }'
```

#### Example Response
```json
{
  "status": "success",
  "data": {
    "root_company": "Tesla, Inc.",
    "depth_requested": 5,
    "nodes": 812,
    "edges": 1149,
    "key_dependencies": [
      {
        "supplier": "LG Energy Solution",
        "tier": 2,
        "products": ["Battery Cells"],
        "region": "KR"
      },
      {
        "supplier": "Tianqi Lithium",
        "tier": 4,
        "products": ["Lithium Hydroxide"],
        "region": "CN"
      }
    ],
    "concentration_index": 0.71
  },
  "credits_used": 10
}
```

### A2A Interaction
| Direction | Partner Agent | Event | Purpose |
|------------|----------------|--------|----------|
| ➡️ Outbound | Concentration Risk Agent | `graph_structure_ready` | Sends structured dependency data for country concentration analysis |
| ➡️ Outbound | Visualization Dashboard Agent | `graph_visual_ready` | Publishes completed graph for UI rendering |
| ⬅️ Inbound | Tariff Monitoring Agent | `tariff_update_event` | Receives alerts to update graph connections by affected HTS codes |

**Example Payload:**
```json
{
  "intent": "graph_structure_ready",
  "payload": {
    "root_company": "Tesla, Inc.",
    "tier_depth": 5,
    "node_count": 812,
    "edge_count": 1149,
    "timestamp": "2025-10-29T13:40:00Z"
  }
}
```

### Related Docs
- [Getting Started](../getting-started.md)  
- [A2A Protocol](../a2a.md)
