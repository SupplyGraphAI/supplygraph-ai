# Customs Classification Agent

### Overview  
Automatically maps products to the correct HS/HTS codes, ensuring compliance, accuracy, and tariff optimization — all within seconds.



### Challenge  
For customs specialists, getting an item’s HTS code is a critical workflow task that involves manually checking product descriptions against thousands of classification tables.  
This process is time-consuming, mentally exhausting, and prone to human error.  
Moreover, single products may correspond to multiple potential HTS codes, each implying different tariff implications.



### Value  
Automated classification reduces manual work time from **hours or days to seconds**, while unlocking opportunities for **tariff optimization**.  
By automating classification, organizations can ensure consistency, accuracy, and compliance across product catalogs and trade operations.



### Why Us  
SupplyGraph AI integrates **real-time global tariff databases** with **graph-based reasoning** to deliver precise, auditable classifications with full traceability.  
Each classification is supported by evidence paths that link back to official customs sources, minimizing compliance risk at scale.



### API Endpoint  
`POST /v1/agents/customs-classification`

#### Example Request  
```bash
curl -X POST https://api.supplygraph.ai/v1/agents/customs-classification \
  -H "Authorization: Bearer <YOUR_API_KEY>" \
  -H "Content-Type: application/json" \
  -d '{
    "product_description": "Men’s leather sneakers",
    "country_of_origin": "CN"
  }'
```

#### Example Response  
```json
{
  "status": "success",
  "data": {
    "predicted_hts": "6403.99.60",
    "description": "Footwear with outer soles of rubber or plastics, with uppers of leather",
    "confidence_score": 0.96,
    "alternatives": [
      { "hts": "6403.99.30", "confidence": 0.71 },
      { "hts": "6403.91.90", "confidence": 0.55 }
    ],
    "source_evidence": [
      "U.S. HTS 2025 Revision 25 – Chapter 64",
      "WCO Explanatory Notes – Footwear, p. 221"
    ]
  },
  "credits_used": 3
}
```



### A2A Interaction  

| Direction | Partner Agent | Event | Purpose |
|------------|----------------|--------|----------|
| ➡️ Outbound | Tariff Calculation Agent | `classification_result` | Passes predicted HTS code for duty computation |
| ⬅️ Inbound | Product Ingestion Agent | `new_product_added` | Receives product description for classification |

**Example Payload:**  
```json
{
  "intent": "classification_result",
  "payload": {
    "product_id": "sku_0921",
    "predicted_hts": "6403.99.60",
    "confidence": 0.96
  }
}
```



### Related Docs  
- [Getting Started](../getting-started.md)  
- [A2A Protocol](../a2a.md)
