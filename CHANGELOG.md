# Changelog

## 1.0.6 — 2026-11-06

- Set the Claude listing icon.

## 1.0.5 — 2026-11-05

- Set the Claude listing publisher to Brief Labs Inc. and add documentation, support, privacy, and terms links.
- Describe Brief as a product strategy agent grounded in workspace context.
- Move maintainer evaluation instructions to the monorepo docs and keep published READMEs focused on users.
- Remove Claude shell fallback and CLI consultation instructions; ask users to reconnect when MCP is unavailable.

## 1.0.4 — 2026-11-04

- BRI-2576: Route named workspace reads and writes through separate MCP tools.

## Unreleased

- Document default MCP Ask continuity and opt-in decision isolation through the
  returned `coding_session_key`.
- Expose Claude commands as `/brief:ask` and `/brief:onboard`, and add a
  behavioral eval gate with a no-plugin comparison.

## 1.0.3 — 2026-10-26

- Finish `/brief:ask` after summarizing Brief's recommendation and rationale,
  without saving product guidance to local conversation memory, in both Claude
  command copies.
- Retain failed eval transcripts for diagnosis and exclude them from public sync.

## 1.0.1 — 2026-07-29

- Use a plugin-specific MCP server ID so the plugin works alongside a manually added Brief connector.

## 1.0.0 — 2026-04-19

- Initial release: `brief` plugin with skill, `/brief-ask` and `/brief-onboard` commands, and MCP server at `https://app.briefhq.ai/mcp`.
