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

Start the local MkDocs development server:

```sh
ksh -c '. .venv/bin/activate && mkdocs serve'
```

Build the static site:

```sh
ksh -c '. .venv/bin/activate && mkdocs build'
```
