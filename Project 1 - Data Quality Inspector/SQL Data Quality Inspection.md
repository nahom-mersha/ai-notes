# SQL Data Quality Inspection

## Overview

The project includes SQLite examples that ask data-quality questions with SQL.

This matters because real-world data often lives in databases rather than CSV files.

## Same questions, different tools

| Question | Pandas idea | SQL idea |
| --- | --- | --- |
| How many rows? | `len(dataframe)` | `COUNT(*)` |
| Missing values? | `isna()` | `IS NULL` |
| Duplicates? | `duplicated()` | `GROUP BY ... HAVING COUNT(*) > 1` |
| Class distribution? | `value_counts()` | `GROUP BY target_column` |
| Category consistency? | lowercase comparison | `GROUP BY LOWER(column)` |

## Example: rows by city

```sql
SELECT city, COUNT(*) AS row_count
FROM customers
GROUP BY city;
```

This groups rows with the same city value, then counts the rows in each group.

## Example: duplicate combinations

```sql
SELECT name, city, COUNT(*) AS duplicate_count
FROM customers
GROUP BY name, city
HAVING COUNT(*) > 1;
```

`HAVING` filters groups after `COUNT(*)` is calculated.

## Example: missing values

```sql
SELECT *
FROM customers
WHERE age IS NULL
   OR age = '';
```

`IS NULL` checks database missing values. The empty-string check is useful because CSV imports can represent missing data differently.

## Key takeaway

Pandas and SQL use different syntax, but they support many of the same data-inspection questions.

## Related notes

- [Data Quality Checks and Validation](Data%20Quality%20Checks%20and%20Validation.md)
- [Data Quality Reports](Data%20Quality%20Reports.md)