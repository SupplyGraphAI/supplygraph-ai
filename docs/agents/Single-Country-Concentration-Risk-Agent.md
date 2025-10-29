# Single-Country Concentration Risk Agent

### Overview
Analyzes and quantifies **geographic concentration risks** across global supply networks — identifying where a company’s upstream dependencies rely too heavily on a single country or region.

### Challenge
Most organizations believe their supply chains are diversified — until a single-country dependency quietly halts production.  
Geopolitical shocks, export bans, and natural disasters can ripple through 4–10 tiers of suppliers, causing component shortages, delivery delays, and millions in lost revenue.  
Traditional visibility tools fail to detect such deep-tier dependencies before disruptions occur.

### Value
The agent reveals **hidden single-country dependencies** by tracing multi-tier supplier relationships and mapping geographic exposure from Tier-2 to Tier-10.  
It provides quantifiable concentration metrics such as **Herfindahl–Hirschman Index (HHI)** and **regional dependency ratios**, helping teams prioritize diversification and resilience planning.

### Why Us
SupplyGraph AI delivers global-scale concentration insights derived from **100M+ companies**, **8,000+ industry benchmarks**, and **1M+ key products**.  
By constructing an **enterprise-centered supply graph** and applying multi-dimensional statistical models, we transform complex geographic exposure into actionable risk intelligence.

### API Endpoint
`POST /v1/agents/concentration-risk`

#### Example Request
```bash
curl -X POST https://api.supplygraph.ai/v1/agents/concentration-risk \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "company_name": "Apple Inc.",
    "depth": 8
  }'
```

#### Example Response
```json
{
  "status": "success",
  "data": {
    "root_company": "Apple Inc.",
    "tiers_analyzed": 8,
    "country_exposure": [
      { "country": "CN", "percentage": 52.4 },
      { "country": "US", "percentage": 18.9 },
      { "country": "JP", "percentage": 7.3 }
    ],
    "hhi_index": 0.341,
    "risk_level": "High",
    "top_concentrated_components": [
      { "component": "Lithium Batteries", "country": "CN", "exposure": 82.3 },
      { "component": "Display Glass", "country": "KR", "exposure": 64.1 }
    ]
  },
  "credits_used": 7
}
```

### A2A Interaction
| Direction | Partner Agent | Event | Purpose |
|------------|----------------|--------|----------|
| ➡️ Outbound | Tier-1 Supplier Discovery Agent | `concentration_result_ready` | Provides geographic concentration insights for sourcing actions |
| ➡️ Outbound | Visualization Agent | `country_exposure_map` | Publishes concentration data for geographic visualization |
| ⬅️ Inbound | Supply Visualization Agent | `graph_structure_ready` | Receives upstream graph for country-level exposure computation |

**Example Payload:**
```json
{
  "intent": "country_exposure_map",
  "payload": {
    "root_company": "Apple Inc.",
    "dominant_country": "CN",
    "dominant_share": 52.4,
    "hhi_index": 0.341,
    "timestamp": "2025-10-29T13:55:00Z"
  }
}
```

### Related Docs
- [Getting Started](../getting-started.md)  
- [API Reference](../api.md)  
- [A2A Protocol](../a2a.md)
