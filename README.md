# brief-plugins (scaffold)

Public-repo tree for the Brief marketplace on Claude Code. This directory is mirrored to [`brief-hq/brief-plugins`](https://github.com/brief-hq/brief-plugins), the repo users install via:

```text
/plugin marketplace add brief-hq/brief-plugins
/plugin install brief@brief-plugins
```

## Source of truth

The plugin content (`plugin.json`, `.mcp.json`, commands, skill, icon, README) is a mirror of `packages/claude-plugin/` in this monorepo. Edit there, not here.

Parity between `packages/claude-plugin/` and `packages/brief-plugins-scaffold/plugins/brief/` is enforced in **BRI-2840**.
