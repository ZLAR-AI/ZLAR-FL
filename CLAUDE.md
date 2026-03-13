# ZLAR-FL — Project Guide

## What this is

ZLAR-FL manages multiple ZLAR-governed agents as a fleet. Centralized registry, distributed enforcement, fleet-wide audit. The governance layer for organizations running many agents across multiple machines and frameworks.

## Architecture

```
Fleet Registry (JSON — human artifact)
    ↓ reads
bin/zlar-fl          — CLI tool (~700 lines bash + jq)
    ↓ queries
Agent audit trails   — each agent's var/log/audit.jsonl
    ↓ produces
Fleet reports, status, topology, policy comparison
```

### Not a controller

ZLAR-FL does not control agents. It does not start, stop, or modify agents. It reads their audit trails and reports on the fleet. Each agent's gate enforces policy independently. FL provides fleet-wide visibility.

### The registry is a human artifact

Like the policy is a human artifact in the gate, the fleet registry is a human artifact. Humans decide which agents are in the fleet. No agent can add itself to the fleet. No agent can remove another agent.

## Core principles

1. **No intelligence.** Read registries, aggregate audit trails, format reports. No ML, no heuristics.
2. **Distributed enforcement.** Each agent has its own gate and policy. FL doesn't override individual agents.
3. **Centralized visibility.** One view across all agents — status, health, policy, audit.
4. **Human-controlled.** The registry is edited by humans, not agents.

## Key files

- `bin/zlar-fl` — the CLI tool
- `etc/fl.json` — configuration (registry path, log directory)
- `etc/fleet-registry.json` — fleet agent registry (gitignored — contains local paths)
- `var/log/` — FL operation logs (gitignored)

## Commands

| Command | What it does |
|---------|-------------|
| `init` | Initialize a new fleet registry |
| `register` | Register an agent in the fleet |
| `deregister` | Remove an agent from the fleet |
| `list` | List all fleet agents |
| `status` | Fleet health status with audit file checks |
| `audit` | Aggregate audit trails across agents |
| `report` | Fleet-wide governance report |
| `policy` | View/compare policies across agents |
| `topology` | Show fleet topology grouped by host |
| `update` | Update an agent's properties |

## Agent registration

```bash
zlar-fl register \
    --id bohm-mini \
    --name "Bohm (Mini)" \
    --gate ~/.zlar-lt \
    --host local \
    --framework claude-code \
    --tags inference,primary
```

Required: `--id`. Optional: everything else. Framework auto-detected from gate path if present.

## Dependencies

- `jq` — required (all JSON processing)
- `bash` 4+ — required
- `ssh` — optional (future: remote agent status checks)
