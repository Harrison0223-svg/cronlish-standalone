# cronlish-standalone

[![Python 3.9+](https://img.shields.io/badge/python-3.9%2B-blue)](https://www.python.org/)
[![License: MIT](https://img.shields.io/badge/license-MIT-green)](LICENSE)

Translate cron expressions into plain human language.

This independent copy of [cronlish](https://github.com/ssmurfgg04-gif/cronlish) is based on [the source snapshot](https://github.com/Harrison0223-svg/cronlish/tree/2ae2dd5aa81562c68c9321db75f1621559408863). The original project is by ssmurfgg04-gif and contributors. See [LICENSE](LICENSE) for the original MIT notice.

| Expression | cronlish says |
| --- | --- |
| `*/5 * * * *` | Every 5 minutes |
| `0 9 * * MON-FRI` | At 09:00, Monday through Friday |
| `30 14 1 * *` | At 14:30 on day 1 of the month |

## Why

Cron syntax is fifty years old and still machine-first. `30 14 1 * *` is precise, but nobody reads it fluently: humans double-check it in their heads, and autonomous agents that need to verify scheduling logic have no cheap way to confirm what an expression actually means. `cronlish` turns any standard five-field expression into one deterministic English sentence, so both audiences can verify scheduling at a glance. The output for a given expression is always byte-identical, which makes it safe to pin in tests, logs, and agent reasoning.

## Install from this repository

```bash
git clone https://github.com/Harrison0223-svg/cronlish-standalone.git
cd cronlish-standalone
python -m pip install .
```

## Usage

Python API:

```python
from cronlish import describe, DescribeError

describe("*/5 * * * *")      # 'Every 5 minutes'
describe("0 9 * * MON-FRI")  # 'At 09:00, Monday through Friday'
describe("30 14 1 * *")      # 'At 14:30 on day 1 of the month'
describe("0 0 1 1 *")        # 'At 00:00 on day 1 of the month in January'
describe("@daily")           # 'At 00:00 every day'

describe("banana")           # raises DescribeError with a helpful message
```

CLI:

```console
$ cronlish '*/5 * * * *'
Every 5 minutes
$ cronlish '30 14 1 * *'
At 14:30 on day 1 of the month
```

Exit codes: `0` success, `2` malformed expression (message on stderr). Quoting is optional — `cronlish */5 * * * *` works too.

## Supported syntax (v0.2)

- Standard five fields: minute, hour, day-of-month, month, day-of-week.
- Values, ranges (`a-b`), lists (`a,b,c`), and steps (`*/n`, `a-b/n`) in all five fields.
- Macros: `@hourly`, `@daily`/`@midnight`, `@weekly`, `@monthly`, `@yearly`/`@annually` (case-insensitive).
- Day-of-week: `0`–`7` (both `0` and `7` are Sunday) and names `MON`–`SUN`, including ranges like `MON-FRI`.
- Output is deterministic; lists are rendered in sorted order.

Known quirks, described honestly: when both day-of-month and day-of-week are restricted, classic cron fires when *either* matches (OR) — cronlish says so ("... on day 1 of the month or on Monday").

This snapshot does not support month names like `JAN` or the `--locale-style` CLI flag.

## Upstream credits

Thanks to [@Muhtasim-Munif-Fahim](https://github.com/Muhtasim-Munif-Fahim) for step values in day-of-month, month, and day-of-week ([upstream #5](https://github.com/ssmurfgg04-gif/cronlish/pull/5)) and cron macros ([upstream #6](https://github.com/ssmurfgg04-gif/cronlish/pull/6)).

For historical upstream bounty references, see [BOUNTIES.md](BOUNTIES.md).

## Development

```bash
python -m pytest -q          # must pass
python -m pytest --tb=short  # same tests, compact tracebacks
```

No dependencies beyond the Python standard library (pytest is dev-only). Python ≥ 3.9.
