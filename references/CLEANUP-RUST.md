# Rust Cleanup

- Baseline: `cargo test`, `cargo clippy --all-targets --all-features`, `cargo fmt --check`.
- Unused code and deps: compiler dead-code warnings, `cargo machete`.
- Duplication: clippy signals, repeated impl/match/helper searches.
- Cycles and dependency shape: `cargo tree -d`, feature and dependency graph review.
- Weak types: clippy casts, `Box<dyn Any>`, stringly typed enums, broad generics.
- Defensive errors: clippy needless match/map_err signals.
- Legacy/slop: feature flag and dependency review.
