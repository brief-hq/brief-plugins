# Brief — Plugin for Claude Code & Cowork

Brief is a product strategy agent that answers from the workspace's decisions, personas, strategy, customers, and competitive landscape.

## Install

In Claude Code:

```text
/plugin marketplace add brief-hq/brief-plugins
/plugin install brief@brief-plugins
```

In Cowork or Claude Desktop, open **Customize > Plugins**, select **Add > Add marketplace**, choose **Add from a repository**, and enter `brief-hq/brief-plugins`. Then add Brief from that marketplace.

## What Brief does

Ask Brief product questions grounded in your workspace's context.

- **Product context** — strategy, personas, features, competitive landscape
- **Decision capture** — record choices and rationale so they compound over time
- **Customer insights** — surface feedback themes, sentiment, and signals
- **Team awareness** — who's working on what, current priorities, open questions

## Commands

| Command | Description |
|---------|-------------|
| `/brief:onboard` | Load what Brief knows about your company |
| `/brief:ask <question>` | Ask Brief a product question grounded in your org's data |

## Authentication

Brief uses OAuth — no API keys or manual config. On first use, a browser window opens to sign in to your Brief workspace. The token is managed automatically.

## Learn more

- [Brief](https://usebrief.com)
- [Documentation](https://usebrief.com/docs/mcp-setup/)
- [Privacy policy](https://usebrief.com/privacy-policy/)
- [Terms of service](https://usebrief.com/terms-of-service/)
- [Support](https://usebrief.com/support/)
