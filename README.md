# AGENTS

Published Codex global instructions and custom subagent definitions.

This repo is the private source of truth for the global `AGENTS.md` file and the custom subagents used by Codex. The parent `Markdown` directory is only a workspace; this `agents/` checkout is the publishing repo.

## Files

| File | Purpose |
| --- | --- |
| `AGENTS.md` | Global Codex operating instructions. |
| `cleanup.toml` | Execution-focused cleanup agent for adjacent dead code, redundant paths, obsolete state, and cleanup opportunities. |
| `explorer_light.toml` | Cheap read-only locator and evidence gatherer for exact text search, filename discovery, small scoped lookups, and path:line refs. |
| `explorer_standard.toml` | Default read-only repo explorer for mapping relevant files, symbols, local code paths, and evidence across a small scoped area. |
| `explorer_deep.toml` | Escalated read-only mapper for multi-module paths or deeper local evidence maps. |
| `reviewer_lite.toml` | Fast read-only reviewer for cheap code review and regression checks. |
| `reviewer_mid.toml` | Standard read-only reviewer for normal code review. |
| `reviewer_heavy.toml` | Deep read-only reviewer for high-reasoning code review, adversarial audits, regressions, and security/correctness risks. |
| `worker.toml` | Execution-focused worker for complex targeted implementation or fixes. |
| `worker_mini.toml` | Execution-focused worker for small targeted implementation or fixes. |

## Subagent Routing

| Agent | Model | Reasoning | Sandbox | Use when |
| --- | --- | --- | --- | --- |
| `cleanup` | `gpt-5.5` | `medium` | `workspace-write` | A change creates obvious dead code, redundant paths, stale state, or cleanup opportunities adjacent to the work. |
| `explorer_light` | `gpt-5.3-codex-spark` | `medium` | `read-only` | The task needs exact text search, filename discovery, small scoped lookups, or a few path:line refs. |
| `explorer_standard` | `gpt-5.4-mini` | `low` | `read-only` | The task needs default read-only repo exploration across a small scoped area. |
| `explorer_deep` | `gpt-5.4-mini` | `high` | `read-only` | Standard exploration is insufficient, the code path spans multiple modules, or deeper local evidence is needed. |
| `reviewer_lite` | `gpt-5.3-codex` | `high` | `read-only` | A fast, cheap code review or regression check is enough. |
| `reviewer_mid` | `gpt-5.4` | `high` | `read-only` | A normal code review needs a standard read-only reviewer. |
| `reviewer_heavy` | `gpt-5.5` | `high` | `read-only` | The change has serious correctness, security, regression, adversarial audit, or invariant risk. |
| `worker` | `gpt-5.5` | `medium` | `workspace-write` | The implementation is complex, risky, or benefits from a stronger execution agent. |
| `worker_mini` | `gpt-5.4-mini` | `high` | `workspace-write` | The implementation is small and scoped, but still needs careful reasoning. |

## Versions

| Item | File | Current version | Tag |
| --- | --- | --- | --- |
| global-agents | `AGENTS.md` | v3.1.9 | `global-agents/v3.1.9` |
| cleanup | `cleanup.toml` | v3.2.2 | `cleanup/v3.2.2` |
| explorer_light | `explorer_light.toml` | v1.0.2 | `explorer_light/v1.0.2` |
| explorer_standard | `explorer_standard.toml` | v1.0.2 | `explorer_standard/v1.0.2` |
| explorer_deep | `explorer_deep.toml` | v1.0.2 | `explorer_deep/v1.0.2` |
| reviewer_lite | `reviewer_lite.toml` | v1.0.0 | `reviewer_lite/v1.0.0` |
| reviewer_mid | `reviewer_mid.toml` | v1.0.0 | `reviewer_mid/v1.0.0` |
| reviewer_heavy | `reviewer_heavy.toml` | v1.0.0 | `reviewer_heavy/v1.0.0` |
| worker | `worker.toml` | v3.1.3 | `worker/v3.1.3` |
| worker_mini | `worker_mini.toml` | v3.1.3 | `worker_mini/v3.1.3` |

## Install / Pin

Use the matching tag when pinning a published file from this repo.

Examples:

- `CratesSo/AGENTS@global-agents/v3.1.5`
- `CratesSo/AGENTS@worker/v2.1.3`
