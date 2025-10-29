# API Reference

Welcome to the **SupplyGraph AI RESTful API Reference**.  
This document provides the global rules, headers, and response formats that apply to all agents across the platform.



## 1. Base URL

```
https://api.supplygraph.ai/v1/
```

All endpoints follow a RESTful convention and require HTTPS.



## 2. Authentication

All API calls require an **API Key** obtained from your developer dashboard.  
Include the key in the `Authorization` header using the Bearer scheme:

```
Authorization: Bearer <YOUR_API_KEY>
```

For instructions on obtaining your key, see the [Getting Started Guide](./getting-started.md).



## 3. Request Format

All requests use JSON.  
Example header set:

```
Content-Type: application/json
Accept: application/json
```



## 4. Standard Response Schema

Every response returns a unified JSON structure:

```
{
  "status": "success",
  "data": { ... },
  "credits_used": 2
}
```

If an error occurs:

```
{
  "status": "error",
  "code": "INVALID_AUTH",
  "message": "API key missing or expired"
}
```



## 5. Error Codes

| Code | Description |
|------|--------------|
| `INVALID_AUTH` | API key is missing, invalid, or expired |
| `INSUFFICIENT_CREDITS` | Not enough credits for this request |
| `INVALID_PAYLOAD` | Payload format or required fields are incorrect |
| `RATE_LIMITED` | Too many requests in a short time |
| `SERVER_ERROR` | Internal system issue — please retry later |



## 6. Rate Limits & Credits

Each request consumes a certain number of credits depending on the agent used.  
Default rate limits are applied per API key to ensure fair use.

| Limit Type | Default |
|-------------|----------|
| Requests per minute | 60 |
| Maximum concurrent tasks | 5 |
| Credit billing interval | per request |

> High-volume users can request custom limits via **enterprise@supplygraph.ai**.



## 7. Common Headers

| Header | Required | Description |
|--------|-----------|--------------|
| `Authorization` | ✅ | `Bearer <API_KEY>` |
| `Content-Type` | ✅ | Always `application/json` |
| `Accept` | Optional | `application/json` (default) |



## 8. Available Agent Endpoints

Each agent provides its own endpoint and schema.  
Detailed documentation is available under `/docs/agents/`.

| Agent | Endpoint | Description |
|:------|:----------|:-------------|
| **Customs Classification Agent** | `/agents/customs-classification` | Maps products to correct HS/HTS codes |
| **Tariff Calculation Agent** | `/agents/tariff-calculation` | Calculates U.S. duty rates and additional tariffs |
| **Tariff Monitoring Agent** | `/agents/tariff-monitoring` | Monitors and alerts tariff schedule changes |
| **Due Diligence Agent** | `/agents/due-diligence` | Generates company due diligence reports |
| **Global Supply Visualization Agent** | `/agents/supply-visualization` | Visualizes global multi-tier supply dependencies |
| **Single-Country Concentration Agent** | `/agents/concentration-risk` | Quantifies country-level dependency |
| **Tier-1 Supplier Discovery Agent** | `/agents/tier1-discovery` | Finds qualified Tier-1 suppliers |
| **Regional Substitution Agent** | `/agents/regional-substitution` | Suggests practical regional supplier alternatives |



## 9. Example Request

```
POST /v1/agents/due-diligence
Authorization: Bearer <YOUR_API_KEY>
Content-Type: application/json
```

Body:
```
{
  "company_name": "Midea Group",
  "country": "CN"
}
```

Response:
```
{
  "status": "success",
  "data": {
    "industry_classification": "Home Appliances",
    "risk_score": 0.13,
    "benchmarks": {
      "sector_average": 0.25,
      "regional_average": 0.19
    }
  },
  "credits_used": 5
}
```



## 10. Webhook (Optional)

If your integration supports async processing,  
you can set a webhook to receive task completion notifications.

Webhook payload example:

```
{
  "agent": "tariff-monitoring",
  "task_id": "ab3d92",
  "status": "completed",
  "timestamp": "2025-10-29T13:10:00Z"
}
```



## 11. Related Docs

- [Getting Started Guide](./getting-started.md)
- [A2A Protocol](./a2a.md)
- [Agent Specifications](./agents/)
