---
type: procedure
created: 2026-03-20
status: active
slug: ccs-010-migrate-to-shoehorn
tags: [procedure, nexus]
---

# CCS-010 — Migrate to Shoehorn (Test Data Assertion Migration)

## Problema
TypeScript test files frequently use `as Type` and `as unknown as Type` assertions to pass partial or intentionally wrong data to functions. These assertions are trained against in production code, require manually specifying target types, and produce double-`as` constructs for intentionally incorrect data. The `@total-typescript/shoehorn` library provides type-safe alternatives that work exclusively in test code, eliminating unsafe assertions while keeping TypeScript satisfied.

## Procedura

### Prerequisites
- TypeScript project with test files using `as` assertions
- Node.js project with npm/pnpm/yarn/bun package manager
- Test files identified (`.test.ts`, `.spec.ts`)

### Steps

1. **Gather requirements.** Ask the user: What test files have `as` assertions causing problems? Are they dealing with large objects where only some properties matter (candidate for `fromPartial`)? Do they need to pass intentionally wrong data for error testing (candidate for `fromAny`)? Do they want to enforce full objects in some tests (candidate for `fromExact`)?

2. **Install the package.** Run the install command for the detected package manager: `npm i @total-typescript/shoehorn` (or pnpm/yarn/bun equivalent). Install as a dev dependency since shoehorn is test-code only.

3. **Understand the three replacement functions.** `fromPartial()`: pass partial data that still type-checks — replaces `{ ...partialObj } as FullType`; `fromAny()`: pass intentionally wrong data while keeping autocomplete — replaces `{ ...wrongData } as unknown as CorrectType`; `fromExact()`: force a full object with all required properties — useful when you want to validate the test data is complete.

4. **Never use shoehorn in production code.** This is a hard rule. If any non-test file is using shoehorn imports, flag it as a bug. The library is designed exclusively for test scenarios where partial data is acceptable.

5. **Find all `as` assertions in test files.** Search with: `grep -r " as [A-Z]" --include="*.test.ts" --include="*.spec.ts"` to find `as Type` patterns. Search with: `grep -r "as unknown as" --include="*.test.ts" --include="*.spec.ts"` to find double-`as` patterns.

6. **Apply the `as Type` → `fromPartial()` migration.** Replace patterns like `{ body: { id: "123" } } as Request` with `fromPartial({ body: { id: "123" } })`. This is used when the test only cares about a subset of the full object's properties. Add import: `import { fromPartial } from "@total-typescript/shoehorn"`.

7. **Apply the `as unknown as Type` → `fromAny()` migration.** Replace patterns like `{ body: { id: 123 } } as unknown as Request` (where `id` is intentionally the wrong type) with `fromAny({ body: { id: 123 } })`. This preserves autocomplete while allowing type violations. Add import: `import { fromAny } from "@total-typescript/shoehorn"`.

8. **Handle large objects with many required properties.** The primary benefit of `fromPartial` is for large types where only 1-3 properties matter for the test. Instead of faking 20 properties, pass only the properties the test actually exercises. This makes test intent clearer and reduces test maintenance when the type gains new required fields.

9. **Batch migrate by file.** Process one test file at a time to keep changes reviewable. After each file, run the type checker to verify no new type errors were introduced.

10. **Run type check after migration.** Execute `tsc --noEmit` (or the project's typecheck command) to verify all shoehorn replacements are correct. Fix any type errors before proceeding to the next file.

11. **Remove unused `as` imports if they were added for type-casting.** After migration, some files may have type imports that were only needed for the `as Type` syntax. Clean those up.

12. **Verify tests still pass.** Run the test suite to confirm behavior is unchanged. Shoehorn is a type-system change only — runtime behavior is unaffected, but verify anyway.

### Verification
- [ ] `@total-typescript/shoehorn` installed as dev dependency
- [ ] No shoehorn imports in production (non-test) files
- [ ] All `as Type` replaced with `fromPartial()` where appropriate
- [ ] All `as unknown as Type` replaced with `fromAny()` where appropriate
- [ ] Type check passes after migration
- [ ] Test suite passes after migration

## Cortex Logging
- collection: procedures
- tags: claude-code-skills, training, typescript, testing, shoehorn, test-data, type-assertions, v2

## Enforcement Loop
- WHERE: TypeScript test files with `as` type assertions
- WHEN: User mentions "shoehorn", "replace `as` in tests", "partial test data", or shows `as unknown as` patterns
- HOW: Always install first, then migrate by file, type-check after each file
- CONNECT: CCS-007 (TDD — good test data setup), CCS-011 (pre-commit hooks catch type errors)
