# goperf-skill

An [Agent Skill](https://agentskills.io) that gives your coding agent recommendations and best practices for writing performant Go code, based on the [Go Optimization Guide](https://goperf.dev).

## What it covers

- **Memory & GC** — Object pooling, preallocation, struct alignment, interface boxing, zero-copy, escape analysis, GOGC/GOMEMLIMIT
- **Concurrency** — Worker pools, atomics, `sync.Once`, immutable data, context and timeouts
- **I/O** — Buffered I/O, batching
- **Compiler** — Build flags, escape analysis
- **Networking** — `net/http` and Transport tuning, connection reuse, 10k+ connections, TLS/DNS, load shedding, backpressure, TCP/HTTP/2/gRPC/QUIC

The skill is measurement-first: it steers the agent to establish baselines and identify bottlenecks before applying patterns. All guidance is self-contained in the skill; [goperf.dev](https://goperf.dev) has extended articles and examples.

## Install

Uses the open agent skills CLI ([vercel-labs/skills](https://github.com/vercel-labs/skills)). Install the skill into your project or globally.

### From this repository

```bash
# Install to your project (recommended: committed with the repo)
npx skills add newtonmunene99/goperf-skill

# Install globally (available in all projects)
npx skills add newtonmunene99/goperf-skill -g

# Install only for Cursor
npx skills add newtonmunene99/goperf-skill -a cursor

# Install for Cursor and Codex
npx skills add newtonmunene99/goperf-skill -a cursor -a codex
```

### From a local path

```bash
npx skills add ./path/to/goperf-skill
```

### Scope

| Scope   | Flag      | Location                                  | Use case                     |
| ------- | --------- | ----------------------------------------- | ---------------------------- |
| Project | (default) | `./.agents/skills/` or agent-specific dir | Share with the whole team    |
| Global  | `-g`      | `~/.cursor/skills/` etc.                  | Use across all your projects |

Supported agents include **Cursor**, **Codex**, **Claude Code**, **OpenCode**, **Windsurf**, and [others](https://github.com/vercel-labs/skills#supported-agents). The CLI detects installed agents and can install to all of them.

## Skill structure

- **SKILL.md** — When to use the skill, workflow (baseline → bottleneck → patterns), and quick cues
- **references/common-patterns.md** — Memory, concurrency, I/O, compiler patterns
- **references/networking.md** — HTTP, Transport, scaling, resilience, TLS/DNS, protocols

## License

Apache-2.0. See [LICENSE](LICENSE).
