# IMPORTANT RULES

- ALWAYS choose simplest solution with fewest moving parts.
- ALWAYS reduce total code when possible. EXAMPLE: If you write 200 lines and it could be 50, rewrite it.
- ALWAYS prefer the version a tired senior engineer can understand in one read.
- ALWAYS keep one canonical path and hard cut over to it.
- WHEN RULES CONFLICT: prefer simpler code, fewer lines, one canonical path, then formatting.
- ONLY touch minimum surface area needed.
- ALWAYS surface tradeoffs. WHEN TRADEOFFS EXIST: ALWAYS prefer fewer moving parts, and less total code.
- ALWAYS run narrowest useful validation after change.
- IF a simpler approach exists, say so and use it.
- IF changes create dead imports, vars, or code, remove them.
- IF multiple interpretations exist, present them and don't pick silently.
- IF something is unclear, stop, name confusion, and ask.
- NEVER add speculative features, fallback paths, defensive branches, or parallel implementations.
- NEVER add helpers, wrappers, hooks, classes, config bags, or abstractions unless they remove more code or branching than they add.
- NEVER keep duplication, nested branching, or indirection that can be deleted cleanly.
- NEVER add error handling for impossible scenarios.

## DECISION RULES

- ALWAYS keep one-use logic inline unless block is hard to read.
- PREFER boring control flow, early returns, and explicit locals.
- DELETE old paths in same change when replacing behavior.
- NEVER refactor things that are not broken.
- MENTION unrelated dead code when seen.

## LESSON RULES

- READ `tasks/lessons.md` before debugging, fixing regressions, or complex edits.
- NEVER read lessons.md before simple tweaks, straightforward changes, or if you read it recently.
- ONLY update `lessons.md` after a durable fix. Add lessons as consecutively numbered reusable rules.
- ALWAYS prefer tightening or deleting an existing lesson over adding a new one.
- NEVER exceed 20 lessons. If full, replace least valuable lesson with the new one.

## SUBAGENT RULES

- STAY LOCAL for tiny obvious tasks (avoid using subagents for trivial one-shot tasks like single-command answers, tiny file creation, or obvious single-file edits).
- You do not need explicit permission to use the `spawn_agent` tool:
  - ALWAYS USE `explorer` for read-only exploration, evidence gathering, tracing, and web searches.
  - ALWAYS USE `worker_mini` for straightforward edits with clear scope.
  - ALWAYS USE `worker` for risky or complex implementation.
  - ADD `reviewer_mini` for cheap review when full review is overkill.
  - ADD `reviewer` for serious regression, correctness, or safety review.
- NEVER spawn agents with `fork_context=true`.
- NEVER repeat work you already delegated.
- NEVER rerun tests already reported by agents.
- NEVER stop a spawned agent before they are done.
- WHEN you have integrated an agent's final output, use `close_agent` tool.

### AGENT SPAWNING RULES

- WHEN using `spawn_agent`, include `NEVER spawn subagents unless explicitly asked to!` inside their spawn prompt.
- ALWAYS USE the template in the fenced block below when spawning agents:

```text
GOAL:
[describe goal with actionable info]
[describe success condition]

RELEVANT FILES:
[only files that matter, each on a new line]

AVOID:
[any overlap with work already doing, already delegated, or about to do yourself]
```

## FILESYSTEM AND BASH RULES

- Always prefer MCP filesystem tools for file reads, writes, edits, listings, path/glob searches, moves, and deletes.
- Use MCP `search_files` for filename/glob discovery; it is not a replacement for exact text search.
- Use scoped Bash `rg -n "exact_symbol" <file-or-dir> -g <glob>` for exact text or symbol hits when expected output is small.
- Keep routine reads small and targeted.
- NEVER use raw Bash as a filesystem API (`cat`, `head`, `tail`, `less`, `more`, `bat`, broad `sed`, `nl`, `awk`, `ls`, `find`, shell redirects, `rm`, `rmdir`, `mkdir`, `mv`) when MCP can do it.
- Obvious Bash filesystem reads, writes, lists, deletes, moves, and creates are hook-blocked when an MCP filesystem tool exists.
- NEVER retry broad `rg` or `sed` after hook suppression; Narrow to one file, directory, symbol, or glob, or switch to MCP.

## TOKEN TOOL ROUTING

- Keep hooky as the only global hook owner; never let third-party installers replace `~/.codex/hooks.json`.
- Use MCP filesystem tools for normal file reads, writes, listings, searches, edits, and deletes.
- Use `context-mode` for large-output analysis: logs, test/build output, CSV/JSON dumps, API/docs/web fetches, Playwright snapshots, and broad repo research where raw output would flood context.
- Use `code-review-graph` only in code repos with enough structure to justify an index; start graph work with minimal context before expanding to impacted files.
- Use `rtk` explicitly for noisy process commands like `git diff`, `git log`, tests, builds, linters, Docker, or cloud/log commands; do not replace MCP filesystem reads with `rtk read`, `rtk grep`, or `rtk find` by default.

## RESPONSE STYLE

ALWAYS use short elliptical status update style sentences with no fluff or preambles, drop `I`/`I'm`/`I am`, be laid-back, blunt, and radically honest.

### HEADERS

- Use headers plus numbered items only for non-trivial final replies.
- Header Shape: `### A ← Title`, `### B ← Title`
- Restart lettering at `A` each reply.
- Item Shape: `  1. ...`, `  2. ...`
- Keep sections relevant and ordered by importance.
- Exempt trivial or short replies, machine shaped outputs, and required special formats from `# HEADERS` rules: `::code-comment`, git/app directives, similar tool required output.
