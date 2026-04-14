---
type: procedure
created: 2026-03-20
status: active
slug: ccs-011-setup-pre-commit
tags: [procedure, nexus]
---

# CCS-011 — Setup Pre-Commit Hooks (Husky + lint-staged)

## Problema
Without automated pre-commit checks, developers push code that fails type checking, breaks tests, or has inconsistent formatting. Manual discipline is insufficient — enforcement must be automated at commit time. Husky with lint-staged provides the standard Node.js pre-commit stack: Prettier for formatting (staged files only, fast), type checking (full codebase), and tests (full suite) — in that order to fail fast on the cheapest checks first.

## Procedura

### Prerequisites
- Node.js project with `package.json`
- One of: npm (`package-lock.json`), pnpm (`pnpm-lock.yaml`), yarn (`yarn.lock`), or bun (`bun.lockb`)
- `typecheck` and `test` scripts in `package.json` (optional but recommended)

### Steps

1. **Detect the package manager.** Check for lockfiles: `package-lock.json` → npm; `pnpm-lock.yaml` → pnpm; `yarn.lock` → yarn; `bun.lockb` → bun. Default to npm if no lockfile is found or the situation is ambiguous. Use the detected package manager for all subsequent commands.

2. **Install dev dependencies.** Install `husky`, `lint-staged`, and `prettier` as dev dependencies in a single command. Example for npm: `npm install --save-dev husky lint-staged prettier`.

3. **Initialize Husky.** Run `npx husky init`. This creates the `.husky/` directory and automatically adds `"prepare": "husky"` to `package.json`. Verify both effects occurred.

4. **Create `.husky/pre-commit` file.** Write the hook file with three commands in order: `npx lint-staged` (fast, staged files only), `npm run typecheck` (or detected package manager equivalent), `npm run test` (or detected package manager equivalent). Husky v9+ does not need a shebang line at the top of hook files.

5. **Adapt for detected package manager.** Replace `npm run` with `pnpm run`, `yarn`, or `bun run` depending on what was detected. Consistency with the project's package manager prevents confusion.

6. **Handle missing scripts gracefully.** Check `package.json` for `typecheck` and `test` scripts before including them in the hook. If either is missing, omit that line from the pre-commit hook and inform the user that the corresponding check was skipped. Do not add a script that doesn't exist — it will cause every commit to fail.

7. **Create `.lintstagedrc`.** Write the lint-staged configuration file with a single rule: `{ "*": "prettier --ignore-unknown --write" }`. The `--ignore-unknown` flag makes Prettier skip files it cannot parse (images, binaries, etc.) instead of erroring. The `--write` flag formats in place. Staging globs to `"*"` formats all staged files regardless of extension.

8. **Create `.prettierrc` only if missing.** Check for any existing Prettier config (`.prettierrc`, `.prettierrc.json`, `.prettierrc.js`, `prettier.config.js`, or `prettier` key in `package.json`). If none exists, create `.prettierrc` with the defaults: `useTabs: false`, `tabWidth: 2`, `printWidth: 80`, `singleQuote: false`, `trailingComma: "es5"`, `semi: true`, `arrowParens: "always"`. If a Prettier config already exists, do not overwrite it.

9. **Verify the hook file is executable.** Check that `.husky/pre-commit` has execute permissions. If `npx husky init` didn't set them, run `chmod +x .husky/pre-commit`.

10. **Run `npx lint-staged` as a smoke test.** Execute lint-staged manually to confirm it runs without errors before making an actual commit. This catches configuration issues before the first real use.

11. **Stage and commit the new configuration files.** Stage all changed/created files (`.husky/pre-commit`, `.lintstagedrc`, `.prettierrc` if created, and `package.json` changes) and commit with the message: `Add pre-commit hooks (husky + lint-staged + prettier)`. This commit itself runs through the new hooks — a built-in smoke test.

12. **Inform the user of what was set up and what was skipped.** After completion, report which checks are now active (lint-staged/prettier always, typecheck if found, test if found) and which were skipped with instructions for adding them later.

### Verification
- [ ] `.husky/pre-commit` exists and is executable
- [ ] `.lintstagedrc` exists with `prettier --ignore-unknown --write` rule
- [ ] `prepare: "husky"` script present in `package.json`
- [ ] Prettier config exists (created or pre-existing)
- [ ] `npx lint-staged` runs cleanly before the commit test
- [ ] First commit with new hooks succeeds (runs all active checks)

## Cortex Logging
- collection: procedures
- tags: claude-code-skills, training, husky, lint-staged, prettier, pre-commit, tooling, v2

## Enforcement Loop
- WHERE: Node.js repositories without pre-commit hooks
- WHEN: User says "add pre-commit hooks", "set up Husky", "configure lint-staged", "add commit-time formatting"
- HOW: Always detect package manager first; always check for existing scripts before adding them to the hook
- CONNECT: CCS-012 (git-guardrails add safety on top of pre-commit), CCS-007 (TDD — tests run in pre-commit)
