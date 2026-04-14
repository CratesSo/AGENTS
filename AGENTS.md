# SIMPLICITY FIRST
- ALWAYS choose simplest solution with fewest moving parts.
- ALWAYS reduce total code when possible. EXAMPLE: If you write 200 lines and it could be 50, rewrite it.
- ALWAYS prefer version a tired senior engineer can understand in one read.
- ALWAYS keep one canonical path and hard cut over to it.
- WHEN RULES CONFLICT: prefer simpler code, fewer lines, one canonical path, then formatting.
- ALWAYS touch minimum surface area needed.
- ALWAYS run narrowest useful validation after change.
- ALWAYS surface tradeoffs.
- WHEN TRADEOFFS EXIST: ALWAYS choose fewer moving parts, and less total code.
- IF a simpler approach exists, say so and use it.
- IF changes create dead imports, vars, or code, remove them.
- IF multiple interpretations exist, present them and don't pick silently.
- IF something is unclear, stop, name confusion, and ask.
- NEVER add speculative features, fallback paths, defensive branches, or parallel implementations.
- NEVER add helpers, wrappers, hooks, classes, config bags, or abstractions unless they remove more code or branching than they add.
- NEVER keep duplication, nested branching, or indirection that can be deleted cleanly.
- NEVER add error handling for impossible scenarios.

# DECISION RULES
- ALWAYS keep one-use logic inline unless block is hard to read.
- Prefer boring control flow, early returns, and explicit locals.
- Extract only when logic repeats or a block is hard to read.
- Delete old paths in same change when replacing behavior.
- Never refactor things that are not broken.
- Mention unrelated dead code when seen.

# LESSONS
- READ `tasks/lessons.md` before debugging, regressions, or complex edits in problem areas.
- DON'T read `tasks/lessons.md` for simple tweaks or small single file edits.
- UPDATE `lessons.md` only after a durable fix with a reusable rule.
- ALWAYS tighten or delete existing lessons over adding similar lessons.

# SUBAGENT USE
- STAY LOCAL for tiny obvious tasks (never use subagents for trivial one-shot tasks like single-command answers, tiny file creation, or obvious single-file edits).
- ALWAYS USE `explorer` for read-only exploration, evidence gathering, tracing, and web/doc checks.
- USE `worker_mini` for straightforward edits with clear scope.
- USE `worker` for risky or complex implementation.
- ADD `reviewer_mini` for cheap review when full review is overkill.
- ADD `reviewer` for serious regression, correctness, or safety review.
- NEVER repeat work already delegated.
- NEVER rerun tests already reported by agents.
- NEVER spawn with `fork_context=true`.
- NEVER stop `reviewer`, `reviewer_mini`, `cleanup`, or `explorer` before they are done.

## SUBAGENT SPAWNING
- WHEN SPAWNING `cleanup`, include `NEVER spawn subagents!`.
- WHEN SPAWNING `explorer` or `explorer_mini`, include `NEVER spawn explorer, probe, cleanup, reviewer, or reviewer_mini agents!`.
- ALWAYS USE THIS TEMPLATE when spawning agents:
```md
### OBJECTIVE
- describe goal and success condition
### RELEVANT FILES
- list only files that matter
### AVOID
- any overlap with work already doing, already delegated, or about to do yourself
```

# BASH RULES
- Start `rg` narrow: one symbol, one file, one directory, or one `-g` glob.
- Prefer one bounded broader search when scope can be mapped in one pass.
- Never retry `rg` with same broad scope plus minor term changes.
- Never run `find ... | xargs sed` repo dumps or chained browse commands just to look around.
- Do `rg -n <symbol>` before `sed` when possible.
- Keep routine reads small and targeted.
- Recover after `sed` suppression with one canonical path: `rg -n` one symbol, then `mcp__filesystem__read_text_file` with `head <=120` on one file.
- After suppression, do not retry broad same file or same turn searches (pick one canonical file and read one small window).
- Prefer `git diff --stat` first, then single-file diffs.

# RESPONSE STYLE
- ALWAYS use short elliptical status update style sentences with no fluff or preambles.
- ALWAYS drop `I`/`I'm`/`I am`.
- ALWAYS be laid-back, blunt, radically honest.

## HEADERS
- Use headers plus numbered items for non-trivial final replies.
- Header Shape: `### A ← Title`, `### B ← Title`
- Restart lettering at `A` each reply.
- Item Shape: `  1. ...`, `  2. ...`
- Keep sections relevant and ordered by importance.
- Exempt trivial replies, machine shaped outputs, and required special formats from `# HEADERS` rules: `::code-comment`, git/app directives, and similar tool-required output.
