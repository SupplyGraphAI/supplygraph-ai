# Tariff Calculation Agent

### Overview
Automatically calculates U.S. duty rates and applicable additional tariffs based on the product’s HTS code and country of origin.

### Challenge
For customs and trade compliance specialists, determining an item’s duty rate and applicable tariffs usually means manually searching through thousands of pages of tariff schedules.  
This process is not only time-consuming but also error-prone, leading to inconsistent or incomplete results.

### Value
**AutoTariff** automates the calculation of duty rates and applicable additional tariffs, cutting analysis time from **hours to seconds**.  
It supports accurate decision-making, enables tariff optimization, and ensures compliance with evolving customs regulations.

### Why Us
SupplyGraph AI integrates **real-time global tariff databases** with **graph-based reasoning**, producing precise and auditable calculations.  
Each result is backed by verifiable source data, ensuring full transparency and minimizing compliance risk at scale.

### API Endpoint
`POST /v1/agents/tariff-calculation`

#### Example Request
```bash
curl -X POST https://api.supplygraph.ai/v1/agents/tariff-calculation \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "hts_code": "2105.00.20.00",
    "country_of_origin": "CN",
    "declared_value_usd": 1000
  }'
```

#### Example Response
```json
{
  "status": "success",
  "data": {
    "hts_code": "2105.00.20.00",
    "base_rate": "17%",
    "additional_duties": [
      { "chapter_99": "9903.88.15", "duty_rate": "25%" }
    ],
    "total_effective_rate": "42%",
    "estimated_duty_due": 420,
    "source_evidence": [
      "HTSUS 2025 Rev. 25 – Chapter 21",
      "USTR Notice 9903.88.15 – Section 301 Tariffs"
    ]
  },
  "credits_used": 5
}
```

### A2A Interaction
| Direction | Partner Agent | Event | Purpose |
|------------|----------------|--------|----------|
| ➡️ Outbound | Tariff Monitoring Agent | `tariff_rate_applied` | Reports calculated duty rate for change monitoring |
| ⬅️ Inbound | Customs Classification Agent | `classification_result` | Receives HTS code for calculation input |

**Example Payload:**
```json
{
  "intent": "tariff_rate_applied",
  "payload": {
    "hts_code": "2105.00.20.00",
    "country_of_origin": "CN",
    "total_effective_rate": "42%",
    "timestamp": "2025-10-29T13:20:00Z"
  }
}
```

### Related Docs
- [Getting Started](../getting-started.md)  
- [API Reference](../api.md)  
- [A2A Protocol](../a2a.md)
