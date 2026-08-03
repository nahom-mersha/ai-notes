# Type Hinting, Type Checking & Data Validation

## Type Hinting

Type hints describe what type a variable or function expects.

Example:

```python
def calculate_mean(values: list[float]) -> float:
```

Type hints mainly help developers, IDEs and analysis tools.

They do **not** stop the program from running.

---

## Type Checking

Type checking uses tools to verify that my code follows the type hints.

If I pass the wrong type, I get warnings or errors during development.

The program itself can still run.

---

## Data Validation

Data validation actually checks the data while the program is running.

If invalid data is received, the program can raise an exception and stop.