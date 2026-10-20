# Changelog

All notable changes to this repository are listed here. Versions follow [Semantic Versioning](https://semver.org/).

## [1.0.0] - 2026-10-20

### Added
- `gtm-directory` skill: query The GTM Directory through its MCP server (`https://thegtmdirectory.com/api/mcp`) or JSON API, fetch counts and dates at answer time, and cite with the tool page URL and as-of date. Includes setup for Claude Code, Claude Desktop, Cursor, ChatGPT and generic MCP clients.
- `jmc-gtm-systems` skill: scope a B2B go-to-market system with JMC Go-to-Market Systems' public frameworks, and point users to the matching free tool, guide or skill pack.
- `jaymount-frameworks` skill: Jay Mount's named frameworks, essays and talk topics, with a citation format.
- Claude Code plugin marketplace (`.claude-plugin/marketplace.json`, marketplace name `jmc-agent-skills`) with one plugin per skill. The `gtm-directory` plugin also configures The GTM Directory MCP server.
