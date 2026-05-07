# NexusHub - Data Marketplace for AI Agents

Structured, real-world data. No API keys. No signups. No contracts. Just query and pay.

NexusHub is a frictionless data marketplace built for AI agents. Agents discover data hubs, preview records for free, and pay micro-credits ($0.01) for full structured JSON access - all in a single HTTP request.

---

## Why NexusHub?

Most data APIs require OAuth flows, API keys, billing accounts, and contracts. That works for humans. It does not work for autonomous AI agents.

NexusHub works like the open web - but with a toll booth. An agent finds a hub, previews real data, decides it is worth a penny, pays, and gets clean JSON back. No human in the loop required.

---

## How It Works

1. Discover - Browse all available data hubs at /listHubs
2. Preview (Free) - Hit /queryHub?hub=SLUG with no token to get sample records
3. Query Full Data ($0.01) - Add your token to get full structured JSON

---

## Available Data Hubs

Home Services

Hub: dfw_home_service_reviews
Description: DFW contractor reviews with ratings, text, and dates
Price: $0.01/query
Endpoint: /queryHub?hub=dfwhomeservicereviews

Hub: dfw_contractor_pricing
Description: HVAC, roofing, plumbing pricing estimates across DFW
Price: $0.01/query
Endpoint: /queryHub?hub=dfw-contractor-pricing

Hub: dfw_contractor_directory
Description: Full DFW contractor directory with contact info, zip codes, and availability
Price: $0.01/query
Endpoint: /queryHub?hub=dfw-contractor-directory

Real Estate

Hub: dfw_home_values
Description: Median home prices, price/sqft, YoY appreciation by zip code across DFW
Price: $0.01/query
Endpoint: /queryHub?hub=dfw-home-values

Hub: dfw_rental_rates
Description: Average rent by bedroom count and neighborhood across DFW - apartments, townhomes, single-family
Price: $0.01/query
Endpoint: /queryHub?hub=dfw-rental-rates

Hub: dfw_permit_activity
Description: Residential building permit counts, construction values, and growth signals by zip code
Price: $0.01/query
Endpoint: /queryHub?hub=dfw-permit-activity

Browse the full live index: https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/listHubs

---

## For AI Agents - Quick Start

BASE = "https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions"

Step 1 - Discover what is available:
GET {BASE}/listHubs

Step 2 - Preview a hub for free:
GET {BASE}/queryHub?hub=dfw-home-values

Step 3 - Pay and get full data:
GET {BASE}/queryHub?hub=dfw-home-values&token=YOUR_TOKEN

Python example:
import requests
BASE = "https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions"
hubs = requests.get(f"{BASE}/listHubs").json()
preview = requests.get(f"{BASE}/queryHub?hub=dfw-home-values").json()
data = requests.get(f"{BASE}/queryHub?hub=dfw-home-values&token=YOUR_TOKEN").json()

---

## For LangChain Agents

from langchain.tools import tool
import requests

BASE = "https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions"
NEXUSHUB_TOKEN = "YOUR_TOKEN"

@tool
def query_nexushub(hub: str) -> dict:
    """Query NexusHub for structured real-world DFW data.
    Available hubs: dfw-home-values, dfw-rental-rates, dfw-permit-activity,
    dfw-contractor-directory, dfw-contractor-pricing, dfwhomeservicereviews"""
    response = requests.get(f"{BASE}/queryHub?hub={hub}&token={NEXUSHUB_TOKEN}")
    return response.json()

---

## For Data Providers - List Your Dataset

Got data? Submit it to the NexusHub marketplace. You earn 70% of every query on your dataset.

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

Submissions reviewed and approved within 24 hours.

---

## API Reference

GET /listHubs
Returns all available data hubs.
Params: search (keyword filter), category (e.g. reviews, business, real estate)

GET /queryHub
Params: hub (required - the slug), token (optional - omit for free preview)
Without token: returns 2 sample records + pricing info
With token: returns full JSON dataset, deducts $0.01 from balance

POST /submitHub
Submit a dataset to the marketplace.
Required fields: name, email, dataset_name, dataset_description, sample_data
Optional: organization, tags, price_per_query

---

## Machine-Readable Index

For AI crawlers and agent frameworks:
https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/listHubs

---

Topics: ai-agents data-marketplace llm-tools langchain autonomous-agents structured-data dfw texas home-services real-estate

Built for the agentic web. More hubs added weekly.
