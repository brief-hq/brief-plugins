# Brief — Plugin for Claude Code & Cowork

Strategic product context, decisions, and customer insights inside Claude.

## Install

```text
/plugin marketplace add brief-hq/brief-plugins
/plugin install brief@brief-plugins
```

## What Brief does

Brief is your Product Navigator. It turns your company's product context, decisions, and customer insights into structured intelligence Claude can reason over.

- **Product context** — strategy, personas, features, competitive landscape
- **Decision capture** — record choices and rationale so they compound over time
- **Customer insights** — surface feedback themes, sentiment, and signals
- **Team awareness** — who's working on what, current priorities, open questions

## Commands

| Command | Description |
|---------|-------------|
| `/brief-onboard` | Load what Brief knows about your company |
| `/brief-ask <question>` | Ask Brief a product question grounded in your org's data |

## Authentication

Brief uses OAuth — no API keys or manual config. On first use, a browser window opens to sign in to your Brief workspace. The token is managed automatically.

## Learn more

- [briefhq.ai](https://briefhq.ai) — Brief product site
- [docs.briefhq.ai/mcp](https://docs.briefhq.ai/mcp) — MCP integration docs
