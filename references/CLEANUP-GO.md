# Go Cleanup

- Baseline: `go test ./...`, `go vet ./...`, `staticcheck ./...`, `gofmt -d`.
- Unused code and deps: compiler/test reachability, `staticcheck`, `go list`, module review.
- Duplication: repeated helper/interface/table-test searches.
- Cycles and dependency shape: `go list -deps -json`; direct cycles are compiler failures.
- Weak types: searches for `interface{}`, `any`, `map[string]interface{}`, and unchecked type assertions.
- Defensive errors: `staticcheck`, ignored errors, redundant wrappers, swallowed error searches.
- Legacy/slop: `go mod why`, deprecated package review.
