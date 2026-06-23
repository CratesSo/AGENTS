# Python Cleanup

- Baseline: `pytest`, `ruff`, `mypy`, `pyright`.
- Unused code and deps: `vulture`, Ruff `F401,F841`, import/package review.
- Duplication: Ruff `SIM,PLR`, repeated dataclass/pydantic/helper searches.
- Cycles and dependency shape: `pydeps`, `grimp`, import graph review.
- Weak types: `mypy`, `pyright`, searches for `Any`, `type: ignore`, untyped defs, and broad dict shapes.
- Defensive errors: Ruff `E722,B,B904,TRY`, broad `except Exception`.
- Legacy/slop: Ruff `ERA,FIX`.
