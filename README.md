# AgentMesh

[![DOI](https://zenodo.org/badge/1188458268.svg)](https://doi.org/10.5281/zenodo.19176144)

A blockchain-inspired distributed AI agent framework secured by MemoryChain. Connect AI agents across multiple machines using file-based communication over encrypted cloud storage.

## What is AgentMesh?

AgentMesh replaces traditional network protocols (HTTP, WebSocket, message queues) with **file synchronization** for inter-agent communication. Agents on different machines share memory, tasks, and broadcasts through an encrypted cloud storage layer (Google Drive + rclone crypt).

### Key Features

- **Zero infrastructure** — No servers, brokers, or databases to maintain
- **Spatial isolation** — Each machine writes only to its own directory, eliminating conflicts
- **Human-AI readable** — All communication uses Markdown, readable by humans and AI alike
- **Offline resilient** — Machines sync when available, no always-on requirement
- **End-to-end encrypted** — All cloud data encrypted with rclone crypt
- **Self-evolving** — Scripts auto-update via git pull

## Quick Start

### Create a mesh (first machine)

```bash
git clone https://github.com/cyhsu2006/agentmesh.git
cd agentmesh
bash bin/agentmesh init
```

This will:
1. Set up Google Drive access (opens browser for OAuth)
2. Generate encryption keys
3. Install scripts and cron job
4. Output a **join token** for other machines

### Join the mesh (other machines)

```bash
git clone https://github.com/cyhsu2006/agentmesh.git
cd agentmesh
bash bin/agentmesh join <token>
```

Each machine only needs to do this once. After that, everything syncs automatically every hour.

## Architecture

```
Machine A                  Google Drive (encrypted)           Machine B
┌──────────────┐          ┌──────────────────┐          ┌──────────────┐
│ machines/    │  push    │                  │  pull    │ machines/    │
│   host-a/   │ ──────→  │  agentmesh-data/ │ ──────→  │   host-a/   │
│   host-b/   │ ←──────  │  (rclone crypt)  │ ←──────  │   host-b/   │
│ broadcasts/ │  pull    │                  │  push    │ broadcasts/ │
│ tasks.json  │          │                  │          │ tasks.json  │
└──────────────┘          └──────────────────┘          └──────────────┘
```

### Subsystems

1. **Memory Sync** — Shared AI memory in Markdown, each machine maintains its own copy
2. **Task Scheduler** — Spec-driven task execution via `claude -p`
3. **Broadcast System** — One-to-many commands with checkbox acknowledgment
4. **Settings Sync** — AI tool settings synchronized across machines
5. **Self-Update** — Scripts auto-update from git

## Commands

| Command | Description |
|---------|-------------|
| `agentmesh init` | Create a new mesh |
| `agentmesh join <token>` | Join an existing mesh |
| `agentmesh sync [full\|pull\|push]` | Sync with cloud |
| `agentmesh broadcast "command"` | Send command to all machines |
| `agentmesh status` | Show mesh status |
| `agentmesh version` | Show version |

## Requirements

- Linux or macOS
- [rclone](https://rclone.org/) — Cloud storage sync
- [Claude Code CLI](https://claude.ai/claude-code) — AI agent
- Python 3
- Google Drive account

## License

MIT
