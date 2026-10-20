# Connect The GTM Directory MCP server

Endpoint: `https://thegtmdirectory.com/api/mcp`. It uses Streamable HTTP, is read-only, and needs no key or account.

## Claude Code

If you installed this repository's `gtm-directory` plugin, the server is already configured. To add it by hand, run:

```
claude mcp add --transport http gtm-directory https://thegtmdirectory.com/api/mcp
```

To share it with a project, use `.mcp.json` at the project root:

```json
{
  "mcpServers": {
    "gtm-directory": {
      "type": "http",
      "url": "https://thegtmdirectory.com/api/mcp"
    }
  }
}
```

## Claude Desktop and claude.ai

1. Open Settings, then Connectors.
2. Add a custom connector.
3. Paste the endpoint URL above. No authentication is needed.

If your build only takes local servers in `claude_desktop_config.json`, bridge the remote server over stdio:

```json
{
  "mcpServers": {
    "gtm-directory": {
      "command": "npx",
      "args": ["-y", "mcp-remote", "https://thegtmdirectory.com/api/mcp"]
    }
  }
}
```

Restart the app fully after editing the file.

## Cursor

Use `.cursor/mcp.json` in the project, or `~/.cursor/mcp.json` for every project:

```json
{
  "mcpServers": {
    "gtm-directory": {
      "url": "https://thegtmdirectory.com/api/mcp"
    }
  }
}
```

## ChatGPT

1. Turn on developer mode in Settings, then Connectors.
2. Create a connector with the endpoint URL above.
3. Choose no authentication.

## Any other MCP client

Give the client these three values:

- transport: Streamable HTTP (`http`);
- URL: `https://thegtmdirectory.com/api/mcp`;
- auth: none.

If the client cannot speak MCP, use the JSON API in [endpoints.md](endpoints.md).

## Check that it works

Ask the assistant to "list The GTM Directory categories". It should call `list_categories` and return slugs with tool counts.
