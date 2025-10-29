# Tier-1 Supplier Discovery Agent

### Overview
Discovers and ranks **qualified Tier-1 suppliers** for a given product, component, or category — providing sourcing teams with rapid, data-driven alternatives that combine reliability, compliance, and performance insights.

### Challenge
Procurement teams struggle to identify reliable Tier-1 suppliers at scale.  
Manual research is fragmented, slow, and often limited to self-reported or outdated databases.  
This lack of transparency weakens negotiation leverage, delays onboarding, and undermines efforts to diversify supplier bases.

### Value
The agent automates global supplier discovery, analyzing millions of enterprise-product relationships to surface relevant, low-risk suppliers within **minutes**.  
It enables smarter sourcing strategies, faster evaluations, and stronger negotiation positions through data-backed insights.

### Why Us
Powered by **100M+ enterprise-product links**, SupplyGraph AI delivers **AI-verified supplier recommendations** enriched with **ESG metrics, compliance flags, and performance signals**.  
This ensures every shortlist is not only relevant but also **auditable, explainable, and continuously updated** for ongoing supply resilience.

### API Endpoint
`POST /v1/agents/tier1-discovery`

#### Example Request
```bash
curl -X POST https://api.supplygraph.ai/v1/agents/tier1-discovery \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "product": "Electric motor assembly",
    "region": "Europe",
    "filters": {
      "esg_compliant": true,
      "risk_score_max": 0.4
    }
  }'
```

#### Example Response
```json
{
  "status": "success",
  "data": {
    "product": "Electric motor assembly",
    "recommended_suppliers": [
      {
        "company_name": "Bosch Mobility Solutions",
        "country": "DE",
        "risk_score": 0.23,
        "esg_compliant": true,
        "performance_index": 0.88
      },
      {
        "company_name": "Valeo Powertrain Systems",
        "country": "FR",
        "risk_score": 0.29,
        "esg_compliant": true,
        "performance_index": 0.85
      }
    ],
    "search_scope": {
      "regions_considered": ["Europe", "Asia"],
      "total_candidates": 4823
    }
  },
  "credits_used": 6
}
```

### A2A Interaction
| Direction | Partner Agent | Event | Purpose |
|------------|----------------|--------|----------|
| ➡️ Outbound | Regional Substitution Agent | `tier1_list_ready` | Sends qualified supplier candidates for regional substitution analysis |
| ➡️ Outbound | Visualization Agent | `supplier_discovery_complete` | Publishes discovered supplier network for graph rendering |
| ⬅️ Inbound | Concentration Risk Agent | `concentration_result_ready` | Receives geographic exposure data to prioritize diversification regions |

**Example Payload:**
```json
{
  "intent": "tier1_list_ready",
  "payload": {
    "product": "Electric motor assembly",
    "suppliers": [
      { "name": "Bosch Mobility Solutions", "country": "DE" },
      { "name": "Valeo Powertrain Systems", "country": "FR" }
    ],
    "timestamp": "2025-10-29T14:10:00Z"
  }
}
```

### Related Docs
- [Getting Started](../getting-started.md)  
- [API Reference](../api.md)  
- [A2A Protocol](../a2a.md)
