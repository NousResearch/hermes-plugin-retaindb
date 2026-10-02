> **Not maintained by Nous Research — looking for a maintainer.** This is a standalone copy of the `retaindb` memory provider that still ships inside Hermes Agent under `plugins/memory/retaindb/`. Nous Research does not maintain memory providers, so this repo gets no fixes or releases from Nous and is not listed in the Hermes plugin catalog. Hermes users need do nothing: the bundled provider keeps working. RetainDB or anyone else who wants to own it: see [HANDOFF.md](HANDOFF.md).

# RetainDB Memory Provider

Cloud memory API with hybrid search (Vector + BM25 + Reranking) and 7 memory types.

## Requirements

- RetainDB account ($20/month) from [retaindb.com](https://www.retaindb.com)
- `requests` (declared in `pyproject.toml`; `hermes plugins install` puts it in the Hermes venv, and Hermes core already ships it)

## Setup

```bash
hermes memory setup    # select "retaindb"
```

Or manually:
```bash
hermes config set memory.provider retaindb
echo "RETAINDB_API_KEY=your-key" >> ~/.hermes/.env
```

## Config

All config via environment variables in `.env`:

| Env Var | Default | Description |
|---------|---------|-------------|
| `RETAINDB_API_KEY` | (required) | API key |
| `RETAINDB_BASE_URL` | `https://api.retaindb.com` | API endpoint |
| `RETAINDB_PROJECT` | auto (profile-scoped) | Project identifier |

## Tools

| Tool | Description |
|------|-------------|
| `retaindb_profile` | User's stable profile |
| `retaindb_search` | Semantic search |
| `retaindb_context` | Task-relevant context |
| `retaindb_remember` | Store a fact with type + importance |
| `retaindb_forget` | Delete a memory by ID |
