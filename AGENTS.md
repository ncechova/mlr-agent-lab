# Working rules

These rules apply to any coding agent working in this repository.

## 1. Read these first

Before changing anything, read:

1. [`README.md`](README.md) — what this project is for and how to run it
2. [`PLAN.md`](PLAN.md) — where the work stands and what comes next

Do not start a task that contradicts the plan without saying so first.

## 2. Python: always use uv

The project environment is managed with [uv](https://docs.astral.sh/uv/).

| Task | Command |
| --- | --- |
| Run a script | `uv run python <file>.py` |
| Add a dependency | `uv add <package>` |
| Add a dev dependency | `uv add --dev <package>` |
| Run a tool | `uv run <tool>` |
| Sync the environment | `uv sync` |

**Never** run `pip install` against a global or system Python, and never
activate a virtualenv by hand and call `pip` from it. Everything goes through
uv so `pyproject.toml` and `uv.lock` stay the single source of truth.

If a dependency you need is missing, add it with `uv add` — do not work around
it by importing from an unmanaged interpreter.

## 3. Verify before claiming it is done

No task is finished until it has been checked, and the check has been shown.

- Run the relevant thing and look at its output. "It should work" is not a
  result.
- If you edited code, execute it. If you added a feature, exercise the new
  path, not just the old one.
- Prefer a check that could actually fail. A command whose output you never
  read proves nothing.
- Report failures as failures, with the real output. Say plainly if you skipped
  a step or could not verify something.
- Do not claim a fix works because you made the edit; claim it because you ran
  it.

When no check exists yet, say what you could not verify rather than implying
the work is proven.

## 4. Recording work

Append an entry to [`RUNS.md`](RUNS.md) when a session produces work worth
tracking: what was asked, what was done, and how it was checked. Keep entries
short — the point is an audit trail, not a narrative.

## 5. Scope

This is a lab folder for experiments. Favour small, reversible changes; ask
before anything irreversible or outward-facing (force-pushes, deleting files,
publishing, spending money).
