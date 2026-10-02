# Maintenance notes — hermes-plugin-retaindb

**Status: unmaintained, looking for an owner.** Nous Research does not maintain memory providers. This
repository is a standalone copy of the `retaindb` memory provider that ships inside `NousResearch/hermes-agent`
under `plugins/memory/retaindb/`, prepared so someone else can take it over. It is **not** listed in the
Hermes plugin catalog and Nous publishes no further fixes or releases here. The last sync with core is
tag `v1.0.1`.

## Taking it over

RetainDB (https://retaindb.com) — or anyone else — who wants to maintain this provider: open an issue in this repo. We can
transfer the repository to you, or you can fork it. Once you maintain it, submit a catalog entry to
`NousResearch/hermes-agent` (`plugin-catalog/retaindb.yaml`, see the
[plugin catalog guide](https://hermes-agent.nousresearch.com/docs/user-guide/features/plugin-catalog)) under the
name `retaindb`. That exact name matters: when core later drops its bundled copy, `hermes update` installs the
catalog plugin of the same name for users who have `memory.provider: retaindb`, keeping their config and data.

## Install (as a user)

Nothing to do: Hermes Agent still bundles this provider (`hermes memory setup`, or
`memory.provider: retaindb` in config.yaml). To try this standalone copy instead, `hermes plugins install
NousResearch/hermes-plugin-retaindb --ref 8d7ea4d79ecaf5632e4f73c05133349b64258482` (tag `v1.0.1`); the bundled copy wins on name while it exists.

## Keeping it in sync with core

Code here tracks the last in-tree copy (`git log -- plugins/memory/retaindb` in hermes-agent) verbatim.
Every release is a tag `vX.Y.Z` matching `version` in `plugin.yaml` and `pyproject.toml`. A future catalog entry would pin a tag by `sha:` and `version:`.

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
