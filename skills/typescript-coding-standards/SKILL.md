---
name: typescript-coding-standards
description: Use when writing, reviewing, or refactoring TypeScript code to apply comprehensive coding standards for typed errors, parsing, domain modeling, modules, adapters, tests, safety, imports, comments, and configuration.
---

# TypeScript Coding Standards

Apply comprehensive TypeScript coding standards from `references/coding-standards.md`.

## Required Reference

Read `references/coding-standards.md` before applying this skill to TypeScript code, designs, architecture, or reviews.

Use the reference to guide:

- typed errors and expected failures as values
- parse-don't-validate boundaries
- branded/refined/domain types
- state machines and boolean blindness
- deep modules, domain modules, services, adapters, and repositories
- functional core / imperative shell boundaries
- workflows, transactions, and idempotency
- confidence-oriented tests through real seams
- TypeScript safety, imports, exports, comments, config, and resources

## Application

- Inspect existing project conventions before introducing patterns.
- Prefer local architecture unless it conflicts with correctness, safety, or debuggability.
- Apply standards to new code paths without forcing broad migrations.
- Translate typed errors and refined values at framework or legacy boundaries.
- Add ADRs only for meaningful new adapters/services after an adapter reuse audit.
- Preserve safe observability; never log secrets or raw sensitive values.
