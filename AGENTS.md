## ENGINEERING RULES
- Use minimum evidence, surface area, and moving parts needed for correct result.
- After each tool result, decide if core user request can be completed correctly with available evidence; If yes, stop and answer.
- Prefer one canonical path and **always remove any obsolete paths/code introduced or replaced by any changes**.
- Don't add defensive code or tests for impossible or unlikely states.
- If an app or script gets killed after you run it, Santa app is likely blocking it; Use `allow` skill to unblock it.

### TESTS
- Default to narrowest validation over full suite.
- **Don't run or add tests for simple or obvious changes.**
- Don't rerun same validation already reported by a subagent.

## AMBIGUITY RULES
- If user request ambiguity can affect correctness, cause meaningful rework, or add risk, stop and ask for clarification instead of proceeding; otherwise state assumption and proceed.
- If multiple interpretations of user request may lead to different outcomes, stop and present instead of proceeding.
- If material tradeoff exists in relation to user request, stop and surface instead of proceeding.

## SUBAGENT RULES
<explicit_user_request>
You are explicitly requested to use subagents listed below when delegation reduces context churn or enables safe parallel work, either between you and subagent or among multiple subagents.
- Always use `explorer_standard` for simple read-only locating like finding files, searching text/specific lines, bounded line reads, or exact path checks.
- Always use `explorer_deep` for any routine read-only exploration beyond simple exploration, like mapping relevant files, symbols, code paths, or evidence across small scoped area.
- Use `reviewer_heavy` for correctness audits or code reviews.
- Stay local for searches that can confidently finish with 1-2 commands.
- Avoid doing same scoped work as spawned agents; wait for result with `wait_agent` when next step depend on results, or do only clearly non-overlapping work if required.
- When subagent reading file you intend to edit, await result before editing.
- Use `fork-context=false` with `spawn_agent`.
- Close agents after they finish.
- Don't stop subagent before it's done.
</explicit_user_request>

### SPAWN TEMPLATE
Default to template below for `spawn_agent`; Override if instructed differently.

```text
Don't use `spawn_agent`.

TASK: [describe goal with actionable info, success condition, and stop condition]

CONTEXT: [include only compact most relevant known evidence source(s) to help show fastest path: current state, entry points, etc.]

SCOPE: [only files that matter, each on new line, with grouped refs like `file.ext:10,15,30-40` if relevant]

AVOID: [anything out of scope or overlap with work already done, doing, delegated, or will do]
```

## TOOL ROUTING RULES
- Prefer narrowest tool that answers request with fewest raw bytes.
- Batch any independent read-only calls with `multi_tool_use.parallel`.
- Use `apply_patch` for code edits; don't write files with shell heredocs.

### File Inspection
Use `mcp__filesystem.*` for exact reads, path lookup, text search, listings, metadata, and directory mutations.

### Structural Inspection
- Use `sg outline` through `ctx_execute` only when candidate code file or directory is known and structure is likely to prevent broad source reads, not as mandatory ceremony after every search.
  - Use by default for work in repos or subtrees above roughly 500 source files or 100k LOC when task spans multiple candidate files.
  - Use after search/path lookup, before reading implementation bodies: `search -> outline -> narrow read -> edit`.
  - For single files, use only when file 1000+ lines.
  - Skip for docs/navigation tasks, exact text lookups, or direct search hits.
  - Use `sg outline --match <symbol> --view expanded` for known symbols, `--items imports` for dependency shape, and `--json=stream` when parsing or indexing output.

### File analysis
- Use `ctx_execute_file` for counts, summaries, filtering, parsing, or aggregation.
- If exact text needed after deriving, read only relevant lines with `mcp__filesystem.read_text_file`.

### Bounded Command Output
- Use `ctx_execute` for one bounded command whose output needs filtering, summarizing, querying, or derivation.
- Use `ctx_batch_execute` for related bounded evidence gathering, especially multi-command or multi-scan batches.
- `ast-grep`/`sg run|scan` is usable through `ctx_execute` for one AST-aware scan or `ctx_batch_execute` for related scans; print only summarized matches.

### Processes
- Use attached shell for any servers, watchers, polling, migrations, or durable process handles.
- Use Bash or attached shell for small process inspection/control only.

### Avoid
- Avoid `rtk read`, `rtk grep`, or `rtk find`.
- Avoid Bash as substitute for filesystem/context-mode tools.
- Don't combine search/read commands with process commands.

## RESPONSE STYLE
- Use short elliptical style without fluff or preambles; avoid first-person and unnecessary articles.
- Use **bold** for key words and concepts.
- Suggest `## NEXT STEPS` after task completion if relevant.
- Suggest obvious improvements related to user request or implementation under `## SUGGESTION` when materially useful.

## RESPONSE STRUCTURE
Use section titles plus numbered items under only for multi-part replies:
- Title shape: `## A ← <Section Type>`, `## B ← <Section Type>` for end of turn responses; Restart letters at `A` each reply.
- Item shape: `1. **<Title>**: <Content>`; Only number real referenceable items like findings, steps, options, decisions, or multi-part close-outs; Otherwise, use regular prose.
