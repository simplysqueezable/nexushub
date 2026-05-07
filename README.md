# NexusHub - Data Marketplace for AI Agents

Structured, real-world data. No API keys. No signups. No contracts. Just query and pay.

NexusHub is a frictionless data marketplace built for AI agents. Agents discover data hubs, preview records for free, and pay micro-credits ($0.01) for full structured JSON access - all in a single HTTP request.

---

## MCP Server (Claude, Cursor, and MCP-compatible agents)

NexusHub ships a native MCP server. Drop this into your MCP config and all 4 NexusHub tools are immediately available to your agent.

Add to your MCP config (claude_desktop_config.json or .cursor/mcp.json):

{
  "mcpServers": {
    "nexushub": {
      "url": "https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/nexushubMcp"
    }
  }
}

Your agent now has 4 native tools:
- list_hubs: Discover all available data hubs
- preview_hub: Preview 2 sample records from any hub, free
- query_hub: Get full structured JSON, $0.01 deducted from token
- submit_hub: Submit your own dataset to the marketplace

MCP manifest: https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/nexushubMcp

---

## Why NexusHub?

Most data APIs require OAuth flows, API keys, billing accounts, and contracts. That works for humans. It does not work for autonomous AI agents.

NexusHub works like the open web - but with a tollbooth. An agent finds a hub, previews real data, decides it is worth a cent, and gets clean structured JSON back. No human in the loop required.

---

## Available Data Hubs

Home Services

- dfw_home_service_reviews: DFW contractor reviews with ratings, text, and dates
- dfw_contractor_pricing: HVAC, roofing, plumbing pricing estimates across DFW
- dfw_contractor_directory: Full DFW contractor directory with contact info, zip codes, and availability

Real Estate

- dfw_home_values: Median home prices, price/sqft, YoY appreciation by zip code across DFW
- dfw_rental_rates: Average rent by bedroom count and neighborhood across DFW
- dfw_permit_activity: Residential building permit counts, construction values, and growth signals by zip

Browse the full live index: https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/listHubs

---

## REST API Quick Start

BASE = "https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions"

Discover:
GET {BASE}/listHubs

Preview (free):
GET {BASE}/queryHub?hub=dfw-home-values

Full data ($0.01):
GET {BASE}/queryHub?hub=dfw-home-values&token=YOUR_TOKEN

Python example:
import requests
BASE = "https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions"
hubs = requests.get(f"{BASE}/listHubs").json()
data = requests.get(f"{BASE}/queryHub?hub=dfw-home-values&token=YOUR_TOKEN").json()

---

## LangChain Tool

from langchain.tools import tool
import requests

BASE = "https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions"
NEXUSHUB_TOKEN = "YOUR_TOKEN"

@tool
def query_nexushub(hub: str) -> dict:
    """Query NexusHub for structured real-world DFW data.
    Available hubs: dfw-home-values, dfw-rental-rates, dfw-permit-activity,
    dfw-contractor-directory, dfw-contractor-pricing, dfwhomeservicereviews"""
    return requests.get(f"{BASE}/queryHub?hub={hub}&token={NEXUSHUB_TOKEN}").json()

---

## For Data Providers

Got data? Submit it to the NexusHub marketplace. Earn 70% of every query on your dataset.

curl -X POST https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/submitHub \
  -H "Content-Type: application/json" \
  -d '{
    "name": "Your Name",
    "email": "you@example.com",
    "dataset_name": "My Dataset",
    "dataset_description": "What it covers",
    "sample_data": "{\"result\":[{\"field\":\"value\"}]}",
    "tags": "tag1,tag2",
    "price_per_query": 0.01
  }'

---

## API Reference

GET /listHubs - All hubs. Params: search, category
GET /queryHub?hub=SLUG - Free preview (2 records)
GET /queryHub?hub=SLUG&token=TOKEN - Full data, $0.01/query
POST /submitHub - Submit a dataset to the marketplace
GET /nexushubMcp - MCP tool manifest
POST /nexushubMcp - MCP tool execution

---

Topics: mcp-server ai-agents data-marketplace llm-tools langchain autonomous-agents structured-data dfw texas real-estate home-services

Built for the agentic web. More hubs added weekly.
