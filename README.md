# Maqui Analytics MCP Server

All-in-one marketing analytics MCP — Instagram, ads, web traffic, SEO and reviews for local businesses and the agencies that manage them.

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

Works with any MCP-compatible AI client:

- [Claude Desktop](https://claude.ai/download) and [Claude.ai](https://claude.ai)
- [ChatGPT](https://chatgpt.com) (with MCP support enabled)
- [Cursor](https://cursor.com)
- [Windsurf](https://codeium.com/windsurf)
- [Cline](https://github.com/cline/cline)
- [Continue](https://continue.dev)
- Any client that supports the [Model Context Protocol](https://modelcontextprotocol.io)

---

## Authentication

Maqui uses per-account authentication. Each client account has its own token — data is always scoped to that account.

### Option A: OAuth 2.0 (recommended for Claude.ai, ChatGPT and modern clients)

Clients that support OAuth 2.0 can connect automatically. Maqui implements the full PKCE flow:

| Endpoint | URL |
|---|---|
| Discovery | `https://app.somosmaqui.com/.well-known/oauth-authorization-server` |
| Authorization | `https://app.somosmaqui.com/oauth/authorize` |
| Token | `https://app.somosmaqui.com/oauth/token` |

Just add the server URL to your client — it will handle login automatically.

### Option B: Bearer token (for Claude Desktop and manual configuration)

Get your API token from the Maqui dashboard, then configure your client manually.

**Claude Desktop** — add to `~/Library/Application Support/Claude/claude_desktop_config.json`:

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

**Other clients** — use the server URL with an `Authorization: Bearer YOUR_API_TOKEN` header.

> API tokens are generated per account from the Maqui admin panel and are valid for one year.

---

## For agencies

Maqui is built for marketing agencies managing multiple clients. Each client account has an isolated dataset and its own API token — there is no cross-account data access.

If you manage several clients, each gets their own token scoped to their data. You can configure multiple instances in your AI client:

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
