# Graph null category screenshots

Evidence for the Graph category-label fix related to [apache/superset#43547](https://github.com/apache/superset/issues/43547).

Both captures use the same saved chart, synthetic three-row dataset, circular layout, and 1600x1050 viewport.

- Before: SQL NULL and literal N/A nodes share the N/A category and color.
- After: SQL NULL nodes use the separate `<NULL>` category and color; literal N/A remains unchanged.

The screenshots are stored separately from the code contribution.

```sql
SELECT 'Source: SQL NULL' AS source, 'Target: SQL NULL' AS target,
CAST(NULL AS VARCHAR) AS source_category,
CAST(NULL AS VARCHAR) AS target_category, 10 AS weight
UNION ALL
SELECT 'Source: literal N/A', 'Target: literal N/A', 'N/A', 'N/A', 20
UNION ALL
SELECT 'Source: alpha', 'Target: alpha', 'alpha', 'alpha', 30
```
