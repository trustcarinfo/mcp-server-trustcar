# mcp-server-trustcar

MCP (Model Context Protocol) server for **[trustcar.info](https://trustcar.info)** — a Ukrainian licence-plate lookup and driver review service. Ask your AI assistant *"что за машина AA1234BC?"* and it will answer with the registration region, vehicle make/model/year, community rating and recent reviews.

This package is a thin stdio bridge to the site's native MCP endpoint (`https://trustcar.info/mcp`). Tools are discovered at startup, so the package never goes stale.

## Tools

| Tool | Description | Auth |
|------|-------------|------|
| `lookup_plate` | Region, vehicle (make/model/year/fuel), rating and recent reviews for a Ukrainian licence plate | none |
| `recent_plates` | Recently active plates on trustcar.info (with vehicle make/model where known) | none |
| `post_review` | Post a licence-plate review on behalf of an authenticated user (labelled as AI-agent-generated, shown separately, does not affect the community rating) | OAuth bearer |

## Install

### Claude Code

```bash
claude mcp add trustcar -- npx -y mcp-server-trustcar
```

### Claude Desktop

```json
{
  "mcpServers": {
    "trustcar": {
      "command": "npx",
      "args": ["-y", "mcp-server-trustcar"]
    }
  }
}
```

### Cursor / Windsurf / other MCP clients

Any client that speaks stdio MCP works the same way: command `npx`, args `-y mcp-server-trustcar`.

### Remote (no install)

MCP clients that support remote servers can skip this package entirely and connect straight to:

```
https://trustcar.info/mcp
```

## Configuration

| Env var | Meaning |
|---------|---------|
| `TRUSTCAR_MCP_URL` | Override the endpoint URL (default `https://trustcar.info/mcp`) |
| `TRUSTCAR_TOKEN` | OAuth bearer token — only needed for `post_review`. Discovery: [`/.well-known/oauth-protected-resource`](https://trustcar.info/.well-known/oauth-protected-resource) |

## Example

> **User:** Что за машина AA1234BC?
>
> **Assistant:** *(calls `lookup_plate`)* AA1234BC is registered in Kyiv (AA region). It's a Toyota Camry, 2019, petrol. Community rating on trustcar.info: 4/5 from 3 reviews, the latest one mentions polite driving…

## Development

```bash
npm install
npm run build
npm test
```

## Related

- [trustcar.info](https://trustcar.info) — the service itself (uk/ru/en)
- API docs: [trustcar.info/.well-known/api-catalog](https://trustcar.info/.well-known/api-catalog)

## License

MIT © [trustcar.info](https://trustcar.info)
