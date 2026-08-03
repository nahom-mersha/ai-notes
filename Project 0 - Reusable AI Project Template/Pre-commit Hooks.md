# Pre-commit Hooks

A pre-commit hook runs automatically before creating a Git commit.

Instead of remembering to run formatting and linting manually, Git can do it automatically.

The flow becomes:

```text
git commit
      ↓
Run formatter/linter
      ↓
Commit succeeds or fails
```

I learned that pre-commit hooks are mainly for fast local checks.

GitHub Actions is still the real quality gate.