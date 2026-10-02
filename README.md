> **Maintained by Nous Research.** This is the `retaindb` memory provider that used to ship inside Hermes Agent under `plugins/memory/retaindb/`; it now installs from the [Hermes plugin catalog](https://hermes-agent.nousresearch.com/docs/plugins/retaindb). Existing `memory.provider: retaindb` setups keep their config and data. See [HANDOFF.md](HANDOFF.md).

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
