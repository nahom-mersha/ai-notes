# SQL for Rental Data Analysis

## Overview

The project uses SQLite to practise retrieving and summarizing rental data with SQL before returning results to Pandas.

This step is not intended to improve the model directly. It demonstrates a common production workflow:

```text
Database
→ SQL filtering and aggregation
→ Pandas
→ preprocessing and modelling
```

## SQL and Pandas

SQL and Pandas can answer many of the same questions using different syntax.

| Question | Pandas idea | SQL idea |
| --- | --- | --- |
| Select columns | `df[[...]]` | `SELECT ...` |
| Filter rows | boolean mask | `WHERE` |
| Count rows | `len(df)` | `COUNT(*)` |
| Average a column | `.mean()` | `AVG()` |
| Count categories | `.value_counts()` | `GROUP BY` and `COUNT(*)` |
| Find missing values | `.isna()` | `IS NULL` |

## SQLite workflow

The raw CSV is loaded into a local SQLite table named `rentals`. The database can then filter Munich listings and calculate statistics without first loading the entire table into a modelling pipeline.

For example:

```sql
SELECT
    COUNT(*) AS listing_count,
    AVG(baseRent) AS average_rent,
    MIN(baseRent) AS minimum_rent,
    MAX(baseRent) AS maximum_rent
FROM rentals
WHERE regio2 = 'München';
```

The raw database contained 4,383 Munich listings before the machine-learning cleaning rules were applied.

## Calculated categories

A `CASE` expression can create rent bands directly inside a query:

```sql
SELECT
    CASE
        WHEN baseRent < 1000 THEN 'under_1000'
        WHEN baseRent < 2000 THEN '1000_to_1999'
        WHEN baseRent < 3000 THEN '2000_to_2999'
        ELSE '3000_plus'
    END AS rent_band,
    COUNT(*) AS listing_count
FROM rentals
WHERE regio2 = 'München'
GROUP BY rent_band;
```

This combines four ideas:

```text
WHERE
→ select relevant rows

CASE
→ create a calculated category

GROUP BY
→ form one group per category

COUNT(*)
→ count rows in each group
```

## Missingness and categories

Conditional aggregation can count missing construction year, floor, interior quality, and condition values. `GROUP BY` can also show that `apartment` is the most common flat type and that many listings have a missing flat type.

These queries help inspect data availability before feature selection and preprocessing.

## Key takeaway

SQL is often used to retrieve, filter, and aggregate data where it is stored. Pandas then supports Python-side exploration, preprocessing, and integration with machine-learning libraries.

## Related notes

- [Rental Prediction Problem and Data Preparation](Rental%20Prediction%20Problem%20and%20Data%20Preparation.md)
- [Leakage-Safe Preprocessing and Scikit-Learn Pipelines](Leakage-Safe%20Preprocessing%20and%20Scikit-Learn%20Pipelines.md)