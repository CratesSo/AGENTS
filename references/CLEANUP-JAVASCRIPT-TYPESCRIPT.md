# JavaScript and TypeScript Cleanup

- Baseline: package scripts for `typecheck`, `lint`, `test`, `build`; `tsc --noEmit`.
- Unused code and deps: `knip`, `tsc --noUnusedLocals --noUnusedParameters`.
- Duplication: `jscpd`, ESLint duplicate import rules, repeated helper/type searches.
- Cycles and dependency shape: `dependency-cruiser`, `madge`.
- Weak types: TypeScript diagnostics, ESLint `no-explicit-any`, searches for `any`, broad records, and type assertions.
- Defensive errors: ESLint `no-empty`, `no-useless-catch`.
- Legacy/slop: package dependency review.
