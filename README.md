# brief-plugins (scaffold)

Public-repo tree for the Brief marketplace on Claude Code. This directory is mirrored to [`brief-hq/brief-plugins`](https://github.com/brief-hq/brief-plugins), the repo users install via:

```text
/plugin marketplace add brief-hq/brief-plugins
/plugin install brief@brief-plugins
```

## Source of truth

The `brief-hq/brief` engineering repository owns `packages/claude-plugin` and
`packages/brief-plugins-scaffold`; this README is included in the scaffold and its
public mirror. The published `brief-hq/brief-plugins` repository contains the plugin
files, not those monorepo directories. Run parity checks in the engineering
repository, and pair public mirror updates with the corresponding source changes.
