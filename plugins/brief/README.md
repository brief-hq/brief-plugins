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
| `/brief:onboard` | Load what Brief knows about your company |
| `/brief:ask <question>` | Ask Brief a product question grounded in your org's data |

## Authentication

Brief uses OAuth — no API keys or manual config. On first use, a browser window opens to sign in to your Brief workspace. The token is managed automatically.

## Behavioral evaluation

From the Brief monorepo, run:

```bash
pnpm plugin:eval
```

The command evaluates this shipped Claude plugin three times per behavior, then
repeats the same prompts without the plugin. Its scorecard reports each
behavior's plugin success rate, no-plugin rate, lift, and perfect-run count.
Every case checks both the expected Brief tool call and whether Claude uses the
returned information. Missing authentication, incomplete runs, changed cases,
or scores below the recorded thresholds fail the command.

Pull requests run deterministic checks over the suite structure and score gate
without receiving the model credential. The model-backed comparison runs on the
trusted `main` ref and must pass before the public-repo sync can publish the
Claude scaffold.

To add a case, create `evals/<case>/prompt.md` and one or more Markdown graders
under `evals/<case>/graders/`. Keep MCP responses deterministic under
`evals/mocks/brief-plugin/`, update `_tools.json` when a tool schema changes,
and add the behavior to `evals/baseline.json`.

Re-baseline only after reviewing a complete three-run result for an intentional
plugin, model, or Claude Code change. Update the date, pinned versions, scores,
and per-behavior results together. Do not lower a threshold to make a regression
pass. The plugin release owner must review that result, then dispatch the sync
workflow from `main` with `allow_rebaseline` enabled. Normal releases compare
against the currently published baseline and fail if it is missing or changed.
This suite has no LLM graders; any future LLM grader must record its judge model
and demonstrate stable results over at least ten repeat runs before it can gate
CI.

## Learn more

- [briefhq.ai](https://briefhq.ai) — Brief product site
- [briefhq.ai/mcp](https://briefhq.ai/mcp/) — MCP integration docs
