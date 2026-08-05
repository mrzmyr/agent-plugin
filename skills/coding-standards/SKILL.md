---
name: coding-standards
description: Use when writing, reviewing, or refactoring TypeScript
---

# Coding Standards

## Principles

- **No backward compatibility.** Remove obsolete paths instead of adding compatibility layers, fallbacks, or migrations.
- **Simplest thing that fully works.** Choose the simplest implementation that meets the current requirements. Avoid speculative abstractions, configuration, and indirection.
- **Grow in layers.** Start with the smallest version that works end to end, and add each capability on top of something that already works. Never trade a working product for unfinished complexity.
- **Modular by default.** Keep components separate and concerns clearly divided.
- **Use existing dependencies first.** Reach for what the project already has before writing your own implementation or adding a package. Do not assume a library lacks a capability without checking its docs and types.
- **Prefer established libraries.** Use well-maintained libraries when they reduce complexity or improve reliability. Do not reimplement common functionality without a clear reason.
- **Decide for the long term.** Do not accept a stopgap that only works for now and is meant to be replaced later.

## Function Calls

- Prefer named parameters over positional parameters.
- Use an object argument when a function takes multiple values, optional values, or values whose meaning is not obvious from type alone.

✅ Use

```ts
getUsers({ pageSize, pageIndex });

const tool = bareToolName({
  toolName: ctx.toolName,
  prefix: prefix,
});
```

❌ Avoid

```ts
getUsers(pageSize, pageIndex);

bareToolName(ctx.toolName, prefix);
```

## Branch Bodies

Never use shorthand if/else. Always use a block body. The same applies to ternaries that stand in for a branch.

✅ Use

```ts
if (audience === getChatScope(ctx)) {
  return "not-applicable";
}
```

❌ Avoid

```ts
if (audience === getChatScope(ctx)) return "not-applicable";

const tool = ctx.toolName.startsWith(prefix)
  ? ctx.toolName.slice(prefix.length)
  : ctx.toolName;
```

## Error Logging

Every real error path must use the project's structured error helper. This includes thrown errors, returned HTTP/OAuth/JSON errors, and error logs.

Structured errors must include:

- `status`
- `message`
- `why`
- `fix`

The fields must be specific enough for an operator or developer to understand what happened, why it happened, and what action resolves it.

✅ Use

```ts
throw createError({
  status: 502,
  message: "Auth0 metadata request failed",
  why: `Auth0 returned status ${response.status} while loading OAuth metadata`,
  fix: "Verify Auth0 is reachable and the issuer configuration is correct",
});
```

❌ Avoid

```ts
throw new Error(`Auth0 metadata request failed with ${response.status}`);
```

## Boolean Names

Boolean-returning functions must be prefixed with a boolean-style verb.

Use prefixes like:

- `is`
- `has`
- `can`
- `should`
- `was`
- `will`

✅ Use

```ts
function isAllowed({ userId }: { userId: string }): boolean {
  return userId.length > 0;
}
```

❌ Avoid ambiguous boolean names

```ts
function allowed(userId: string): boolean {
  return userId.length > 0;
}
```

## Function Names

Function names should be short and start with an action verb. Prefer `verb + subject`.

Use verbs like:

- `get`
- `set`
- `create`
- `update`
- `delete`
- `fetch`
- `load`
- `parse`
- `format`
- `increase`
- `decrease`

✅ Use

```ts
getAttrs();
increaseRetries();
formatName();
```

❌ Avoid noun-only or abbreviated names when the function performs an action

```ts
baseAttr();
retryCount();
displayName();
```

✅ Use one-word function names when the word is the complete, intentional API and reads clearly at the call site, especially for small DSL-style helpers or result builders

```ts
success();
error();
text();
```

## Local File Name Context

Use the file name as naming context. Do not repeat the domain already supplied by the file path or file name unless it removes ambiguity at the call site.

✅ Use

```ts
// data-analysis.ts
export function getSandbox() {
  // ...
}
```

❌ Avoid

```ts
// data-analysis.ts
export function getDataAnalysisSandbox() {
  // ...
}
```

- Prefer shorter local names when the module boundary already supplies the noun
- Use longer names only when exported APIs are commonly imported into contexts where the shorter name would be unclear

## Fallbacks

- Question your usage of fallbacks (e.g. environment variables overrides).

✅ Use

```ts
const EVAL_USER_EMAIL = process.env.EVAL_USER_EMAIL;

if (!EVAL_USER_EMAIL) {
  throw new Error("EVAL_USER_EMAIL is required");
}
```

❌ Avoid

```ts
const EVAL_USER_EMAIL =
  process.env.EVAL_USER_EMAIL ??
  process.env.BIGQUERY_EVAL_USER_EMAIL ??
  "first.lastname@gmail.com";
```

## Regex Pattern Matching

- Question your usage of pattern matching via regex.

✅ Use

```ts
async function readOnlyQuery({ bigquery, query }) {
  const [job] = await bigquery.createQueryJob({ query, dryRun: true, useLegacySql: false });

  if (job.metadata.statistics?.query?.statementType !== "SELECT") {
    // throw structured error
  }

  return query;
}
```

❌ Avoid

```ts
function readOnlyQuery(query) {
  const sql = String(query).trim().replace(/;+\s*$/, "");
  if (!/^(select|with|explain)\b/i.test(sql)) throw new Error("read only only");
  if (/\b(insert|update|delete|drop|alter|create)\b/i.test(sql)) throw new Error("mutating sql");
  return sql;
}
```