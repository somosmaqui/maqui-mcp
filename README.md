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

Connect an MCP client that supports Streamable HTTP and OAuth authorization with PKCE. Maqui accepts approved client registrations or Client ID Metadata Documents from approved origins. Unrecognized clients and callback URLs are rejected; contact Maqui to request support for another client.

Clients that accept a custom `Authorization` header can also use a per-account API token. A local stdio bridge such as `mcp-remote` supports this alternative.

---

## Authentication

OAuth is available at the public endpoint with resource discovery, PKCE S256 and explicit consent. Sign in with your Maqui account, then approve read access. OAuth access tokens expire after one hour. When the client requests `offline_access`, refresh tokens rotate and expire within 30 days; reuse revokes the token family. Older OAuth connections must reconnect.

Protected resource metadata: `https://app.somosmaqui.com/.well-known/oauth-protected-resource/api/mcp`.
Authorization server metadata: `https://app.somosmaqui.com/.well-known/oauth-authorization-server`.

Existing bearer API tokens remain available from the Maqui admin panel. Each API token is scoped to one account, expires after one year and can be revoked independently. The endpoint requires a valid token for both tool discovery and data queries. Access to real business data is provisioned by Maqui.

### Synthetic demo

Choose **Probar solo con datos sintéticos** on the OAuth consent page to try all 22 tools without signing in. Demo responses are explicitly marked and use fictional examples for September 2026. They never query client metrics, and changing `account_id` cannot switch a demo token to real data. The requested date range is shown separately from the fixed sample period; the demo does not apply every filter.

Directory health checks can use a separate, revocable demo bearer token issued by Maqui, valid for 30 days. Never publish that credential in a README or repository. A Glama listing does not mean its test profile has already been configured.

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
