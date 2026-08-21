---
name: brief
description: Product context, decisions, and strategic reasoning via the Brief MCP server. Use when starting a task, making architectural decisions, or needing product knowledge.
---

# Brief — Product Navigator

Brief holds the business context, customer insights, and strategic decisions that live outside any single document or conversation. It exposes that knowledge to this agent through the Brief MCP server.

This skill teaches you **when** to call Brief MCP tools. The tool descriptions surfaced during the MCP handshake teach you **how** to call each one.

## On session start

Call `brief_get_onboarding_context` once at the beginning of a session. It returns a structured snapshot: product strategy, personas, active decisions, recent work, and velocity.

- Default `scope: 'full'` returns the complete strategic snapshot.
- `scope: 'product'` returns just the product context object — narrower and faster.
- `scope: 'identity'` returns just workspace id and name — cheap sanity check.

This is the canonical first-contact tool. Do not use `brief_search` or `brief_ask` as a substitute — search is for finding one specific thing, ask is for strategic reasoning **after** context is loaded.

If Brief has no context yet, send the user's framing to `brief_ask` with `onboarding: true` and a bounded `context` payload: prioritize the package manifest + `git log --oneline -20`, then add a trimmed README excerpt if space allows. Brief synthesizes product context and responds with its first strategic insight.

## Before strategic decisions

Call `brief_ask` with `mode: "check"` and the proposed direction phrased as a question. Brief validates the approach against existing decisions, customer signals, and team priorities, and flags conflicts.

Example: `brief_ask({ question: "Should we use Postgres or DynamoDB for primary storage?", mode: "check" })`.

## When evaluating options or reviewing proposals

Call `brief_ask` with `mode: "advise"` for guidance grounded in your org's real data — past decisions, customer feedback, current priorities.

For sub-2-second context checks mid-build, use `mode: "fast"`.

## Capture decisions

When a consequential choice is made, record it via `brief_record_decision` with the decision and rationale. Brief renders a decision approval card so the user can approve, edit, or reject before it is persisted — this is the preferred path.

For rare batch or escape-hatch writes, fall back to `brief_execute_operation` with `operation: "record_decision"`.

Do not store product decisions in conversation memory. Conversation memory is local and ephemeral; Brief is shared and permanent.

## Factual lookups

For factual retrieval — finding specific decisions, documents, personas, features — call `brief_search` with the appropriate `types` filter, or `brief_browse` for structured listings. Use `brief_list_pending_decisions` for decisions awaiting approval.

Use `brief_ask` for judgment calls, not for data retrieval.

## Multi-turn conversations

Brief automatically continues the current MCP coding-session conversation when `conversation_id` is omitted. Pass an explicit `conversation_id` for a one-off override, or `new_conversation: true` to start a fresh default conversation. Stateful responses include `coding_session_key`; pass it as a decision `isolation_key` only when the decision should remain scoped to this coding session. Fast mode is stateless.

## Update product context

Call `brief_update_product_context` to write the product address, company, goals, or persona fields as the user shares them. Prefer silent updates during onboarding over announcing each write.

## Available MCP tools

| Tool | Purpose |
|---|---|
| `brief_get_onboarding_context` | Primer — load full workspace snapshot (decisions, personas, features, pipeline). Call once at session start. |
| `brief_ask` | Strategic reasoning, multi-turn, grounded in your org's data. Modes: `advise`, `check`, `fast`. |
| `brief_search` | Keyword search across decisions, documents, personas, features, signals. Filter with `types`. |
| `brief_browse` | Structured listings of workspace objects. |
| `brief_record_decision` | Capture a decision with rationale — renders an approval card. |
| `brief_confirm_decision` / `brief_reject_decision` / `brief_archive_decision` | Lifecycle actions on decisions. |
| `brief_list_pending_decisions` | Decisions awaiting approval. |
| `brief_update_product_context` | Write the product address, company, goals, or persona fields. |
| `brief_execute_operation` | Escape hatch for operations without a direct tool — prefer direct tools when available. |
| `brief_report_progress` / `brief_report_completion` | Send phase updates and completion status for in-flight coding tasks. |

## MCP prompts

If the MCP host supports prompts, the server also registers `brief-setup`, `brief-welcome-back`, and `brief-context`. Users can invoke these from the host's prompt picker as shortcuts for the flows above.

## CLI fallback

Use this only when the Brief MCP server is unavailable. Discover the complete shell surface with `brief manifest --json`; these common commands include the current pagination, isolation, and document-creation requirements.

```bash
brief decisions search "<query>" --offset 0 --isolation <key> --json
brief decisions get <decision-id> --isolation <key> --json
brief decisions create --decision "..." --rationale "..." --expires-when "..." --isolation <key> --json
brief signals list --order-by created_at --order desc --json
brief conversations list --cursor <cursor> --source agent --json
brief docs list --page 1 --json
brief docs create --title "..." --folder <folder-id> --content "..." --json
brief workspace list --json
```

## Rules

- Call `brief_get_onboarding_context` once at session start before reasoning about the user's product.
- Use `brief_ask` for judgment; use `brief_search` / `brief_browse` for factual lookups.
- Let `brief_ask` reuse the current coding-session conversation by default; use `conversation_id` only to override it or `new_conversation: true` to start fresh.
- Record non-trivial decisions in Brief — do not leave them in conversation memory.
- Never construct Brief web URLs by hand. Use `workspace_url` and per-object `view_url` fields from tool responses.
- These are **MCP tools**, not shell commands. Do not shell out to `brief ask` / `brief context` / `brief decisions` — that is the CLI channel, which lives behind a different install path (`brief init`).
