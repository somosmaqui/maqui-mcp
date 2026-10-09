# Maqui Analytics MCP Server

All-in-one marketing analytics MCP for local businesses and agencies: Instagram, ads, web traffic, SEO and reviews.

**Server URL:** `https://app.somosmaqui.com/api/mcp`

---

## What you can ask

Connect Maqui to your AI assistant and query all your marketing channels in plain language — no dashboards, no tab switching.

| Channel | What you can ask |
|---|---|
| **Instagram** | Reach, engagement rate, top posts, follower growth, hashtag performance |
| **Google Ads** | Spend, CTR, CPC, conversions, CPA, ROAS by campaign and keyword |
| **Meta Ads** | Facebook & Instagram ad spend, reach, leads, messages, purchases |
| **Google Analytics 4** | Sessions, users, bounce rate, top pages, traffic sources |
| **Search Console** | Organic clicks, impressions, rankings, top queries and pages |
| **Reviews** | Google Business & Facebook ratings, response rate, recent reviews |
| **CRM / Leads** | Pipeline leads by status and source |
| **POS Sales** | Fudo restaurant sales by channel (dine-in, delivery, counter) |

### Example prompts

- *"How did our Instagram perform last month compared to the month before?"*
- *"Which Google Ads campaign has the best ROAS this quarter?"*
- *"What are our top 5 organic search queries this week?"*
- *"Show me all 1-star reviews from the last 30 days"*
- *"How many leads did we get from Meta Ads vs organic in January?"*

---

## Compatible clients

Use a client that supports Streamable HTTP with a custom `Authorization` header, or a local stdio bridge such as `mcp-remote`.

OAuth-only connector flows are currently unavailable. A client must be able to supply the per-account bearer token.

---

## Authentication

Maqui uses a bearer API token issued from the Maqui admin panel. Each token is scoped to one account, expires after one year and can be revoked independently. The endpoint requires authentication for both introspection and data queries.

**OAuth is currently disabled.** There is no automatic OAuth login or PKCE flow available at the public endpoint. The OAuth discovery and authorization endpoints return `404` with `oauth_temporarily_disabled`.

Access is provisioned by Maqui. A listing in an MCP directory does not provide an account or a token.

### Direct Streamable HTTP

For clients that accept remote MCP URLs and custom headers:

```json
{
  "mcpServers": {
    "maqui": {
      "url": "https://app.somosmaqui.com/api/mcp",
      "headers": {
        "Authorization": "Bearer YOUR_API_TOKEN"
      }
    }
  }
}
```

### Local stdio bridge

For clients that launch a local MCP process (requires Node.js):

```json
{
  "mcpServers": {
    "maqui": {
      "command": "npx",
      "args": [
        "-y",
        "mcp-remote",
        "https://app.somosmaqui.com/api/mcp",
        "--header",
        "Authorization:Bearer ${MAQUI_TOKEN}"
      ],
      "env": {
        "MAQUI_TOKEN": "YOUR_API_TOKEN"
      }
    }
  }
}
```

Keep real tokens out of public repositories and shared configuration examples.

---

## For agencies

Maqui is built for marketing agencies managing multiple clients. Each client account has an isolated dataset and its own API token — there is no cross-account data access.

If you manage several clients, each gets their own token scoped to their data. When a client relationship ends, revoke that client's token from the admin panel and their access stops immediately — the other clients' tokens keep working. You can configure multiple instances in your AI client:

```json
{
  "mcpServers": {
    "maqui-cliente-a": {
      "url": "https://app.somosmaqui.com/api/mcp",
      "headers": { "Authorization": "Bearer TOKEN_CLIENTE_A" }
    },
    "maqui-cliente-b": {
      "url": "https://app.somosmaqui.com/api/mcp",
      "headers": { "Authorization": "Bearer TOKEN_CLIENTE_B" }
    }
  }
}
```

---

## About Maqui

[Maqui](https://somosmaqui.com) is a marketing analytics platform for restaurants, retail, and service businesses. It syncs data daily from Instagram, Google Ads, Meta Ads, Google Analytics, Search Console, Google Business reviews, and POS systems — and makes it queryable through a conversational interface.

→ [somosmaqui.com](https://somosmaqui.com)
