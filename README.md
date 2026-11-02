# Maqui Analytics MCP Server

All-in-one marketing analytics MCP for local businesses and agencies: Instagram, ads, web traffic, SEO and reviews.

**Server URL:** `https://app.somosmaqui.com/api/mcp`

## Official registry

Canonical server name: **`com.somosmaqui/maqui`**, verified through the `somosmaqui.com` domain.

- [Official MCP Registry record](https://registry.modelcontextprotocol.io/v0.1/servers/com.somosmaqui%2Fmaqui/versions/latest)
- [Website and setup guide](https://somosmaqui.com/asistentes-ia)

The previous registry name, `io.github.luis-fuentes/maqui`, remains available during the transition for directories already linked to it. Both records point to the same hosted endpoint. Existing connections keep using that endpoint.

The version in `server.json` identifies a published registry record; the hosted service reports its dashboard build version separately. Release **1.0.1** updates the registry identity and adds the website URL. It does not change the MCP endpoint or authentication.

### Publishing registry metadata

Update `server.json` and the matching version in `package.json` when publishing a registry release. Published records are immutable, so use a new version for metadata changes. Changes to the hosted implementation do not require editing each directory individually; downstream synchronization depends on the directory.

Domain verification uses the public TXT record in `.registry/dns-record.txt`. The publishing key is excluded from Git in `.registry/.env`; its encrypted SOPS backup is `.registry/.env.enc`, using Maqui's existing age recipient. Never publish the plaintext key or registry authentication cache.

Validate with `mcp-publisher validate server.json`, authenticate using the domain key, then publish with `mcp-publisher publish server.json`. Publishing is a separate step from pushing this repository; no automatic registry publishing workflow is configured yet.

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
| **Microsoft Clarity** | Sessions, engagement, behavior signals, top pages and traffic sources |
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

Directory health checks can use a separate, revocable demo bearer token issued by an authenticated Maqui admin, valid for 30 days. In the dashboard, choose **Admin → Tokens MCP → Generar token demo para Glama**, verify the demo responses, then configure the directory's test profile. Never publish that credential in a README or repository. Use a separate credential for each directory.

The [Glama connector](https://glama.ai/mcp/connectors/io.github.luis-fuentes/maqui) was verified **Healthy on October 9, 2026**, with 22 indexed tools and a synthetic demo in **Admin → Test Profile → API Key → Bearer Authentication**. This test profile uses a demo bearer token; it does not establish OAuth compatibility with Glama. Renew the demo before its 30-day expiry to keep the checks active.

[![Maqui Analytics MCP connector](https://glama.ai/mcp/connectors/io.github.luis-fuentes/maqui/badges/score.svg)](https://glama.ai/mcp/connectors/io.github.luis-fuentes/maqui)

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
