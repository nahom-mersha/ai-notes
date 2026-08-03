# Unit Testing with pytest

## What is a unit test?

A unit test checks one small part (one "unit") of the program, usually one function.

Each test should be independent.

---

## Why use pytest?

Instead of manually checking my functions every time, pytest runs all my tests automatically.

I compare:

- expected value
- actual returned value

using:

```python
assert
```

If they match, the test passes.

If not, pytest reports the failure.