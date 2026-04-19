# Brief plugins for Claude Code

Official Claude Code marketplace for [Brief](https://briefhq.ai) — your Product Navigator. Brings your company's product context, decisions, and customer insights into every Claude Code session.

## Install

Add the marketplace and install the `brief` plugin:

```
/plugin marketplace add brief-hq/brief-plugins
/plugin install brief@brief-plugins
```

Restart Claude Code. On first use the browser opens for OAuth — no API keys.

## Plugins in this marketplace

| Plugin | Description |
|--------|-------------|
| [`brief`](plugins/brief) | Product context, decisions, and customer insights via the Brief MCP server. Adds `/brief-onboard` and `/brief-ask` commands plus a `brief` skill. |

## What Brief does

Brief holds the business context, customer insights, and strategic decisions that live outside any single document or conversation. It turns that knowledge into structured intelligence Claude can reason over:

- **Product context** — strategy, personas, features, competitive landscape
- **Decision capture** — record choices and rationale so they compound over time
- **Customer insights** — surface feedback themes, sentiment, and signals
- **Team awareness** — who's working on what, current priorities, open questions

## Team deployment

Teams can auto-install by adding to `.claude/settings.json`:

```json
{
  "extraKnownMarketplaces": {
    "brief-plugins": {
      "source": { "source": "github", "repo": "brief-hq/brief-plugins" }
    }
  },
  "enabledPlugins": { "brief@brief-plugins": true }
}
```

## Learn more

- [briefhq.ai](https://briefhq.ai) — Brief product site
- [docs.briefhq.ai/mcp](https://docs.briefhq.ai/mcp) — MCP integration docs
- [Issues](https://github.com/brief-hq/brief-plugins/issues) — report bugs or request plugins

## License

[ISC](./LICENSE)
