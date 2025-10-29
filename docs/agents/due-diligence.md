# Due Diligence Agent

### Overview
Generates automated company due diligence reports across **8,000+ industry classifications** worldwide — complete with benchmarking, anomaly detection, and dynamic updates that keep intelligence current and auditable.

### Challenge
Traditional due diligence is **slow, fragmented, and expensive**.  
Analysts often spend over 10 hours per company collecting and verifying unstructured data that becomes outdated almost immediately.  
Manual processes lack standardization and consistency, making comparisons across companies or regions nearly impossible.

### Value
Automated due diligence reduces research time by **up to 90%**, delivering standardized and comparable intelligence in minutes.  
The agent continuously monitors entity changes, key risk indicators, and market signals, ensuring teams always work with **current, validated data**.

### Why Us
Powered by real-time data from **100M+ companies**, **8,000+ industry benchmarks**, and **1M+ key products**, SupplyGraph AI builds structured, explainable, and continuously updated profiles.  
Each report maintains an **evidence trail**, ensuring every insight can be audited, shared, and trusted across compliance, finance, and M&A teams.

### API Endpoint
`POST /v1/agents/due-diligence`

#### Example Request
```bash
curl -X POST https://api.supplygraph.ai/v1/agents/due-diligence \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "company_name": "Midea Group Co., Ltd.",
    "country": "CN",
    "report_type": "comprehensive"
  }'
```

#### Example Response
```json
{
  "status": "success",
  "data": {
    "company_name": "Midea Group Co., Ltd.",
    "industry": "Home Appliances",
    "risk_score": 0.18,
    "financial_health": {
      "stability_index": 0.82,
      "liquidity_ratio": 1.7
    },
    "governance": {
      "sanction_flags": false,
      "esg_score": 74
    },
    "benchmarks": {
      "sector_average_risk": 0.25,
      "regional_average_risk": 0.22
    },
    "last_updated": "2025-10-29T12:00:00Z"
  },
  "credits_used": 6
}
```

### A2A Interaction
| Direction | Partner Agent | Event | Purpose |
|------------|----------------|--------|----------|
| ➡️ Outbound | Risk Sentinel Agent | `due_diligence_ready` | Sends structured risk insights for further analysis |
| ➡️ Outbound | Consulting Agent | `due_diligence_summary` | Shares company summaries for advisory workflows |
| ⬅️ Inbound | Monitoring Agent | `company_update_detected` | Triggers refresh when new company signals are observed |

**Example Payload:**
```json
{
  "intent": "due_diligence_ready",
  "payload": {
    "company_id": "mdg_23122",
    "risk_score": 0.18,
    "updated_at": "2025-10-29T12:00:00Z",
    "benchmark_context": "Home Appliances / East Asia"
  }
}
```

### Related Docs
- [Getting Started](../getting-started.md)  
- [API Reference](../api.md)  
- [A2A Protocol](../a2a.md)
