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

## Decision standard

A decision is a rule or choice that will still guide someone after the ticket ships. Ask: "Will this still guide someone after this ticket ships?" State it in plain language in 1–3 sentences, understandable without the ticket, PR or code, and explain why in product or business terms. A durable choice not to build something also counts when its product or business reason survives the ticket. Keep ticket-scoped notes in the ticket, PR description or a code comment. Keep file paths, function names, flags and run IDs out of decision text; put ticket, PR and decision IDs only in references. When a tool has no references field, keep reference links in the originating ticket or PR and omit IDs from the decision and rationale.

Session isolation does not make ticket notes eligible decisions.

## Capture decisions

Before recommending, recording or proposing a decision, apply the decision standard above.

When a settled choice meets the decision standard above and the user is making it right now, record it via `brief_record_decision` with the decision and rationale. Brief renders a decision approval card so the user can approve, edit, or reject before it is persisted — this is the preferred path for a decision the user is confirming themselves. (It also supports `status: 'pending'` for proposing to someone else, but only on hosts that render that card — on other hosts it writes nothing.)

When a decision needs **someone else's** sign-off, or isn't settled yet, use `brief_propose_decision` instead — it writes a pending proposal immediately, no card required, on any host. **Never record an unsettled decision as confirmed; propose the intended lasting rule as a declarative statement for review.** If multiple people must sign off, propose once per approver.

Example: asked to "get sign-off from Drew on offering a free plan," call `brief_propose_decision({ decision: "Offer a free plan", rationale: "Small teams need to try the product before paying.", assigned_to_email: "drew@example.com" })` — not `brief_record_decision`.

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
| `brief_record_decision` | Record a decision now with rationale — renders an approval card; only proposes via `status: 'pending'` on hosts that render it. |
| `brief_propose_decision` | Create a pending decision proposal directly, no card required. Use when sign-off is needed from someone else or the decision isn't settled. |
| `brief_confirm_decision` / `brief_reject_decision` / `brief_archive_decision` | Lifecycle actions on decisions. |
| `brief_list_pending_decisions` | Decisions awaiting approval. |
| `brief_update_product_context` | Write the product address, company, goals, or persona fields. |
| `brief_execute_operation` | Escape hatch for operations without a direct tool — prefer direct tools when available. |
| `brief_report_progress` / `brief_report_completion` | Send phase updates and completion status for in-flight coding tasks. |

## MCP prompts

If the MCP host supports prompts, the server also registers `brief-setup`, `brief-welcome-back`, and `brief-context`. Users can invoke these from the host's prompt picker as shortcuts for the flows above.

## CLI fallback

`brief decisions create` records as confirmed immediately and has no approval card. Use it only after the current user explicitly authorizes recording that settled choice; otherwise propose it for review.

Use this only when the Brief MCP server is unavailable. Discover the complete shell surface with `brief manifest --json`; these common commands include the current pagination, isolation, and document-creation requirements.

```bash
brief decisions search "<query>" --offset 0 --isolation <key> --json
brief decisions get <decision-id> --isolation <key> --json
brief decisions create --decision "..." --rationale "..." --expires-when "..." --isolation <key> --json
brief decisions propose --decision "..." --rationale "..." --assignee <email> --json
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
- Record only lasting rules that meet the decision standard above; keep ticket notes in the ticket, PR description or a code comment.
- Never construct Brief web URLs by hand. Use `workspace_url` and per-object `view_url` fields from tool responses.
- These are **MCP tools**, not shell commands. Do not shell out to `brief ask` / `brief context` / `brief decisions` — that is the CLI channel, which lives behind a different install path (`brief init`).
