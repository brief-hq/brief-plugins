# Changelog

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
