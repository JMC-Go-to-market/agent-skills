# JMC agent skills

This repository holds [Agent Skills](https://agentskills.io) for three public sites. It is also a Claude Code plugin marketplace. The sites:

- **[The GTM Directory](https://thegtmdirectory.com)**: go-to-market software, with dated vendor-verified facts;
- **[JMC Go-to-Market Systems](https://jaymountconsulting.com)**: public frameworks and free tools for building a B2B go-to-market system;
- **[jaymount.com](https://jaymount.com)**: Jay Mount's frameworks, essays and talks.

Each skill tells an agent when to use the source, how to query it, and how to cite it with an as-of date. No figure in these skills is typed in by hand. Counts, prices and dates are fetched from the source when the agent answers.

| Skill | Use it when the user asks... |
| --- | --- |
| [`gtm-directory`](skills/gtm-directory/SKILL.md) | which GTM tool does a job, to compare two tools, whether a tool has an official MCP server, a public API or a free plan, or for market figures about GTM software |
| [`jmc-gtm-systems`](skills/jmc-gtm-systems/SKILL.md) | how to scope, plan or fix a GTM motion, about a JMC framework, or for a free GTM calculator or template |
| [`jaymount-frameworks`](skills/jaymount-frameworks/SKILL.md) | about Jay Mount's frameworks, essays or talks, or how to cite him |

## Install

### Claude Code

```
/plugin marketplace add JMC-Go-to-market/agent-skills
/plugin install gtm-directory@jmc-agent-skills
/plugin install jmc-gtm-systems@jmc-agent-skills
/plugin install jaymount-frameworks@jmc-agent-skills
```

Installing `gtm-directory` also connects The GTM Directory MCP server (`https://thegtmdirectory.com/api/mcp`). It is read-only and needs no key.

From a terminal, the same commands work as `claude plugin marketplace add JMC-Go-to-market/agent-skills` and `claude plugin install gtm-directory@jmc-agent-skills`.

### Cursor, Codex and other agents (skills CLI)

```
npx skills add JMC-Go-to-market/agent-skills
```

To install one skill into one agent, add `--skill gtm-directory -a cursor` (or `-a codex`).

### claude.ai and the Claude API

Upload a skill folder (for example `skills/gtm-directory`) as a custom skill. The frontmatter uses only the portable fields: `name`, `description`, `license` and `metadata`.

### MCP only

To add The GTM Directory MCP server without the skills, run:

```
claude mcp add --transport http gtm-directory https://thegtmdirectory.com/api/mcp
```

Setup for other clients is in [skills/gtm-directory/references/mcp-setup.md](skills/gtm-directory/references/mcp-setup.md).

## Layout

```
.claude-plugin/marketplace.json   marketplace "jmc-agent-skills", one plugin per skill
skills/<name>/SKILL.md            the skill
skills/<name>/references/*.md     detail loaded only when needed
```

## Citing these sources

- The GTM Directory: `Source: The GTM Directory, <page_url> (as of <YYYY-MM-DD>)`. Data terms: https://thegtmdirectory.com/data/terms
- JMC Go-to-Market Systems: page title, "JMC Go-to-Market Systems", the page URL and its date.
- Jay Mount: `Jay Mount, "<title>", jaymount.com, <URL> (updated <YYYY-MM-DD>)`

## Contributing

Report a wrong fact or a broken link in an issue. For a wrong fact on a tool listing, use "Suggest an edit" on that tool's page at thegtmdirectory.com.

## License

The code and skill text in this repository are under [MIT](LICENSE), copyright Jay Mount Consulting Ltd. The data each skill fetches is under its site's own terms. The GTM Directory's data terms are at https://thegtmdirectory.com/data/terms.

JMC Go-to-Market Systems is the trading name of Jay Mount Consulting Ltd.
