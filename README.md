# Graph category identity screenshots

Screenshots for apache/superset#44790, using the same saved Graph chart, synthetic PostgreSQL dataset, circular layout, and 1600 x 1050 viewport.

- Before: previous PR revision `54b99d65ffc1d523820a582e5a80fd2b6e03c2aa`. SQL NULL and literal `<NULL>` share their category and color.
- After: `97f44ac1e1d324dc55c47a540ea8c0ece6e6eb81`. SQL NULL, literal `N/A`, and literal `<NULL>` have independent category identities and colors. The literal `<NULL>` label is quoted.

```sql
SELECT 'Source: SQL NULL' AS source, 'Target: SQL NULL' AS target, CAST(NULL AS VARCHAR) AS category, 10 AS weight UNION ALL SELECT 'Source: literal N/A', 'Target: literal N/A', 'N/A', 20 UNION ALL SELECT 'Source: literal <NULL>', 'Target: literal <NULL>', '<NULL>', 30
```

Chart settings: `source` and `target` node columns, `category` for both category columns, `SUM(weight)` metric, Superset color scheme, plain legend, circular layout.

Browser checks confirmed six nodes and unchanged values in both captures; two categories before and three after; independent legend toggling of SQL NULL and literal `<NULL>` after; no page errors.

Validation used a Docker backend without Flask's reloader and a Windows frontend development build. Both builds used the same local Webpack `fullySpecified: false` override for installed geostyler dependencies. This environment override is outside the PR.
