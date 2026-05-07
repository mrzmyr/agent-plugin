---
name: deslop
description: Question fallbacks, regex matching, and custom code when a popular library could be used.
---

- question your usage of fallbacks (e.g. environment variables overrides)
- question your usage of pattern matching via regex
- question if a very popular library for your specific case could be used rather than adding all the code yourself (e.g. oauth)
- question your usage of type overwrites/assertions when the inferred type should be used

Bad fallback example:

```ts
function maxResults(value, fallback) {
  const n = Number(value ?? fallback);
  if (!Number.isFinite(n)) return fallback;
  return Math.max(1, Math.min(MAX_RESULTS, Math.trunc(n)));
}
```

Bad regex example:

```ts
function readOnlyQuery(query) {
  const sql = String(query).trim().replace(/;+\s*$/, "");
  if (!/^(select|with|explain)\b/i.test(sql)) throw new Error("read only only");
  if (/\b(insert|update|delete|drop|alter|create)\b/i.test(sql)) throw new Error("mutating sql");
  return sql;
}
```
