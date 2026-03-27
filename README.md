# FAIR-Maturity-Matrix

## Initial setup

Create the local Python virtual environment:

```sh
ksh -c 'python -m venv .venv'
```

Activate the virtual environment:

```sh
ksh -c '. .venv/bin/activate'
```

Install the project dependencies:

```sh
ksh -c '. .venv/bin/activate && pip install -U pip && pip install -e .'
```

## Run MkDocs

The repository keeps the canonical GitHub Pages URL in `mkdocs.yml`:

```yaml
site_url: https://chris-day.github.io/FAIR-Maturity-Matrix/
```

For local development, keep the GitHub Pages `site_url` in the config and expose the dev server on all interfaces.

Start the local MkDocs development server:

```sh
ksh -c '. .venv/bin/activate && mkdocs serve --dev-addr 0.0.0.0:8000'
```

Build the static site:

```sh
ksh -c '. .venv/bin/activate && mkdocs build'
```
