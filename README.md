# ZLAR-FL

**Multi-agent fleet governance. One view across all your governed agents.**

ZLAR-FL manages multiple ZLAR-governed agents as a fleet. Register agents, monitor health, aggregate audit trails, compare policies, and generate fleet-wide governance reports.

Each agent has its own gate. FL gives you the fleet-wide picture.

## The Problem

You have three machines running ZLAR-governed agents. Each has its own gate, its own policy, its own audit trail. You need:

- Are all agents healthy?
- Are they all running the same policy version?
- What did the fleet do today — total events, denials, high-risk actions?
- Which agent is generating the most denied actions?

ZLAR-FL answers these questions from a single CLI.

## Quick Start

```bash
git clone https://github.com/ZLAR-AI/ZLAR-FL.git
cd ZLAR-FL

# Initialize a fleet
bin/zlar-fl init "my-fleet"

# Register an agent
bin/zlar-fl register \
    --id agent-01 \
    --name "Dev Machine" \
    --gate ~/.zlar-lt \
    --framework claude-code

# Check fleet status
bin/zlar-fl status

# Fleet-wide audit
bin/zlar-fl audit

# Governance report
bin/zlar-fl report
```

## Commands

| Command | Description |
|---------|-------------|
| `init [name]` | Initialize a new fleet registry |
| `register` | Register an agent in the fleet |
| `deregister <id>` | Remove an agent from the fleet |
| `list` | List all fleet agents |
| `status` | Fleet health — gate presence, audit file, event counts |
| `audit` | Aggregate audit trails across all agents |
| `report` | Fleet-wide governance report |
| `policy [show\|compare]` | View and compare policies across agents |
| `topology` | Show fleet topology grouped by host |
| `update` | Update an agent's properties |

## Registering Agents

```bash
# Local agent with ZLAR-LT
bin/zlar-fl register \
    --id bohm-mini \
    --name "Bohm (Mac Mini)" \
    --gate ~/.zlar-lt \
    --host local \
    --framework claude-code \
    --tags primary,inference

# Another local agent with full ZLAR-Gate
bin/zlar-fl register \
    --id dev-laptop \
    --name "Dev Laptop" \
    --gate ~/.zlar-gate \
    --host local \
    --framework cursor

# Remote agent (status shows as remote, audit not aggregated yet)
bin/zlar-fl register \
    --id staging-01 \
    --name "Staging Server" \
    --gate /opt/zlar-gate \
    --host staging.internal \
    --tags staging,ci
```

### What gets registered

| Field | Required | Description |
|-------|----------|-------------|
| `--id` | Yes | Unique agent identifier |
| `--name` | No | Human-readable name (defaults to id) |
| `--gate` | No | Path to agent's ZLAR gate installation |
| `--host` | No | Hostname (default: "local") |
| `--framework` | No | claude-code, cursor, windsurf (auto-detected from gate path) |
| `--tags` | No | Comma-separated tags for filtering |
| `--audit` | No | Explicit path to audit file (overrides gate-based detection) |

## Fleet Status

```bash
bin/zlar-fl status
```

Shows for each agent:
- Gate installation found (✓/✗)
- Audit file accessible (✓/✗)
- Event count
- Health indicator (● healthy, ○ no audit, ✗ gate missing, ? remote)

## Fleet Audit

Aggregate audit trails across all local agents:

```bash
# Show recent events per agent
bin/zlar-fl audit

# Filter by agent
bin/zlar-fl audit --agent bohm-mini

# Filter by outcome
bin/zlar-fl audit --outcome deny

# Merge all agents into single stream (for SIEM/analysis)
bin/zlar-fl audit --merge --json
```

## Policy Comparison

Compare policies across the fleet:

```bash
# Show each agent's policy summary
bin/zlar-fl policy show

# Compare policy hashes to find drift
bin/zlar-fl policy compare
```

Agents with the same policy hash have identical rule sets. Different hashes mean policy drift — investigate.

## Topology

Visual fleet topology grouped by host:

```bash
bin/zlar-fl topology
```

```
╔══ local
║  ● bohm-mini — Bohm (Mac Mini)
║    framework: claude-code
║    tags: primary,inference
║  ● dev-laptop — Dev Laptop
║    framework: cursor
╚══

╔══ staging.internal
║  ● staging-01 — Staging Server
║    tags: staging,ci
╚══
```

## Fleet Report

Fleet-wide governance report:

```bash
bin/zlar-fl report
```

Includes: per-agent summary (events, allows, denials), fleet totals, framework distribution, host distribution.

## The Fleet Registry

The registry is a JSON file (`etc/fleet-registry.json`). It is a **human artifact** — like the policy is a human artifact in the gate.

- Humans decide which agents are in the fleet
- No agent can register itself
- No agent can deregister another agent
- The registry is edited through `zlar-fl` commands or by hand

The registry is gitignored by default (it contains local file paths). To version-control your fleet configuration, add it explicitly.

## Known Limitations

- **Local agents only for audit aggregation.** Remote agent audit trails require SSH access (planned for v2).
- **No real-time monitoring.** FL reads audit files at query time. For real-time fleet monitoring, use a SIEM with ZLAR-AU Splunk CIM export.
- **Policy sync is manual.** FL compares policies but doesn't distribute them. Use your deployment pipeline for policy distribution.
- **No agent communication.** FL reads agent data. Agents don't know they're in a fleet and don't communicate with each other through FL.
- **Registry is not encrypted.** Don't store secrets in agent metadata.

## Dependencies

- `jq` — required
- `bash` 4+ — required
- `ssh` — optional (future remote agent support)

## The ZLAR Family

| Product | What It Does |
|---------|-------------|
| [ZLAR-OC](https://github.com/ZLAR-AI/ZLAR-OC) | OS-level containment for OpenClaw agents |
| [ZLAR-CC](https://github.com/ZLAR-AI/ClaudeCode_ZLAR-CC) | Hook-based gate for Claude Code |
| [ZLAR Gate](https://github.com/ZLAR-AI/ZLAR-Gate) | Universal gate — Claude Code + Cursor + Windsurf |
| [ZLAR-LT](https://github.com/ZLAR-AI/ZLAR-LT) | Zero-config governance — one command install |
| [ZLAR-AU](https://github.com/ZLAR-AI/ZLAR-AU) | Audit trail analysis & compliance reporting |
| [ZLAR-NT](https://github.com/ZLAR-AI/ZLAR-NT) | Network egress policy for AI agents |
| **ZLAR-FL** | **Multi-agent fleet governance** |

All open source. Apache 2.0. Built by [ZLAR Inc.](https://zlar.ai)

## License

Apache 2.0 — see [LICENSE](LICENSE)
