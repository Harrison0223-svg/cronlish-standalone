# Contributing

Keep each topic branch focused. Include a reproducible synthetic fixture, expected and actual results, and exact test commands. Do not include private data, secrets, or unlicensed assets. Add a regression test for changes in behavior. Disclose material AI assistance and review the diff yourself before submitting.

## Repository scope and checks

For changes to this independent copy, open pull requests against Harrison0223-svg/cronlish-standalone. For contributions to the original project, follow [upstream instructions](https://github.com/ssmurfgg04-gif/cronlish) and open a separate upstream pull request.

Use the standard library for runtime code, type public functions, and keep existing output deterministic. See [AGENTS.md](AGENTS.md).

With pytest installed:

    python -m pytest -q
