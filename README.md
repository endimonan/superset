# Graph category colors and quoted labels

Evidence for apache/superset#44790.

- Before: `97f44ac1e1d324dc55c47a540ea8c0ece6e6eb81`.
- After: `726f784285b41c62d91c37fcaa95f32b5f2abfac`.
- Same saved dashboard, two charts, synthetic PostgreSQL datasets, circular layout, Superset color scheme, and 1600 x 1360 viewport.

Both charts use `source` and `target` node columns, `category` for both category columns, and `SUM(weight)` as the metric.

Dashboard JSON metadata includes:

```json
{"color_scheme":"supersetColors","label_colors":{"__superset_null__":"#E53935"}}
```

## Custom color: __superset_null__ = red

```sql
SELECT 'SQL NULL source' AS source, 'SQL NULL target' AS target, CAST(NULL AS VARCHAR) AS category, 1 AS weight UNION ALL SELECT 'Reserved literal source' AS source, 'Reserved literal target' AS target, '__superset_null__' AS category, 2 AS weight UNION ALL SELECT 'N/A source' AS source, 'N/A target' AS target, 'N/A' AS category, 3 AS weight
```

## Legend labels: <NULL> and "<NULL>"

```sql
SELECT 'SQL NULL source' AS source, 'SQL NULL target' AS target, CAST(NULL AS VARCHAR) AS category, 1 AS weight UNION ALL SELECT 'Literal <NULL> source' AS source, 'Literal <NULL> target' AS target, '<NULL>' AS category, 2 AS weight UNION ALL SELECT 'Quoted <NULL> source' AS source, 'Quoted <NULL> target' AS target, '"<NULL>"' AS category, 3 AS weight
```

Before, the reserved literal loses its configured red color and SQL NULL receives it; the second chart has two identical displayed labels. After, red belongs to the literal and the quoted labels are distinguishable.

Browser verification covers all six legend items independently and confirms unchanged node names, values and category identities, with no page errors. The automatic null color is allocated per chart and may change as filters change the visible categories. Literal custom and saved color mappings are preserved.

Validation: 35 Graph Jest tests; pre-commit, including lint and targeted TypeScript checks; two independent code/test reviews. Eight new regression cases failed before the fixes.

Both builds used the same local Webpack `fullySpecified: false` override for installed geostyler dependencies, a Windows development build, and a Docker backend without Flask's reloader. The local build override is outside the PR.
