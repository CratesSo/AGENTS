# AGENTS

Published Codex global instructions and custom subagent definitions.

This repo is the private source of truth for the global `AGENTS.md` file and the custom subagents used by Codex. The parent `Markdown` directory is only a workspace; this `agents/` checkout is the publishing repo.

## Files

| File | Purpose |
| --- | --- |
| `AGENTS.md` | Global Codex operating instructions. |
| `cleanup.toml` | Execution-focused cleanup agent for adjacent dead code, redundant paths, obsolete state, and cleanup opportunities. |
| `explorer_light.toml` | Cheap read-only search and reconnaissance agent for simple file discovery, exact text search, and evidence gathering. |
| `explorer_standard.toml` | Read-only explorer for mapping code paths, gathering evidence, local file search, and external verification. |
| `explorer_deep.toml` | Read-only explorer for complex mapping, evidence gathering, local file search, and external verification. |
| `reviewer.toml` | Read-only reviewer for high-reasoning code review, audits, regressions, and correctness/security risks. |
| `reviewer_mini.toml` | Read-only reviewer for cheaper review and regression checks. |
| `worker.toml` | Execution-focused worker for complex targeted implementation or fixes. |
| `worker_mini.toml` | Execution-focused worker for small targeted implementation or fixes. |

## Subagent Routing

| Agent | Model | Reasoning | Sandbox | Use when |
| --- | --- | --- | --- | --- |
| `cleanup` | `gpt-5.5` | `medium` | `workspace-write` | A change creates obvious dead code, redundant paths, stale state, or cleanup opportunities adjacent to the work. |
| `explorer_light` | `gpt-5.3-codex-spark` | `medium` | `read-only` | The task needs very fast, lightweight file discovery, exact text search, or evidence gathering. |
| `explorer_standard` | `gpt-5.4-mini` | `medium` | `read-only` | The task needs bounded read-only exploration, file tracing, or external verification. |
| `explorer_deep` | `gpt-5.5` | `low` | `read-only` | The task needs complex read-only mapping, code path tracing, or evidence gathering without edits. |
| `reviewer` | `gpt-5.5` | `high` | `read-only` | The change has serious correctness, security, regression, or invariant risk. |
| `reviewer_mini` | `gpt-5.5` | `medium` | `read-only` | A focused review is useful, but a full high-reasoning review is overkill. |
| `worker` | `gpt-5.5` | `medium` | `workspace-write` | The implementation is complex, risky, or benefits from a stronger execution agent. |
| `worker_mini` | `gpt-5.4-mini` | `high` | `workspace-write` | The implementation is small and scoped, but still needs careful reasoning. |

## Versions

| Item | File | Current version | Tag |
| --- | --- | --- | --- |
| global-agents | `AGENTS.md` | v3.1.7 | `global-agents/v3.1.7` |
| cleanup | `cleanup.toml` | v3.2.2 | `cleanup/v3.2.2` |
| explorer_light | `explorer_light.toml` | v1.0.0 | `explorer_light/v1.0.0` |
| explorer_standard | `explorer_standard.toml` | v1.0.0 | `explorer_standard/v1.0.0` |
| explorer_deep | `explorer_deep.toml` | v1.0.0 | `explorer_deep/v1.0.0` |
| reviewer | `reviewer.toml` | v3.5.4 | `reviewer/v3.5.4` |
| reviewer_mini | `reviewer_mini.toml` | v3.5.4 | `reviewer_mini/v3.5.4` |
| worker | `worker.toml` | v3.1.3 | `worker/v3.1.3` |
| worker_mini | `worker_mini.toml` | v3.1.3 | `worker_mini/v3.1.3` |

## Install / Pin

Use the matching tag when pinning a published file from this repo.

Examples:

- `CratesSo/AGENTS@global-agents/v3.1.5`
- `CratesSo/AGENTS@worker/v2.1.3`
