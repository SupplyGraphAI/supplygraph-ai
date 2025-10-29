# Tariff Monitoring Agent

### Overview
Continuously monitors the 5,000+ page U.S. Customs Tariff Schedule, detecting and summarizing any policy or rate changes within hours — keeping enterprises compliant and ahead of regulatory shifts.

### Challenge
Tariff changes, Section 301 updates, and CBP reclassifications occur frequently and without warning.  
Manually tracking these updates across the USTR, CBP, and Federal Register is time-consuming, resource-intensive, and error-prone.  
Organizations often learn about tariff impacts **days or weeks too late**, leading to compliance violations, shipment delays, and unanticipated costs.

### Value
Automated, real-time tariff monitoring cuts detection time from **days to minutes**, enabling teams to act within two hours of a regulatory change.  
It prevents costly errors, avoids penalties, and eliminates the need for constant manual tracking.

### Why Us
SupplyGraph AI deploys **policy-aware monitoring agents** that read, parse, and interpret every change to the U.S. tariff schedule, matching it directly to your product and supplier network.  
All updates are **mapped to HTS codes and Section 99 provisions**, and every alert includes **source-linked evidence** for immediate verification.

### API Endpoint
`POST /v1/agents/tariff-monitoring`

#### Example Request
```bash
curl -X POST https://api.supplygraph.ai/v1/agents/tariff-monitoring \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "hts_codes": ["2105.00.20.00", "8431.31.00"],
    "notification_email": "alerts@enterprise.com"
  }'
```

#### Example Response
```json
{
  "status": "success",
  "data": {
    "monitored_hts": ["2105.00.20.00", "8431.31.00"],
    "changes_detected": [
      {
        "hts_code": "2105.00.20.00",
        "change_summary": "Section 301 duty reduced from 25% to 15%",
        "effective_date": "2025-11-01",
        "source": "USTR Federal Register Vol. 90, No. 2025-231"
      }
    ],
    "next_check": "2025-10-29T22:00:00Z"
  },
  "credits_used": 4
}
```

### A2A Interaction
| Direction | Partner Agent | Event | Purpose |
|------------|----------------|--------|----------|
| ➡️ Outbound | Tariff Calculation Agent | `tariff_change` | Notifies calculation engine to recompute effective rates |
| ➡️ Outbound | Visualization Agent | `tariff_update_event` | Updates supply graph visualization to reflect new duty changes |
| ⬅️ Inbound | Risk Sentinel Agent | `monitoring_request` | Receives instructions on specific categories or partners to monitor |

**Example Payload:**
```json
{
  "intent": "tariff_change",
  "payload": {
    "hts_code": "9903.88.15",
    "change_summary": "Additional duty decreased from 25% to 15%",
    "effective_date": "2025-11-01",
    "source_reference": "USTR Federal Register 2025-231"
  }
}
```

### Related Docs
- [Getting Started](../getting-started.md)  
- [API Reference](../api.md)  
- [A2A Protocol](../a2a.md)
