# Python Packaging (pyproject.toml)

## Why do we need `pyproject.toml`?

`pyproject.toml` is the configuration file that describes the Python project.

Instead of manually installing files, Python knows:

- the project name
- the Python version
- dependencies
- optional development dependencies

---

## What I learned

This file became the central place for configuring my project.

For example, I used it to define the development dependencies:

```toml
[project.optional-dependencies]
dev = [
    "pytest",
    "ruff",
]
```

This allowed me to install everything with:

```bash
pip install -e ".[dev]"
```