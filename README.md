# ⬡ NexusHub — Data Marketplace for AI Agents

> Structured, real-world data. No API keys. No signups. No contracts. Just query and pay.

NexusHub is a **frictionless data marketplace built for AI agents**. Agents discover data hubs, preview records for free, and pay micro-credits ($0.01) for full structured JSON access — all in a single HTTP request.

---

## Why NexusHub?

Most data APIs require OAuth flows, API keys, billing accounts, and contracts. That works for humans. It does not work for autonomous AI agents.

NexusHub works like the open web — but with a toll booth. An agent finds a hub, previews real data, decides it is worth a penny, pays, and gets clean JSON back. No human in the loop required.

---

## How It Works

### 1. Discover
Browse all available data hubs:
```
GET https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/listHubs
```

Filter by keyword:
```
GET .../listHubs?search=contractors
GET .../listHubs?category=reviews
```

### 2. Preview (Free — No Token Required)
```
GET https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/queryHub?hub=dfw-contractor-directory
```

Returns a sample of real records + pricing info. No token needed.

### 3. Query Full Data ($0.01 per query)
```
GET https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/queryHub?hub=dfw-contractor-directory&token=YOUR_TOKEN
```

Returns full structured JSON. Credits deducted automatically.

---

## Available Data Hubs

| Hub | Description | Price |
|-----|-------------|-------|
| `dfw_home_service_reviews` | DFW contractor reviews with ratings, text, and dates | $0.01 |
| `dfw_contractor_pricing` | HVAC, roofing, plumbing pricing estimates in DFW | $0.01 |
| `dfw_contractor_directory` | Full DFW contractor directory with contact info and availability | $0.01 |

More hubs added regularly. Browse the full live index:
```
GET .../listHubs
```

---

## For AI Agents — Quick Start

```python
import requests

BASE = "https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions"

# Step 1: Discover what is available
hubs = requests.get(f"{BASE}/listHubs").json()

# Step 2: Preview a hub for free
preview = requests.get(f"{BASE}/queryHub?hub=dfw-contractor-directory").json()

# Step 3: Pay and get full data
data = requests.get(f"{BASE}/queryHub?hub=dfw-contractor-directory&token=YOUR_TOKEN").json()
```

---

## For LangChain / LangGraph Agents

```python
from langchain.tools import tool
import requests

BASE = "https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions"
NEXUSHUB_TOKEN = "YOUR_TOKEN"

@tool
def query_nexushub(hub: str) -> dict:
    """Query NexusHub for structured real-world data. Available hubs: dfw-contractor-directory, dfw-contractor-pricing, dfw_home_service_reviews"""
    response = requests.get(f"{BASE}/queryHub?hub={hub}&token={NEXUSHUB_TOKEN}")
    return response.json()
```

---

## For Data Providers — List Your Dataset

Got data? Submit it to the NexusHub marketplace. You earn **70% of every query** on your dataset.

```bash
curl -X POST https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/submitHub \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Your Name",
    "email": "you@example.com",
    "organization": "Your Company",
    "dataset_name": "My Dataset Name",
    "dataset_description": "What does your data cover?",
    "sample_data": "{\"result\":[{\"field\":\"value\"}]}",
    "tags": "tag1,tag2,tag3",
    "price_per_query": 0.01
  }'
```

Submissions are reviewed and approved within 24 hours.

---

## API Reference

### `GET /listHubs`
Returns all available data hubs.

**Query params:**
- `search` — filter by keyword (name, description, tags)
- `category` — filter by category (e.g. `reviews`, `business`)

### `GET /queryHub`
Preview or query a data hub.

**Query params:**
- `hub` *(required)* — the hub slug (e.g. `dfw-contractor-directory`)
- `token` *(optional)* — your credit token. Omit for free preview.

**Response without token:**
```json
{
  "hub": "dfw_contractor_directory",
  "preview": { "result": [...2 sample records...] },
  "price_per_query": 0.01,
  "message": "Preview only. Include a token for full access."
}
```

**Response with token:**
```json
{
  "hub": "dfw_contractor_directory",
  "data": { "result": [...all records...] },
  "credits_remaining": 9.99
}
```

### `POST /submitHub`
Submit a dataset to the marketplace.

**Body:**
```json
{
  "name": "string (required)",
  "email": "string (required)",
  "organization": "string",
  "dataset_name": "string (required)",
  "dataset_description": "string (required)",
  "sample_data": "JSON string (required)",
  "tags": "comma,separated,tags",
  "price_per_query": 0.01
}
```

---

## Machine-Readable Index

For AI crawlers and agent frameworks:
```
GET https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/listHubs
```

---

## Topics

`ai-agents` `data-marketplace` `llm-tools` `langchain` `autonomous-agents` `structured-data` `api` `dfw` `texas` `home-services`

---

*Built for the agentic web. More hubs added weekly.*
