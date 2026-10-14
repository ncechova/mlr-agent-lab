# Plan

## Status

**Phase: setup — tooling and agent rules in place, no experiments started.**

### Done

- Portable, non-admin install of `gh` 2.102.0 and `uv` 0.13.1 into
  `~/.local/bin` (already on the user PATH).
- Repository created on GitHub as **public**, default branch `main`.
- Python project hygiene: `.gitignore`, `.gitattributes` (LF normalisation,
  binary guards).
- `uv` project defined — `pyproject.toml` exists, so `uv add` works.
- Working rules written down: `AGENTS.md`, loaded by `CLAUDE.md`.
- Session log started: `RUNS.md`.

### Open questions

- Should commits be attributed to the school address currently in the Git
  config, or to the GitHub noreply address? The public repo currently exposes
  `256971@vutbr.cz` in commit metadata; the account's "keep email private"
  setting does not cover this. Unresolved — see `RUNS.md`.
- No test runner, linter, or CI chosen yet.

## Next step

**Decide what the first experiment actually is.** The repo currently contains
rules and a placeholder script and nothing to show for either.

Concretely, in order:

1. Pick the first experiment and write it up here under a new "Phase 1"
   heading, with something falsifiable as its success criterion.
2. Add the tooling that experiment needs via `uv add --dev` — realistically
   `pytest` and `ruff` — and wire up a check command so rule 3 of `AGENTS.md`
   has something concrete to point at.
3. Add CI that runs that check on push, so "run checks before saying it is
   done" is enforced rather than trusted.

## Deferred

- Deciding on a repo structure for multiple experiments (subdirectories per
  experiment vs. branches) — wait until there is a second one.
- Resolving the commit-email question above.
