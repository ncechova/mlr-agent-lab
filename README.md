# mlr-agent-lab

A small public lab for experimenting with coding agents and the Python tooling
around them — how to set up an environment an agent can drive safely, what
working rules make an agent useful rather than merely fast, and what actually
breaks in practice.

## Requirements

- [uv](https://docs.astral.sh/uv/) — manages the virtualenv and all packages
- Python 3.12+ (uv will use an existing interpreter or fetch one)

No global `pip install` is used anywhere in this project.

## How to run things

Set up the environment (once, and again after pulling dependency changes):

```bash
uv sync
```

Run the placeholder script:

```bash
uv run python hello.py
```

Add a dependency — this edits `pyproject.toml` and `uv.lock` for you:

```bash
uv add <package>
uv add --dev <package>   # development-only, e.g. a linter or test runner
```

Run an installed tool inside the environment:

```bash
uv run <tool>
```

## Layout

| Path | Purpose |
| --- | --- |
| `hello.py` | Placeholder entry point |
| `AGENTS.md` | Working rules for any coding agent in this repo |
| `CLAUDE.md` | Loads `AGENTS.md` into Claude Code |
| `PLAN.md` | Current state of the work and the next step |
| `RUNS.md` | Log of sessions: what was asked, done, and how it was checked |

Agents working here should read `AGENTS.md` first, then `README.md` and
`PLAN.md`.
