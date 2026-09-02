# Stienhardt store on MCP

A small New York lab-grown diamond jeweler's live inventory, cart, and checkout, reachable by any
Model Context Protocol client. The endpoint is Shopify's own per-store Universal Commerce Protocol
(UCP) server; this repository registers it in the official MCP Registry and documents what an
agent can and cannot do with it. Disclosure up front: we sell rings. Nothing here appraises or
verifies a stone; the tools point you to the grading lab's own verification page.

- Registry entry: `io.github.JacobiusMakes/stienhardt-store` (published 2026-09-01)
- Endpoint: `https://stienhard-stones.myshopify.com/api/ucp/mcp` (streamable HTTP, 13 tools)
- Discovery: `https://stienhardt.com/.well-known/ucp` (UCP 2026-08-25; Shop Pay, Google Pay, card)
- Hosted helper: `https://diamond-mcp.stienhardt.workers.dev/mcp` (education tools plus live
  inventory search, with the agent profile UCP requires injected for you)

## Try it (no signup, nothing to install)

**Claude.ai** (any plan): Customize, Connectors, Add custom connector, name `Stienhardt`, URL
`https://diamond-mcp.stienhardt.workers.dev/mcp`, auth None. Then ask: "Find a 2 carat Dutch
Marquise in stock and show me how big it reads in millimeters."

**Claude Code**:

```
claude mcp add --transport http stienhardt https://diamond-mcp.stienhardt.workers.dev/mcp
```

**MCP Inspector** against the raw Shopify endpoint:

```
npx @modelcontextprotocol/inspector --transport streamable-http --server-url https://stienhard-stones.myshopify.com/api/ucp/mcp
```

`initialize` and `tools/list` answer anonymously. Every `tools/call` must carry
`arguments.meta.ucp-agent.profile`, a URL to a UCP platform profile; see the gating table below.

## What a self-registered agent can and cannot do (probed 2026-09-02)

| Step | Result |
|---|---|
| `search_catalog` "Dutch Marquise 2 carat" | Ten live products with report-certified titles, prices, and links |
| `create_cart` with one variant | A cart with totals and a `continue_url` |
| `create_checkout` | A checkout object: status `incomplete`, totals, refund, shipping, privacy and terms links, `payment.instruments: []`, and a `continue_url` to Stienhardt's hosted Shopify checkout |
| `complete_checkout` | Not self-serve. Shopify grants direct completion case by case; the buyer finishes at the `continue_url` (Shop Pay, Google Pay, or card) |

So the honest sentence is: an assistant can search live inventory, read the report numbers, build a
cart, open a checkout, and hand you the link to pay. It does not pay for you.

## The one thing an agent must know (capability gating)

Shopify fetches the profile URL you pass, requires `Content-Type: application/json` (GitHub raw
fails with `profile_malformed`; a CDN such as jsDelivr works), requires a `payment_handlers` block,
and exposes only the tools whose capabilities the profile declares. Declare too little and the
tools "do not exist".

| Tool family | Capability key the profile must declare |
|---|---|
| search_catalog | dev.ucp.shopping.catalog.search |
| lookup_catalog, get_product | dev.ucp.shopping.catalog.lookup |
| cart tools | dev.ucp.shopping.cart |
| checkout tools | dev.ucp.shopping.checkout (plus fulfillment, discount) |

A working profile lives at
`https://cdn.jsdelivr.net/gh/JacobiusMakes/diamond-mcp@<sha>/agent-profile.json` (source in the
diamond-mcp repo). Pin the commit sha; jsDelivr caches `@main` for days. Shopify's fetcher also
refuses `*.workers.dev` hosts, which is why the hosted helper points at the CDN copy.

Generic MCP clients do not send this profile on their own. The hosted helper injects it and exposes
two clean tools, `search_inventory` and `get_product`, beside the eight open education tools.

## The open education stack

- diamond-mcp on npm (MIT): eight tools over a sourced, dated facts file and a 90-entry gemology
  encyclopedia. https://github.com/JacobiusMakes/diamond-mcp
- Hugging Face dataset `JacobiusMakes/diamond-gemology-encyclopedia` (MIT).
- "Dutch Marquise: Open Geometry Specification", DOI 10.5281/zenodo.21938900. A Dutch Marquise is
  an elongated hexagonal cut diamond, cut for brilliance; IGI reports read it as "Hexagonal
  Modified Brilliant".

## Consumer-safety notes (they apply when an agent shops for jewelry)

- Every agent-facing title carries the lab-grown qualifier (16 CFR 23.12).
- The buyer confirms in a trusted checkout before any payment (UCP checkout spec); agents never
  hold card data.
- Each stone's lab and report number are in the product data; verify them on the lab's own report
  lookup before you pay.
- Returns and resizing are exposed in the checkout's policy links, because an agent buyer cannot
  try on.

## Files

- `server.json`: the registry manifest (GitHub namespace, remote streamable-http).
- `PUBLISH-RUNBOOK.md`: the two-command publish.

Stienhardt sources certified Lab Grown Diamonds and hand-sets and finishes rings in New York City,
by appointment. Store: https://stienhardt.com/?utm_source=github&utm_medium=readme&utm_campaign=mcp-store
Questions: jgalperin@stienhardt.com.
