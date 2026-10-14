# Runs

Log of agent sessions: the model used, what was asked, and how the result was
checked. Newest last.

Model is recorded as reported by the CLI session (`/status` / `/context`).
Note: this project runs with a custom model alias, so the value below may not
name an underlying Claude model — confirm with `/status` if it matters.

---

## Run 1 — 2026-10-14 — tool install

**Model:** `agentic` (per `/context`; 200k token window)

**Asked:** Install GitHub CLI and uv into the user folder without administrator
rights, and check both work.

**Done:** Downloaded portable release archives with `curl`, unpacked the plain
executables into `~/.local/bin` (already on the user PATH). No installer, no
UAC prompt, no PATH edit. `gh` 2.102.0; `uv` / `uvw` / `uvx` 0.13.1.

**Checked:**

- `gh --version` → `gh version 2.102.0`. `gh auth api` reached GitHub and
  returned the expected "not logged in" message (correct behaviour at that
  point, not a fault).
- `uv venv` created an environment; `uv run --python 3.12 python -c ...`
  executed; `uv pip install requests` then `import requests` →
  `requests 2.34.2 on 3.12.10`; `uvx --from cowsay cowsay -t "uv works"`
  resolved a real PyPI package and produced output.
- Fresh shell with no `export` resolved both binaries from PATH.

**Not verified at the time:** neither tool was tested against a real
authenticated GitHub operation until Run 2.

**Side finding:** `uv python list` picks up `C:\Program Files\Inkscape\bin\python.exe`
first on PATH and fails to inspect it (`Unknown operating system:
mingw_x86_64_ucrt_gnu`). uv falls back correctly; pin `--python 3.12` if it
matters.

---

## Run 2 — 2026-10-14 — repository created and pushed

**Model:** `agentic`

**Asked:** Use `gh` to create a public repo `mlr-agent-lab` from this folder;
show what would be uploaded before pushing.

**Done:** Staged locally first and showed the full contents of every file for
review. On approval, added `.gitignore`, `.gitattributes` and a stub README,
committed, then `gh repo create mlr-agent-lab --public --source=. --push`.

**Checked:**

- `gh api repos/ncechova/mlr-agent-lab` returned 404 before creation (no
  overwrite risk).
- After push: `gh repo view --json visibility,defaultBranchRef` →
  `visibility: PUBLIC`, branch `main`; `gh api .../contents` listed the 4
  expected files with sizes matching the local staging preview; remote HEAD
  `d9ff70a` matches local `git log`.
- Line endings: `git show HEAD:README.md | file -` reports UTF-8 text with no
  CR, confirming `.gitattributes` normalisation took effect in stored blobs.

**Mistakes made:** two `Write` calls were rejected as malformed JSON and
retried (no duplicates left behind). A `uvx` test used a non-existent package
name and a wrong flag (`-c` is uv's `--constraints`, not a command separator),
producing two failed test runs before the check was valid.

---

## Run 3 — 2026-10-14 — email exposure claim, tested rather than asserted

**Model:** `agentic`

**Asked:** User pointed out that "Keep my email addresses private" is enabled,
questioning a claim that their school email was publicly visible.

**Done:** Tested both surfaces separately instead of reasoning about them.

**Checked:**

- `gh api users/ncechova` as an anonymous caller → `"email": null`. The account
  setting **is** working; the address is hidden on the profile.
- `git clone` over HTTPS with credentials disabled, then `git log --pretty=fuller`
  → `Author: ncechova <256971@vutbr.cz>`. The commit object carries the address
  regardless of the account setting, because it came from local
  `git config user.email` and Git clients serve it verbatim.

**Conclusion:** the user's setting is correct and effective for the profile;
commit metadata is a separate surface it does not cover. Left unresolved
deliberately — rewriting published history needs a force-push, and VUT
coursework may require school-identity attribution.

**Note:** an attempt to confirm the same thing via anonymous REST calls returned
HTTP 403 (API rate limit), which is inconclusive rather than evidence either
way; the git-clone test above is the one that settles it.

---

## Run 4 — 2026-10-14 — working rules

**Model:** `agentic`

**Asked:** Write `AGENTS.md` with the working rules (read `README.md` and
`PLAN.md` first; use uv, no global `pip install`; run checks before saying done),
make `CLAUDE.md` load it, and create `README.md`, `PLAN.md`, `RUNS.md`.

**Done:** Wrote all five files. `CLAUDE.md` pulls in `AGENTS.md` via an `@`
import so both files stay in force without duplicating the rules.

**Checked:**

- Confirmed none of the five files existed before writing — no silent
  overwrites (`README.md` was replaced intentionally, being a stub).
- Tested the rules against reality before enshrining them: `uv add requests`
  with no project file → `error: No pyproject.toml found` (exit 2);
  `uv run python x.py` → works. The `uv add` rule was therefore not runnable
  as given.

**Consequence:** added a minimal `pyproject.toml` so `uv add` — a rule this
file mandates — actually functions. See the note in the commit.

**Not verified:** that `CLAUDE.md`'s `@AGENTS.md` import resolves as intended.
That only shows up at the start of a fresh Claude Code session; check with
`/context` in a new session and confirm `AGENTS.md` appears as loaded.

---

## Run 5 — 2026-10-14 — rules committed and pushed

**Model:** `agentic` (alias — confirm with `/status`)

**Asked:** Commit and push the working-rules files.

**Done:** Staged 7 paths — 6 new (`AGENTS.md`, `CLAUDE.md`, `PLAN.md`,
`RUNS.md`, `pyproject.toml`, `uv.lock`) plus modified `README.md` — committed
and pushed to `origin/main`.

**Checked:**

- Reviewed the staged file list before committing — confirms `.venv/` and
  `uv`'s cache are excluded by `.gitignore`, and that no untracked scratch
  files rode along.
- After push: remote `main` HEAD matches local `git rev-parse HEAD`, and
  `gh api .../contents` lists the expected file set.

**Still not verified:** the `@AGENTS.md` import in `CLAUDE.md` (carried over
from Run 4 — needs a fresh session). Nothing in this run created or changed any
Python behaviour, so no code was executed beyond the Run 4 `uv sync` /
`uv run python hello.py` checks.
