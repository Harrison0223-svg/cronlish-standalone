# Instructions for GitHub Copilot and other AI coding agents

`AGENTS.md` is the canonical agent contract; this file mirrors it for tools
that only read this path.

## Build and test commands

- `python -m pytest -q` must pass before you open a PR. Non-negotiable.
- Use `python -m pytest --tb=short` for compact tracebacks while iterating.
- No other build steps: the project is pure stdlib Python (>= 3.9).

## Code style

- Standard library only. Do not add dependencies.
- Type hints on all public functions.
- Keep `describe()` output deterministic: the same input must always produce
  the same sentence. If you change wording, update the tests in the same PR.

## PR rules

- Keep PRs small. One issue per PR.
- Include tests for any new behavior.
- Reference a relevant issue in this repository when one exists.
- Do not reformat or "modernize" code unrelated to your issue.
