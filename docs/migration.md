# Migration To MkDocs Material

This document records the changes made to migrate this repository from the previous GitHub Pages / Jekyll layout to MkDocs with the Material theme.

## Scope

The migration changed both the documentation structure and the local documentation toolchain.

The main goals were:

- replace the old Jekyll-style publishing layout with MkDocs Material
- move site source content into `docs/`
- make internal links local and MkDocs-friendly
- install and pin the Python packages needed for the new documentation build
- resolve build and serve warnings encountered during the migration

## Initial Environment Setup

The migration also established a standard local setup for working on the documentation site.

### Python Virtual Environment

The repository uses a local virtual environment in `.venv`.

Typical setup and usage:

```sh
ksh -c 'python -m venv .venv'
ksh -c '. .venv/bin/activate && pip install -U pip'
ksh -c '. .venv/bin/activate && pip install -e .'
```

Once the environment is active, the MkDocs commands are run from the virtual environment:

```sh
ksh -c '. .venv/bin/activate && mkdocs serve'
ksh -c '. .venv/bin/activate && mkdocs build'
```

The migration work was done using `.venv/bin/pip` and `.venv/bin/mkdocs` directly to keep execution explicit and local to the repository.

### Environment Compatibility Notes

The local environment was adjusted to avoid MkDocs and plugin warnings during development.

This included:

- pinning `mkdocs` to `1.6.1`
- setting `NO_MKDOCS_2_WARNING=1` in the virtual environment activation script
- aligning `chardet` in `.venv` to avoid the `requests` dependency warning triggered by `mkdocs-macros-plugin`

### Git Ignore Expectations

The migration assumes generated and local-only Python artifacts are not committed.

Relevant `.gitignore` entries now cover:

- `.venv/`
- `/site`
- Python cache directories such as `__pycache__/`
- common build and packaging output

This keeps the MkDocs output and local virtual environment out of version control while allowing the source content, configuration, and support scripts to be tracked.

## Repository Structure Changes

The documentation source was moved into the `docs/` directory so MkDocs can build the site from a standard layout.

This included moving:

- top-level markdown pages such as `index.md`, `summary.md`, `introduction.md`, `FAIRMaturityMatrix.md`, `FMMdimensions.md`, `FMMlevels.md`, `level0.md` to `level5.md`, `contributors.md`, `resources_lexicon.md`, `termsofuse.md`, and `FAIR_training.md`
- personas from `FAIR_personas/` to `docs/FAIR_personas/`
- roles from `FAIR_roles/` to `docs/FAIR_roles/`
- assets and supporting content from `assets/`, `images/`, `videos/`, `Data-Viz/`, and `Pistoia-Alliance-style/` into `docs/`

## MkDocs Configuration

A new `mkdocs.yml` file was added to replace the previous Jekyll-oriented setup.

The current configuration includes:

- `docs_dir: docs`
- Material theme
- explicit navigation for the main pages
- explicit navigation for all persona pages
- search plugin
- minify plugin
- network graph plugin
- macros plugin
- Material navigation features such as sections, tabs, sticky tabs, and top navigation
- extra CSS and JavaScript support for site customizations and MathJax

## Python Packaging And Dependencies

A new `pyproject.toml` file was created for the project.

Dependencies added for the MkDocs-based site include:

- `mkdocs==1.6.1`
- `mkdocs-material[imaging]`
- `mkdocs-macros-plugin`
- `mkdocs-plantuml-local`
- `mkdocs-network-graph-plugin`
- `mkdocs-git-revision-date-localized-plugin`
- `mkdocs-minify-plugin`
- `rdflib`
- `pyyaml`
- `linkml`

MkDocs was pinned to `1.6.1` to stay on the latest stable 1.x release and avoid MkDocs 2.x incompatibility concerns.

## Content And Navigation Updates

The navigation was expanded and aligned with the migrated content:

- all main pages were added to the nav
- all persona pages were added explicitly to the nav
- supporting pages such as image, video, data visualization, and style references were exposed under the `Various` section

The persona index file [`docs/FAIR_personas/file_list.md`](FAIR_personas/file_list.md) was also rewritten from a plain filename list into readable local links such as:

- `[Data Engineer](FAIR_personas/Data_Engineer.md)`
- `[Business Leader](FAIR_personas/Business_Leader.md)`

## Link Migration And Cleanup

A large number of internal links were converted from absolute GitHub Pages URLs to local relative Markdown links.

This included:

- replacing links like `https://pistoiaalliance.github.io/FAIRMaturityMatrix/termsofuse` with `termsofuse.md`
- converting internal navigation links on main pages to relative `.md` links
- converting valid persona-to-persona links from absolute URLs to local relative links
- keeping only a small number of unresolved persona links in place where no matching local file currently exists

The home page table of contents in [`docs/index.md`](index.md) was updated to use relative local links instead of deployed site URLs.

## Anchor Fixes

Several broken anchor references were corrected during the migration.

Examples include:

- fixing `fairprocesses` links to use the generated MkDocs anchor form `fair-processes`
- updating `FAIRMaturityMatrix.md` links so they match actual section anchors in `FMMdimensions.md` and `level0.md` to `level5.md`
- fixing glossary and table-of-contents anchors where generated IDs differed from the original hand-written links

## Asset Updates

Image and asset references were updated after the move into `docs/`.

This included renaming references from:

- `PistoiaAlliance_main_white_RGB v2.png`

to:

- `PistoiaAlliance_main_white_RGBv2.png`

and updating Markdown references accordingly.

## Build And Environment Compatibility Fixes

During the migration, several local environment issues were addressed so `mkdocs build` and `mkdocs serve` work cleanly in the repository virtual environment.

These changes included:

- using the project `.venv`
- adding `NO_MKDOCS_2_WARNING=1` to the virtual environment activation script
- disabling parallel processing for the git revision date plugin when it conflicted with the sandboxed environment
- fixing a `requests` dependency warning by aligning `chardet` to a compatible version in `.venv`

## Current Result

The repository now builds as an MkDocs Material site from `docs/`.

The migration covers:

- site structure
- package management
- navigation
- internal links
- asset references
- compatibility fixes required for local MkDocs usage

The repository still contains a few unresolved persona links that point to names without matching local persona files. Those were intentionally left untouched until the correct target pages are defined.
