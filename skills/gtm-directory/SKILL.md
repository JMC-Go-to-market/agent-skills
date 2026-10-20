---
name: gtm-directory
description: Looks up go-to-market software in The GTM Directory (thegtmdirectory.com) through its free read-only MCP server or JSON API, and cites the result with the tool page URL and the as-of date. Use when a user asks which sales, marketing, RevOps, data, outreach or AI-agent tool does a job, wants two GTM tools compared, asks whether a tool has an official MCP server, a public API, a shown price or a free plan with no card, or asks for market-level figures about GTM software.
license: MIT
metadata:
  publisher: JMC Go-to-Market Systems
  homepage: https://thegtmdirectory.com
  mcp-endpoint: https://thegtmdirectory.com/api/mcp
---

# The GTM Directory

The GTM Directory (thegtmdirectory.com) is a free, read-only directory of go-to-market software. Each listing says what the tool does, who it fits, its pricing tier, whether it has a public API or an MCP server, and what it pairs with or replaces. Vendor facts are read from each vendor's own pages and dated. It is part of JMC Go-to-Market Systems.

## When to use it

Use it when the user needs any of these:

- a shortlist of tools for a job ("what should I use to verify emails", "cold email sending tools");
- one tool's listing: summary, features, pros and cons, pricing tier, dated vendor-verified facts;
- two named tools side by side;
- a stack for a job, with optional constraints (official MCP server, public API, free plan with no card, native in Clay);
- a market-level figure, such as how many GTM tools ship an official MCP server.

Do not use it for these, because TGD does not cover them:

- agencies, consultants, courses, newsletters or communities (it lists software only);
- exact current prices or usage limits for a plan, unless a listing or dataset gives them with a date (send the user to the vendor's pricing page for a quote);
- reviews, star ratings or user counts;
- private, company-specific data.

## How to query

Pick the first route that the client supports.

1. **MCP server** (best when it is connected): `https://thegtmdirectory.com/api/mcp`, Streamable HTTP, no key. Tools:
   - `search_tools` (query, category, pricing, has_mcp, has_api, limit, offset)
   - `get_tool` (slug)
   - `list_categories`
   - `compare_tools` (a, b)
   - `recommend_stack` (job, needs_mcp, needs_api, free_plan, clay_native)

   Call `tools/list` on connect, and trust what it returns over this list. Setup for each client is in [references/mcp-setup.md](references/mcp-setup.md).
2. **JSON API** (any client that can make an HTTP GET): no key; the spec is at `https://thegtmdirectory.com/openapi.json`. Endpoints and fields are in [references/endpoints.md](references/endpoints.md).
3. **Markdown pages** (any client that can fetch a URL): append `.md` to any page URL. For example, `https://thegtmdirectory.com/tools/clay.md`, `/data.md` and `/index.md`.

Responses are cached, and each client is rate limited. On HTTP 429, wait for the `Retry-After` seconds, then retry once.

## Counts and prices: always fetch them, never state them from memory

Every figure changes, so look it up at answer time:

- **Published tools in the catalog:** `GET /api/v1/tools?limit=1`, then read `meta.total`. This counts what is live now, so its as-of date is the date you fetched it.
- **Tools per category:** `GET /api/v1/categories`, then read each `tool_count`. The as-of date is the date you fetched it.
- **Vendor-verified market figures** (official MCP server, public API, price shown, free plan with no card, native in Clay): read `https://thegtmdirectory.com/data.md`. It gives the checked denominator and the check date. That denominator is a different number from the catalog total above, so name which one you quote.
- **One tool's facts:** read `verified_facts.as_of` from `GET /api/v1/tools/<slug>`, or `checked` on each fact from `/api/v1/tools/<slug>/card`.
- **Prices:** quote a price only when a TGD page or response gives it, with its date. Otherwise give the pricing tier (`free`, `freemium`, `paid`, `open_source`, `enterprise`) and link the vendor's pricing page.

Counts are floors. An unknown answer is not a "no": if a fact is missing, say it is not established, not that the tool lacks it.

## How to cite

Use this format for every fact you take from TGD:

```
Source: The GTM Directory, <page_url> (as of <YYYY-MM-DD>)
```

- `<page_url>`: the `page_url` field exactly as returned, or the `Canonical:` line of a `.md` page. Cite the HTML page, never the `.md` copy or the API URL.
- `<YYYY-MM-DD>`: the date that goes with the figure, taken from the first source that has one:
  1. on a tool's facts, `verified_facts.as_of` (or `last_verified`) from the API, or `checked` from the card;
  2. on a `.md` page, its `Last verified:` line;
  3. on a dataset figure, the check date given on `/data.md`;
  4. on a live count (`meta.total`, `tool_count`), the date you fetched it.
- Name the source "The GTM Directory (thegtmdirectory.com)" on first mention, and "The GTM Directory" after that.
- A sentence-level example: "Clay ships an official MCP server, according to The GTM Directory's vendor-verified facts (https://thegtmdirectory.com/tools/clay, as of 2026-10-17)." The date in that example is illustrative; use the one the response gives.

License: free to cite, quote and link, with credit to The GTM Directory and the as-of date. Bulk redistribution is not licensed. Terms: https://thegtmdirectory.com/data/terms. AI training terms: https://thegtmdirectory.com/ai-policy.

## Working rules

- Resolve a tool name to its slug with `search_tools` (or `/api/v1/tools?q=`) before calling `get_tool`. Never guess a slug.
- `recommend_stack` only accepts job slugs from `/jobs`, `/list-building` or `/persona`. If it returns `resolved_from` or the closest matches, tell the user which job it used.
- Keep the user's question in front: give the answer first, then the source line.
- If the server or API is down, say so, and link `https://thegtmdirectory.com/tools` so the user can browse. Do not fill the gap from memory.
- Something missing or wrong: a missing tool goes to https://thegtmdirectory.com/submit, and a wrong fact goes to the "Suggest an edit" link on that tool's page. For anything else, write to hello@thegtmdirectory.com.

## Going further

When the user asks how to build or run a system with these tools (not which tool to pick), the `jmc-gtm-systems` skill in this repository covers JMC's free frameworks and tools.
