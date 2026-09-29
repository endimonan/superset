# Cartodiagram category colors in Explore

Before/after captures use the same saved Cartodiagram, dataset, Pie settings, viewport (1600 × 1050), and map extent.

- Before: upstream `ce86ac9aefcbf4fa4448909f9c9fa818206bdf4a`.
- After: `fix/cartodiagram-colors`.
- Expected: four distinct categories use four distinct colors across two locations. Repeated categories retain their color within the map.

| Category | Before | After |
| --- | --- | --- |
| far | `#1FA8C9` | `#1FA8C9` |
| faz | `#454E7C` | `#454E7C` |
| baz | `#1FA8C9` | `#5AC189` |
| bar | `#454E7C` | `#FF7F44` |

Both runs returned four rows with HTTP 200 and no browser JavaScript errors. The captures show a pre-existing Pie clipping issue and no basemap tiles; the comparison validates category colors in the legends, not map layout.

## Reproduction

Create a virtual dataset using the issue's two Point geometries:

```sql
SELECT '{"coordinates":[10.021094598147045,51.12881242291189],"type":"Point"}' AS geom, 1 AS foo, 'bar' AS cat
UNION ALL
SELECT '{"coordinates":[10.021094598147045,51.12881242291189],"type":"Point"}', 1, 'baz'
UNION ALL
SELECT '{"coordinates":[9.573277401918602,50.77733656456951],"type":"Point"}', 1, 'far'
UNION ALL
SELECT '{"coordinates":[9.573277401918602,50.77733656456951],"type":"Point"}', 1, 'faz'
```

Save a Pie chart grouped by `cat`, with metric `SUM(foo)`, the Superset palette, and its legend enabled. Create a Cartodiagram using this dataset, select the saved Pie chart, and set the geometry column to `geom`. Open it in Explore and compare category colors across locations.
