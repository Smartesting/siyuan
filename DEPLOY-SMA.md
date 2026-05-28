# SiYuan - Deploy / Seed / Verify / Reset

## Quick Start

```bash
./tester-env deploy    # Build image from source, start container on port 6806
./tester-env seed      # Populate with deterministic "Acme Launch Knowledge Base" notebook
./tester-env verify    # Check seeded notebook, documents, tags, and attachment
./tester-env reset     # Clean slate (stop container, remove container + data volume)
```

## Commands

| Command | Description |
|---------|-------------|
| `deploy` | Build Docker image from source (Dockerfile at repo root), start container, wait for healthy API |
| `seed`   | Create "Acme Launch Knowledge Base" notebook with 6 authored notes, 1 attachment, 18 tag mentions across 12 visible documents |
| `verify` | Assert seeded notebook exists, all 12 document paths match, >=3 `#launch-2024` tag blocks present, attachment contains "Maya Chen" |
| `reset`  | `docker stop` + `docker rm` + `docker volume rm` for container and workspace volume |
| `stop`   | Stop container (preserves data) |
| `logs`   | Tail container logs |
| `status` | Show container status and URL |
| `help`   | Print usage |

## Options

| Option | Description |
|--------|-------------|
| `--run-id <id>` | Isolate container/volume names for parallel runs (default: `default`) |
| `--port <port>` | Host port (default: `6806`) |

## Env Vars

| Variable | Description |
|----------|-------------|
| `IMAGE_TAG` | Docker image tag; set by `scripts/rl-env` for content-addressed runs (default: `tester-env-siyuan:dev`) |

## Auth

Deploy uses `SIYUAN_ACCESS_AUTH_CODE_BYPASS=true` to skip auth. Seed/verify use access auth code `siyuan123` via the API.

## Baseline

- Upstream commit: `96dfe0bea4742d6664e941f105a17f35fade4a26` (v3.6.5)
- Fork: `https://github.com/Smartesting/siyuan`
- Branch: `tester-env-baseline`
- Dockerfile: repo root
- Port: 6806
- Workspace volume: `siyuan-workspace` (mounted at `/siyuan/workspace`)

## Seed Data

Notebook "Acme Launch Knowledge Base" with:

- 6 authored notes across directories: Inbox, Projects, Operations, Meetings, Reference
- 1 attachment (`/data/assets/acme-q2-launch-brief.txt`)
- 18 tag mentions (`#launch-2024`, `#research`, `#retail`, `#project`, `#mobile`, `#decision`, `#support`, `#operations`, `#playbook`, `#meeting`, `#weekly-review`, `#architecture`, `#offline`)
- Rich content: tables, backlinks, checklists, inline references
- Deterministic: reset + deploy + seed always produces identical visible state
