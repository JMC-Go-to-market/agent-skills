# The GTM Directory: public endpoints

Base URL: `https://thegtmdirectory.com`. Read-only, no key, CORS open. The OpenAPI description is at `/openapi.json`, and it wins over this page if they disagree.

## JSON API

| Endpoint | Returns | Date field to cite |
| --- | --- | --- |
| `GET /api/v1/tools` | Search results. Params: `q` (the job in plain words), `category` (slug), `pricing` (`free`, `freemium`, `paid`, `open_source`, `enterprise`), `has_mcp` (`true`/`false`), `has_api` (`true`/`false`), `limit` (up to 50), `offset`. `meta.total` is the number of matching published tools. | Fetch date (a live count) |
| `GET /api/v1/tools/{slug}` | One tool: tagline, best_for, description, category, subcategory, pricing_tier, has_api, has_mcp_server, is_open_source, features, pros, cons, use_cases, `verified_facts {as_of, facts[]}`, `last_verified`, `page_url`, `markdown_url`. | `verified_facts.as_of`, else `last_verified` |
| `GET /api/v1/tools/{slug}/card` | A small card: name, `url`, up to two facts, each with `source` (the vendor page) and `checked`. | `checked` on each fact |
| `GET /api/v1/categories` | Each category with its `slug`, `name`, `tool_count` and `page_url`. | Fetch date (a live count) |

Each response's `meta` carries `license` and `attribution`. Keep them with anything you store.

## MCP

`POST https://thegtmdirectory.com/api/mcp`: Streamable HTTP, stateless, JSON responses. A plain browser GET returns 406, which is expected. `https://thegtmdirectory.com/mcp` is the human setup page, not the endpoint.

| Tool | Input | Use it for |
| --- | --- | --- |
| `search_tools` | `query`, `category`, `pricing`, `has_mcp`, `has_api`, `limit`, `offset` | Tools for a job |
| `get_tool` | `slug` | One full listing with dated vendor-verified facts |
| `list_categories` | none | Category slugs and tool counts |
| `compare_tools` | `a`, `b` (slugs) | Two tools side by side. Each fact comes with its source URL and checked date. |
| `recommend_stack` | `job`, plus optional `needs_mcp`, `needs_api`, `free_plan`, `clay_native` | A ranked shortlist for each stack slot, using only vendor-verified matches |

Every `page_url` in an MCP result carries a `utm_source=tgd-mcp` tag. Keep it as returned.

## Markdown pages

Append `.md` to any page URL. Each one starts with its title, then `Canonical:` (the URL to cite) and `Last verified:` (the date to cite).

- `/index.md`: home
- `/tools/{slug}.md` and `/tools/{slug}/alternatives.md`
- `/compare/{a}-vs-{b}.md`: only for pairs that have been written up; follow any redirect
- `/category/{slug}.md` and `/category/{slug}/{subcategory}.md`
- `/best/{subcategory}.md`: a ranked list for one job
- `/data.md`: vendor-verified market figures, with the denominator, check date and method
- `/research/{slug}.md`: studies, with dated editions at `/research/{slug}/{YYYY-MM}.md`
- `/jobs.md`, `/list-building.md`, `/persona.md`: the slugs that `recommend_stack` accepts

Site guide for agents: `/llms.txt`. One line per enriched tool: `/llms-full.txt`.

## Data pages for people

- `/data`: what GTM tools say on their own websites (vendor-verified facts and method)
- `/data/pricing`: how GTM tools price
- `/data/clusters`: tool cluster indices
- `/research`: original studies
- `/data/terms`: data license
