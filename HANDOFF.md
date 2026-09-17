# Handoff notes — hermes-plugin-retaindb

This repository is a **handoff copy** of the `retaindb` memory provider that shipped inside
`NousResearch/hermes-agent` under `plugins/memory/retaindb/`. Nous Research is moving every memory
provider out of the core tree; this repo exists so the upstream project (https://retaindb.com)
can clone it, take ownership, and submit it to the [Hermes plugin catalog](https://hermes-agent.nousresearch.com/plugins)
under their own org. It is **not** an officially maintained Nous plugin.

What it is: RetainDB memory.

## Install (as a user)

```
hermes plugins install NousResearch/hermes-plugin-retaindb
hermes plugins enable retaindb
hermes memory setup            # or: set memory.provider: retaindb in config.yaml
```

Dependencies in `pyproject.toml` are installed into the Hermes venv automatically and survive
`hermes update`.

## What changed versus the in-tree copy

Mechanical only; behaviour is identical.

- Absolute self-imports (`from plugins.memory.retaindb.x import ...`) became relative imports so the
  package loads from `~/.hermes/plugins/retaindb/` under the loader's synthetic namespace.
- No shared helpers needed.
- Added `pyproject.toml` declaring: `requests>=2.31,<3`.

## For the maintainer taking this over

- `config_schema.py` still imports from `plugins.memory.config_schema`: Hermes core loads that file
  directly and the dashboard expects core's `ProviderConfigSchema` type, so it must not be vendored.
- The in-tree `tools.lazy_deps.ensure("memory.retaindb")` calls were removed: they pinned the exact
  (old) version in Hermes' lazy-deps registry and downgraded newer installs (hermes-agent#86992).
  `pyproject.toml` is now the only dependency authority; bump it when you need a newer client.
- The setup wizard imports private helpers from `hermes_cli.memory_setup` (`_curses_select`,
  `_prompt`, ...). Those are Hermes internals, not API; expect to own a copy or drop the wizard
  hook if they move.
- Tests were not copied: the in-tree tests import `plugins.memory.retaindb` and depend on the
  hermes-agent test harness. See `tests/plugins/memory/` in hermes-agent for the originals.
- While the in-tree copy still exists, a same-named user plugin is shadowed by it
  (bundled providers win on name). It takes effect the moment core drops `plugins/memory/retaindb`.

Original authors are preserved in hermes-agent's history: `git log -- plugins/memory/retaindb`.
