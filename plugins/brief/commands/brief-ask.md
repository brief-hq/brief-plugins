# Brief Ask

Ask Brief a product question. Brief is your Product Navigator — it knows business context, customer insights, and strategic decisions that live outside any single document.

## Usage

`/brief-ask <question>`

Calls continue the current MCP coding-session conversation by default. Start fresh with `new_conversation: true`, or pass an explicit `conversation_id` as a one-off override that leaves the default in place; fast mode is stateless.

## Examples

- `/brief-ask What should we prioritize next quarter?`
- `/brief-ask --mode check Should we pursue enterprise or SMB first?`
- `/brief-ask --mode fast Is this decision already documented?`

## Modes

- **advise** (default) — Strategic guidance grounded in your org's data
- **check** — Validate a direction against existing decisions and customer signals
- **fast** — Quick, low-latency context checks during active implementation
