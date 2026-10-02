# Maintenance notes — hermes-plugin-retaindb

This repository is the `retaindb` memory provider that shipped inside `NousResearch/hermes-agent` under
`plugins/memory/retaindb/`. Nous Research moved every memory provider out of the core tree; this repo is
now its home, **maintained by Nous Research** and listed in the
[Hermes plugin catalog](https://hermes-agent.nousresearch.com/docs/plugins/retaindb) as `retaindb` (tier
`official`), pinned to a reviewed release tag. If RetainDB (https://retaindb.com) wants to own the plugin, open an issue here: we transfer the repo or re-point the catalog entry at their fork, keeping the `retaindb` name so existing users migrate.

## Install (as a user)

```
hermes plugins install retaindb     # from the catalog, at the reviewed pin
hermes memory setup            # or: set memory.provider: retaindb in config.yaml
```

Users who already had `memory.provider: retaindb` need do nothing: once core drops its bundled copy,
`hermes update` (and agent start) installs this plugin from the catalog automatically. Config
(`memory.retaindb` / plugin sections) and data files are unchanged. Dependencies in `pyproject.toml` are
installed into the Hermes venv automatically and survive `hermes update`.

## Keeping it in sync with core

Code here tracks the last in-tree copy (`git log -- plugins/memory/retaindb` in hermes-agent) verbatim.
Every release is a tag `vX.Y.Z` matching `version` in `plugin.yaml` and `pyproject.toml`; the catalog
pin moves only through a hermes-agent PR that bumps `sha:` and `version:` together.

Differences versus the in-tree copy (mechanical only; behaviour is identical):

- Self-imports are relative so the package loads from `~/.hermes/plugins/retaindb/` under the loader's
  synthetic namespace.
- `pyproject.toml` is the dependency authority (no `tools.lazy_deps` calls).

## For maintainers

- The in-tree `tools.lazy_deps.ensure("memory.retaindb")` calls were removed: they pinned the exact
  (old) version in Hermes' lazy-deps registry and downgraded newer installs (hermes-agent#86992).
  `pyproject.toml` is now the only dependency authority; bump it when you need a newer client.
- `tests/` holds the in-tree tests (`tests/plugins/memory/test_retaindb*.py` in hermes-agent) with the
  import path switched to the package `tests/conftest.py` loads from this repo. Run them locally with
  `HERMES_AGENT_REPO=~/.hermes/hermes-agent PYTHONPATH=~/.hermes/hermes-agent python -m pytest -q`;
  CI runs the same on Linux, macOS and Windows against hermes-agent `main`.
- While a Hermes install still carries the bundled `plugins/memory/retaindb`, that copy wins on name and
  this plugin is dormant; it takes over once core drops the bundled directory.

Original authors are preserved in hermes-agent's history: `git log -- plugins/memory/retaindb`.
