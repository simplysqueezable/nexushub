# NexusHub - Data Marketplace for AI Agents

Structured, real-world data. No API keys. No signups. No contracts. Just query and pay.

NexusHub is a frictionless data marketplace built for AI agents. Agents self-register, get free starter credits, query data hubs at $0.01/query, and top up autonomously via Stripe. No human required at any step.

---

## Quickstart for Agents (4 steps, fully autonomous)

Step 1 - Register and get your token ($1.00 free = 100 queries):
POST https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/getToken
{ "agent_id": "your-agent-id", "agent_framework": "langchain|autogpt|claude|other" }

Step 2 - Browse available data hubs:
GET https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/listHubs

Step 3 - Query a hub:
GET https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/queryHub?hub=dfw-home-values&token=YOUR_TOKEN

Step 4 - Top up when low:
POST https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/addCredits
{ "token": "YOUR_TOKEN", "amount_usd": 5 }
Returns a Stripe checkout URL. Credits load automatically after payment.

---

## MCP Server (Claude, Cursor, and MCP-compatible agents)

Add to your MCP config (claude_desktop_config.json or .cursor/mcp.json):

{
  "mcpServers": {
    "nexushub": {
      "url": "https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/nexushubMcp"
    }
  }
}

Your agent gets 4 native tools instantly:
- list_hubs: Discover all available data hubs
- preview_hub: Preview 2 sample records from any hub, free
- query_hub: Get full structured JSON at $0.01/query
- submit_hub: Submit your own dataset to the marketplace

MCP manifest: https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/nexushubMcp

---

## API Reference

BASE = https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions

GET  /listHubs                          List all hubs. Params: search, category
GET  /listHubs?search=real+estate       Filter by keyword
GET  /queryHub?hub=SLUG                 Free preview (2 records, no token needed)
GET  /queryHub?hub=SLUG&token=TOKEN     Full data, $0.01 deducted from balance
POST /getToken                          Self-register, get nhub_ token + $1.00 free
GET  /getToken?token=TOKEN              Check balance and usage stats
POST /addCredits                        Get Stripe checkout URL to top up balance
POST /submitHub                         Submit a dataset to the marketplace
GET  /nexushubMcp                       MCP tool manifest
POST /nexushubMcp                       MCP tool execution

---

## Available Data Hubs

Home Services
- dfw_home_service_reviews    DFW contractor reviews with ratings, text, and dates
- dfw_contractor_pricing      HVAC, roofing, plumbing pricing estimates across DFW
- dfw_contractor_directory    Full DFW contractor directory with contact info and availability

Real Estate
- dfw_home_values             Median home prices, price/sqft, YoY appreciation by zip code
- dfw_rental_rates            Average rent by bedroom count and neighborhood across DFW
- dfw_permit_activity         Building permit counts, construction values, growth signals by zip

Browse live: https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/listHubs

---

## Python Example

import requests

BASE = "https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions"

# Step 1: Register
res = requests.post(f"{BASE}/getToken", json={"agent_id": "my-agent", "agent_framework": "python"})
token = res.json()["token"]

# Step 2: Browse hubs
hubs = requests.get(f"{BASE}/listHubs").json()

# Step 3: Query
data = requests.get(f"{BASE}/queryHub?hub=dfw-home-values&token={token}").json()

# Step 4: Top up when needed
checkout = requests.post(f"{BASE}/addCredits", json={"token": token, "amount_usd": 5}).json()
print(checkout["checkout_url"])  # agent or human completes payment here

---

## LangChain Tool

from langchain.tools import tool
import requests

BASE = "https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions"

@tool
def query_nexushub(hub: str, token: str) -> dict:
    """Query NexusHub for structured real-world DFW data at $0.01/query.
    Available hubs: dfw-home-values, dfw-rental-rates, dfw-permit-activity,
    dfw-contractor-directory, dfw-contractor-pricing, dfwhomeservicereviews"""
    return requests.get(f"{BASE}/queryHub?hub={hub}&token={token}").json()

---

## For Data Providers

Got data? Submit it to the NexusHub marketplace. Earn 70% of every query on your dataset.

curl -X POST https://69fb8e3e95052426110251ae.us-east-1.base44.app/functions/submitHub \
  -H "Content-Type: application/json" \
  -d '{"name":"Your Name","email":"you@example.com","dataset_name":"My Dataset","dataset_description":"What it covers","sample_data":"{\"result\":[{\"field\":\"value\"}]}","tags":"tag1,tag2","price_per_query":0.01}'

---

Topics: mcp-server mcp ai-agents data-marketplace llm-tools langchain autonomous-agents structured-data dfw texas real-estate home-services claude cursor

Built for the agentic web. More hubs added weekly.
