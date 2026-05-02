# Cleanup Tooling Reference

Use this as a repo-agnostic pattern matrix during cleanup analysis preflight. Prefer commands already present in the repo through package scripts, task runners, local binaries, or documented workflows. Adapt examples to the package manager, workspace root, and scope. Do not run examples blindly.

## Tool Selection Rules

- Detect repo languages, package managers, monorepo layout, native scripts, and existing analyzers before choosing tools.
- Prefer repo-native commands over global tools and one-off downloads.
- Run the narrowest useful command for the cleanup target. Use repo-wide scans only when the task scope is repo-wide or the tool needs whole-project context.
- Exclude generated, vendored, fixture, dist, coverage, cache, build, and lockfile areas unless the parent explicitly scoped cleanup there.
- Analyzer output is only a signal. Verify references, entrypoints, dynamic loading, codegen, plugin registration, and framework conventions before editing.
- If a useful analyzer is missing, add it only as a repo dev dependency when the dominant package manager is clear or can be inferred. Report those dependency changes separately.
- If no safe install path is clear, skip the tool and report a coverage gap.

## JavaScript and TypeScript

- Baseline: package scripts for `typecheck`, `lint`, `test`, `build`; `tsc --noEmit`.
- Unused code and deps: `knip`, `ts-prune`, `tsc --noUnusedLocals --noUnusedParameters`.
- Duplication: `jscpd`, ESLint duplicate import rules, repeated helper/type searches.
- Cycles and dependency shape: `dependency-cruiser`, `madge`.
- Weak types: TypeScript diagnostics, ESLint `no-explicit-any`, searches for `any`, broad records, and type assertions.
- Defensive errors: ESLint `no-empty`, `no-useless-catch`, and targeted searches for swallowed errors.
- Legacy/slop: `knip`, package dependency review, searches for `deprecated|legacy|fallback|compat|old|TODO|stub|placeholder|temporary|hack`.

## Python

- Baseline: `pytest`, `ruff`, `mypy`, `pyright`.
- Unused code and deps: `vulture`, Ruff `F401,F841`, import/package review.
- Duplication: Ruff `SIM,PLR`, pylint duplicate-code, repeated dataclass/pydantic/helper searches.
- Cycles and dependency shape: `pydeps`, `grimp`, import graph review.
- Weak types: `mypy`, `pyright`, searches for `Any`, `type: ignore`, untyped defs, and broad dict shapes.
- Defensive errors: Ruff `E722,B,B904,TRY`, broad `except Exception`, swallowed exception searches.
- Legacy/slop: `vulture`, Ruff `ERA,FIX`, searches for deprecated/fallback paths and placeholder comments.

## Rust

- Baseline: `cargo test`, `cargo clippy --all-targets --all-features`, `cargo fmt --check`.
- Unused code and deps: compiler dead-code warnings, `cargo udeps`, `cargo machete`.
- Duplication: clippy signals, repeated impl/match/helper searches.
- Cycles and dependency shape: `cargo tree -d`, feature and dependency graph review.
- Weak types: clippy casts, `Box<dyn Any>`, stringly typed enums, broad generics.
- Defensive errors: clippy needless match/map_err signals and catch-all error mapping searches.
- Legacy/slop: feature flag review, dependency review, searches for deprecated/fallback paths and placeholders.

## Go

- Baseline: `go test ./...`, `go vet ./...`, `staticcheck ./...`, `gofmt -d`.
- Unused code and deps: compiler/test reachability, `staticcheck`, `go list`, module review.
- Duplication: repeated helper/interface/table-test searches and `gofmt -d`.
- Cycles and dependency shape: `go list -deps -json`; direct cycles are compiler failures.
- Weak types: searches for `interface{}`, `any`, `map[string]interface{}`, and unchecked type assertions.
- Defensive errors: `staticcheck`, ignored errors, redundant wrappers, swallowed error searches.
- Legacy/slop: `go mod why`, deprecated package review, placeholder and fallback searches.

## Swift, C, C++, and Shell

- Swift baseline: `swift build`, `swift test`, `swiftlint lint`, `periphery scan`.
- Swift cleanup signals: unused declarations, force casts/unwraps, `Any`, duplicated models, fallback paths.
- C/C++ baseline: repo build command, compiler warnings, `clang-tidy`, `include-what-you-use`.
- C/C++ cleanup signals: unused includes/symbols, include graph issues, duplicated helpers, legacy compile flags.
- Shell baseline: `shellcheck`, `shfmt -d`.
- Shell cleanup signals: swallowed errors, dead flags, unused variables, placeholder comments, obsolete scripts.

## JVM, .NET, PHP, and Ruby

- JVM baseline: Gradle or Maven tests/checks, `jdeps`, `spotbugs`, `detekt`.
- .NET baseline: `dotnet test`, `dotnet build`, `dotnet format --verify-no-changes`, Roslyn analyzers.
- PHP baseline: Composer scripts, `phpstan`, `psalm`, `composer-unused`.
- Ruby baseline: `rspec`, `rubocop`, `steep`, `debride`.
- Map tools by signal: duplicate detectors for duplication, dependency graph tools for cycles, type analyzers for weak types, unused/dependency analyzers for dead code and legacy paths, lint/search for defensive errors and slop.

## Optional Slop Scan

If `/Users/admin/Desktop/codex/hooky/hooks/slop_scan.py` exists, it can be used as an advisory repo or directory scan:

```bash
python3 /Users/admin/Desktop/codex/hooky/hooks/slop_scan.py
```

Run it from the repo root or smallest target directory. It is optional, local-machine-specific, and never a required dependency. Report missing support tools such as `semgrep`, `ast-grep`, `shellcheck`, and `shfmt` as coverage gaps.
