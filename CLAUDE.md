# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## What this repo is

**Learn OpenUSD** is a Sphinx documentation site (published at https://docs.nvidia.com/learn-openusd/latest/) that teaches OpenUSD development and prepares developers for the OpenUSD Development Certification. Lessons are **MyST Markdown "notebooks"** with executable Python (`pxr`/`usd-core`) code cells that MyST-NB runs at build time. Dependencies are managed with `uv`; large binary assets (images, video, `.usd*`) are stored in **Git LFS**.

Two companion docs are authoritative and worth reading for any non-trivial task:
- **AGENTS.md** — detailed agent guide: content structure, how to add lessons/modules, and a content-review checklist.
- **STYLEGUIDE.md** — terminology, MyST syntax, code-cell conventions, the notebook test harness, and required file headers.

## Commands

```bash
# Build the docs (executes all Python code cells, runs custom build hooks)
uv run sphinx-build -M html docs/ docs/_build/

# Clean build (caches notebook execution by default; remove to force re-run)
rm -rf docs/_build/

# Preview the built site
uv run python -m http.server 8000 -d docs/_build/html/   # then open http://localhost:8000

# Launch the executed notebooks in JupyterLab (extra args pass through to `jupyter lab`)
uv run launch_notebooks                  # requires a prior build
uv run launch_notebooks --ip=0.0.0.0     # reach the server from another machine

# Tests
uv run pytest                                                  # all
uv run pytest tests/test_docs_stage_setting.py                 # one file
uv run pytest tests/test_docs_stage_setting.py::TestStageNotebook::test_full_notebook   # one test
```

**Build before testing.** The doc tests execute notebooks from `docs/_build/jupyter_execute/`, which only exists after a Sphinx build. Run `sphinx-build` first or the tests raise `FileNotFoundError`. (`tests/test_notebook_harness.py` and `tests/test_workshop_prep.py` are exceptions — they use an `.ipynb` fixture / the filesystem and need no build.)

A clean build is expected to emit ~163 warnings; the rule is **no _new_ warnings or errors** (CONTRIBUTING.md, AGENTS.md).

## Architecture

### The `lousd` package (`src/lousd/`)
Installed package exposing two console scripts (see `pyproject.toml [project.scripts]`):
- `launch_notebooks` (`launch_notebooks.py`) — opens JupyterLab rooted at `docs/_build/jupyter_execute/`, forwarding extra args to `jupyter lab`.
- `workshop_prep` (`workshop_prep.py`) — **destructive, in-place** transform of `docs/` into a trimmed "Applied Concepts Workshop": rewrites `index.md`, deletes modules in `MODULES_TO_REMOVE`, prunes setup pages, and rewrites cross-references to removed content into external links. Run on a throwaway checkout; review with `git diff`.
- `lousd.directives` — a Sphinx extension (registered in `conf.py` `extensions`) providing the `{kaltura}` video-embed directive.

### Build pipeline (`docs/conf.py`)
Beyond standard Sphinx config, `conf.py` wires several `build-finished` / `builder-inited` hooks that produce real outputs — changing build behavior usually means editing these:
- `prepare_executed_notebooks` — rewrites image paths in executed `.ipynb`s and copies `exercise_content/` next to them so notebooks' relative paths (`../exercise_content/...`, `_assets/...`) resolve at runtime. **The test harness depends on this layout.**
- `create_exercises_archives` — zips each `docs/exercise_content/<module>/` into `_static/<module>-exercise-files.zip` (underscores → hyphens).
- `copy_asset_folders` — copies every `_assets/` dir into the HTML output.
- `extract_glossary_from_html` — parses `glossary.html` into `_static/data/glossary-definitions.js` for the interactive glossary graph.
- A MyST-NB renderer monkey-patch adds per-cell `emphasize-lines` support; `LOUSDHTMLTranslatorMixin` rewrites `.mp4/.webm/.ogg` image references into `<video>` tags.

### Content layout (`docs/`)
Each learning module is a directory with an `index.md` toctree (e.g. `stage-setting/`, `composition-basics/`, `asset-structure/`). Exercise USD/Python files live in `docs/exercise_content/<module>/`; images in `docs/images/<module>/`; lesson-local build assets in per-lesson `_assets/` dirs. `docs/glossary.md` is the single source of truth for USD term spelling and definitions.

### Notebook test harness (`tests/conftest.py`)
The `run_notebook` fixture executes selected code cells of a built notebook in an isolated namespace inside a temp dir, returning a `Notebook` whose attributes are the cells' variables (`nb.stage`, …) with `nb._work_dir` for files written under `_assets/`.
- Select cells by `cells=[0,2]` (indices) **or** `tags=["setup", ...]` (the `test-tags` cell metadata) — never both.
- Tagged cells run in **document order**; a tag matching no cell raises `ValueError` (catches typos), an out-of-range index raises `IndexError`.
- Pass `needs_content=True` to stage `exercise_content/` so `../exercise_content/...` resolves.
- To make a lesson cell testable, add `:test-tags: [name, ...]` to its `{code-cell}` directive (see STYLEGUIDE.md → Notebook Testing).

## Conventions

- **USD terms are lowercase** unless sentence-initial ("prim", not "Prim"); link with the `{term}` role on first use; spelling comes from `docs/glossary.md`.
- **License header required** on every `.py` and every executable-code lesson `.md` — the Apache-2.0 SPDX block (in `.md` it goes inside the YAML frontmatter; in `.py` as `#` comments). Executable lessons also need Jupytext frontmatter. See STYLEGUIDE.md → Required File Headers.
- Adding a lesson/module: create the `.md`, add it to the module's `index.md` toctree, place assets/exercise files per the layout above, then build to verify. AGENTS.md has step-by-step instructions.
- Contributions require **DCO sign-off** (`git commit -s`); work against `main`. See CONTRIBUTING.md.
- New binary assets (`.png .jpg .gif .mp4 .webm .usd .usda .usdc .usdz`) are tracked by Git LFS via `.gitattributes` — ensure `git lfs install` has been run.
