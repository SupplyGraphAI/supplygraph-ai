# Regional Substitution Recommendation Agent

### Overview
Identifies and recommends **regional supplier alternatives** across deep-tier networks — enabling proactive compliance readiness, diversification, and operational resilience when global supply disruptions or trade restrictions occur.

### Challenge
Most enterprises only track Tier-1 suppliers.  
This limited visibility means deeper-tier dependencies (Tier-3 to Tier-10) often remain undiscovered until a regional crisis, export ban, or audit exposes them.  
The inability to quickly identify feasible substitutes in compliant regions leads to production halts, lost revenue, and regulatory risk.

### Value
The agent analyzes multi-tier supplier relationships to surface **practical, region-specific substitution options**.  
By mapping equivalent suppliers across geographies, it allows procurement and compliance teams to **restructure sourcing** strategically, maintaining business continuity during disruptions.

### Why Us
Built on **100M+ enterprise–product links** and a **multi-hop tracing algorithm**, SupplyGraph AI delivers transparent, auditable substitution recommendations beyond static supplier lists.  
Each result is grounded in verified enterprise-product relationships, regional policy data, and performance signals — all continuously refreshed.

### API Endpoint
`POST /v1/agents/regional-substitution`

#### Example Request
```bash
curl -X POST https://api.supplygraph.ai/v1/agents/regional-substitution \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "product": "Semiconductor wafer",
    "current_supplier_region": "East Asia",
    "target_region": "North America",
    "risk_context": "geopolitical"
  }'
```

#### Example Response
```json
{
  "status": "success",
  "data": {
    "product": "Semiconductor wafer",
    "recommended_regions": [
      {
        "region": "North America",
        "potential_suppliers": [
          {
            "company_name": "GlobalFoundries Inc.",
            "country": "US",
            "risk_score": 0.22,
            "capacity_index": 0.87,
            "compliance_certified": true
          },
          {
            "company_name": "SkyWater Technology",
            "country": "US",
            "risk_score": 0.31,
            "capacity_index": 0.78,
            "compliance_certified": true
          }
        ]
      }
    ],
    "substitution_confidence": 0.91,
    "source_evidence": [
      "SupplyGraph AI Deep-Tier Database v2025.10",
      "U.S. Semiconductor Supply Initiative Report"
    ]
  },
  "credits_used": 7
}
```

### A2A Interaction
| Direction | Partner Agent | Event | Purpose |
|------------|----------------|--------|----------|
| ➡️ Outbound | Visualization Agent | `regional_substitution_ready` | Publishes substitution map for visualization |
| ➡️ Outbound | Risk Sentinel Agent | `substitution_applied` | Informs risk module of successful substitution |
| ⬅️ Inbound | Tier-1 Supplier Discovery Agent | `tier1_list_ready` | Receives qualified supplier list for substitution matching |

**Example Payload:**
```json
{
  "intent": "regional_substitution_ready",
  "payload": {
    "product": "Semiconductor wafer",
    "region": "North America",
    "suppliers": [
      { "name": "GlobalFoundries Inc.", "country": "US" },
      { "name": "SkyWater Technology", "country": "US" }
    ],
    "timestamp": "2025-10-29T14:25:00Z"
  }
}
```

### Related Docs
- [Getting Started](../getting-started.md)  
- [API Reference](../api.md)  
- [A2A Protocol](../a2a.md)
