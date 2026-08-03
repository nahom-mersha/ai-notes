# Repository Structure (src layout)

## Why use the `src` layout?

Instead of putting my source code directly in the project root, I place it inside:

```text
src/
    ai_template/
```

This makes the project behave more like an installed package.

It helps avoid import problems and reduces the chance that Python accidentally imports files directly from the project folder instead of the installed package.

---

## `__init__.py`

Adding

```python
__init__.py
```

marks the folder as a Python package.

Modern Python can sometimes work without it, but it is still considered good practice.