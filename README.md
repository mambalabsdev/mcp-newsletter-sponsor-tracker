# Newsletter Sponsor Tracker MCP Server

[![Smithery](https://smithery.ai/badge/mambabuilt/mcp-newsletter-sponsor-tracker)](https://smithery.ai/servers/mambabuilt/mcp-newsletter-sponsor-tracker) [![Glama score](https://glama.ai/mcp/servers/mambalabsdev/mcp-newsletter-sponsor-tracker/badges/score.svg)](https://glama.ai/mcp/servers/mambalabsdev/mcp-newsletter-sponsor-tracker) [![MCP Registry](https://img.shields.io/badge/dynamic/json?url=https%3A%2F%2Fregistry.modelcontextprotocol.io%2Fv0%2Fservers%3Fsearch%3Dcom.mambabuilt%252Fmcp-newsletter-sponsor-tracker%26limit%3D1&query=%24.servers%5B0%5D._meta%5B%22io.modelcontextprotocol.registry%2Fofficial%22%5D.status&label=mcp%20registry&color=blue)](https://registry.modelcontextprotocol.io/v0/servers?search=com.mambabuilt/mcp-newsletter-sponsor-tracker&limit=1) [![npm version](https://img.shields.io/npm/v/@mambalabsdev/mcp-newsletter-sponsor-tracker)](https://www.npmjs.com/package/@mambalabsdev/mcp-newsletter-sponsor-tracker) [![npm downloads](https://img.shields.io/npm/dm/@mambalabsdev/mcp-newsletter-sponsor-tracker)](https://www.npmjs.com/package/@mambalabsdev/mcp-newsletter-sponsor-tracker) [![license](https://img.shields.io/github/license/mambalabsdev/mcp-newsletter-sponsor-tracker)](https://github.com/mambalabsdev/mcp-newsletter-sponsor-tracker/blob/main/LICENSE) [![mcpservers.org](https://img.shields.io/badge/mcpservers.org-listed-blue)](https://mcpservers.org/servers/mambalabsdev/mcp-newsletter-sponsor-tracker)

An MCP server for the Mamba Labs Newsletter Sponsor Tracker actor on Apify. Give it newsletter links to see who sponsors them, or a brand to see which newsletters it sponsors.

## What's Inside

- [What it does](#what-it-does)
- [Quick start](#quick-start)
- [Prerequisites](#prerequisites)
- [Example prompts](#example-prompts)
- [Tool](#tool)
- [Inputs](#inputs)
- [Pricing](#pricing)
- [Your keys](#your-keys)
- [Full actor documentation](#full-actor-documentation)
- [License](#license)

## What it does

Two modes:

- **newsletter**: give `newsletter_urls`. You get one row per sponsor found in the newest issues: sponsor name, domain, link without tracking, placements, share of issues, first and last issue seen, and issue links, plus one free summary row per newsletter.
- **brand**: give `brand_name` and `brand_domain`. You get the newsletters whose recent issues carry a sponsor placement for the brand, found through Google results and confirmed in each newsletter's own issues.

Archives are read from Substack archive JSON, beehiiv and other sitemaps, or RSS. A placement is a short block with a sponsor label and the first outside link beside it.

All of the work runs on Apify. This package is a thin client that starts the actor run, waits for it, and hands back the dataset rows.

## Quick start

You need Node.js 18 or newer and an Apify account with an API token.

Add this to your Claude Desktop config (`claude_desktop_config.json`), Cursor, or any MCP client:

```json
{
  "mcpServers": {
    "mamba-newsletter-sponsor-tracker": {
      "command": "npx",
      "args": [
        "-y",
        "@mambalabsdev/mcp-newsletter-sponsor-tracker"
      ],
      "env": {
        "APIFY_TOKEN": "your-apify-token"
      }
    }
  }
}
```

Restart the client. The `track_newsletter_sponsors` tool appears.

## Prerequisites

- Node.js 18 or newer.
- An Apify API token from [console.apify.com/account/integrations](https://console.apify.com/account/integrations), set as `APIFY_TOKEN`. The server lists its tool without a token; a call needs one.

## Example prompts

- "Who sponsored Not Boring and Superhuman AI in the last 20 issues?"
- "Which newsletters has Attio sponsored recently?"
- "List the sponsors of these five AI newsletters with how often each appeared."

## Tool

`track_newsletter_sponsors`. One tool. Example arguments:

```json
{
  "mode": "newsletter",
  "newsletter_urls": [
    "https://www.notboring.co"
  ],
  "issues_to_scan": "20"
}
```

## Inputs

Every buyer facing input of the actor, unchanged. Omitted inputs take the actor's own defaults. Numbers and booleans are sent as strings.

| Input | Type | Description |
|---|---|---|
| `mode` | string | newsletter (default) or brand |
| `newsletter_urls` | array | Newsletter mode. Home page links or domains, up to 100 |
| `brand_name` | string | Brand mode. The sponsor name |
| `brand_domain` | string | Brand mode. The sponsor domain |
| `newsletters_to_check` | string | Brand mode. 1 to 10. Default 10 |
| `issues_to_scan` | string | 1 to 200; at most 20 in brand mode. Default 20 |
| `since_days` | string | Date window in days. Default 365 |
| `max_charge_usd` | string | Optional cost cap for the run |

## Pricing

Pay per event on Apify, billed to your Apify account. Prices in USD.

| Event | Free plan | Bronze | Silver | Gold |
|---|---|---|---|---|
| `sponsor-row`, per sponsor of one newsletter | $0.004 | $0.0038 | $0.0036 | $0.0034 |
| `brand-search`, brand mode only, once per run | $0.065 | $0.06175 | $0.0585 | $0.05525 |
| `apify-actor-start`, per run start | $0.0005 | $0.0005 | $0.0005 | $0.0005 |

Newsletter summary rows, no results rows, and error rows are free. The brand search event is charged when at least one candidate newsletter was read.

## Your keys

The actor needs no key. It reads public newsletter archives, sitemaps, and feeds, and in brand mode Google result pages through the Apify proxy, and holds no key of any kind. Mamba Labs pays the platform and proxy costs of the run.

## Full actor documentation

[apify.com/mambalabs/newsletter-sponsor-tracker](https://apify.com/mambalabs/newsletter-sponsor-tracker)

## License

MIT. See [LICENSE](LICENSE).
