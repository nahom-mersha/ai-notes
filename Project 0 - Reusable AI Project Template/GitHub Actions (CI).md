# GitHub Actions (CI)

GitHub Actions automatically checks my project after I push it to GitHub.

The workflow is:

```text
Push
    ↓
Create clean environment
    ↓
Install dependencies
    ↓
Run Ruff
    ↓
Run pytest
    ↓
Pass or fail
```

The important lesson:

> Never automate a command that I cannot run successfully myself.

First make it work locally.

Then let GitHub automate it.