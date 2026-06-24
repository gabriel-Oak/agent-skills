---
name: nextjs-setup
description: "Sets up a complete Next.js project with TypeScript, Vitest, Playwright, Tailwind CSS, Husky, commitlint, and GitHub Actions CI. Trigger words: criar projeto next, criar projeto nextjs, novo projeto next, nextjs starter, next project setup."
---

# Next.js Setup Skill

## When to Use

Activate this skill when the user asks to create a new Next.js project or set up a Next.js project from scratch.

## Project Stack

Always use the following stack:

| Tool | Version/Config |
|------|----------------|
| **Next.js** | Latest stable version (check `npm view next version`) |
| **TypeScript** | Enabled (default with `--typescript`) |
| **Tailwind CSS** | Enabled (default with `--tailwind`) |
| **Vitest** | For unit/component testing |
| **Playwright** | For e2e testing |
| **Husky** | For git hooks |
| **commitlint** | Conventional Commits via Husky commit-msg hook |

## Setup Procedure

### Step 1 — Create the Project

```bash
npx create-next-app@latest <project-name> \
  --typescript \
  --tailwind \
  --eslint \
  --app \
  --src-dir \
  --no-import-alias \
  --use-npm
```

Then `cd <project-name>`.

### Step 2 — Add Testing Dependencies

Install Vitest and its DOM testing library:

```bash
npm install -D vitest @testing-library/react @testing-library/jest-dom jsdom
```

Install Playwright and its browsers:

```bash
npm install -D @playwright/test
npx playwright install
```

### Step 3 — Add Husky and commitlint

```bash
npm install -D husky @commitlint/config-conventional commitlint
```

Initialize Husky:

```bash
npx husky init
```

### Step 4 — Configure Vitest

Create `vitest.config.ts` in the project root:

```ts
import { defineConfig } from 'vitest/config';
import react from '@vitejs/plugin-react';
import path from 'path';

export default defineConfig({
  plugins: [react()],
  test: {
    environment: 'jsdom',
    globals: true,
    setupFiles: ['./src/test/setup.ts'],
    alias: {
      '@': path.resolve(__dirname, './src'),
    },
  },
});
```

Create `src/test/setup.ts`:

```ts
import '@testing-library/jest-dom';
```

Add test script to `package.json` (if not already present):

```json
"test": "vitest"
```

### Step 5 — Configure Playwright

Run `npx playwright install-deps` (if on CI or a fresh machine).

Ensure `playwright.config.ts` exists (created by Playwright install). Update it to use the `@playwright/test` reporter and point to the correct test directory (`e2e/` or `tests/`).

Add e2e test script to `package.json`:

```json
"test:e2e": "playwright test"
```

### Step 6 — Configure Husky Hooks

After `husky init`, Husky creates a `pre-commit` hook. Edit `.husky/pre-commit` to run lint, unit tests, and e2e tests:

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npx lint-staged
npm run test
npm run test:e2e
```

Create `lint-staged.config.js` in the project root:

```js
export default {
  '*.{js,jsx,ts,tsx}': ['eslint --fix', 'git add'],
  '*.{json,md}': ['prettier --write', 'git add'],
};
```

Ensure `package.json` has `lint-staged` in devDependencies and a `lint` script:

```bash
npm install -D lint-staged
```

```json
"lint": "next lint",
"prepare": "husky"
```

### Step 7 — Configure Commit Lint

Edit `.husky/commit-msg` (created by `husky init`):

```bash
#!/usr/bin/env sh
. "$(dirname -- "$0")/_/husky.sh"

npx -- --no-install commitlint --edit $1
```

Create `commitlint.config.js` in the project root:

```js
export default { extends: ['@commitlint/config-conventional'] };
```

### Step 8 — Configure GitHub Actions CI

Create the directory `.github/workflows/` if it doesn't exist.

Create `.github/workflows/ci.yml`:

```yaml
name: CI

on:
  push:
    branches: [main, master]
  pull_request:
    branches: [main, master]

jobs:
  check:
    runs-on: ubuntu-latest

    strategy:
      matrix:
        node-version: [20.x, 22.x]

    steps:
      - name: Checkout
        uses: actions/checkout@v4

      - name: Setup Node.js
        uses: actions/setup-node@v4
        with:
          node-version: ${{ matrix.node-version }}
          cache: npm

      - name: Install dependencies
        run: npm ci

      - name: Lint
        run: npm run lint

      - name: Unit tests
        run: npm run test -- --run

      - name: Install Playwright browsers
        run: npx playwright install --with-deps

      - name: E2E tests
        run: npm run test:e2e -- --reporter=list
```

This CI workflow:
- Triggers on **push** to `main`/`master` and **pull requests** to `main`/`master`
- Runs on **Node.js 20.x and 22.x** (matrix strategy)
- Runs **three checks** in sequence: lint → unit tests → e2e tests
- Uses `npm ci` for deterministic installs
- Installs Playwright browsers with system dependencies via `--with-deps`

### Step 9 — Update `.gitignore`

Add the following entries to `.gitignore` (append if file exists):

```
# Plans and specs
.plan/
.specs/

# Temp
/tmp
```

### Step 10 — Verify the Setup

Run the following commands to verify everything works:

```bash
npm run lint
npm run test -- --run
npm run test:e2e -- --reporter=list
```

All must pass with zero errors. If any fail, fix them before declaring the project ready.

## Completion Criteria

The project is **ready for development** only when ALL of the following are true:

- [ ] Next.js project created with TypeScript, Tailwind CSS, ESLint, App Router
- [ ] Vitest configured with React DOM testing library and path alias `@/*`
- [ ] Playwright configured with e2e test script
- [ ] Husky pre-commit hook runs: lint-staged → unit tests → e2e tests
- [ ] Husky commit-msg hook runs commitlint with conventional commits
- [ ] `.gitignore` includes `.plan`, `.specs`, and `/tmp`
- [ ] GitHub Actions CI workflow created (`.github/workflows/ci.yml`) running lint, unit tests, and e2e tests on push and PR
- [ ] `npm run lint` passes
- [ ] `npm run test -- --run` passes
- [ ] `npm run test:e2e -- --reporter=list` passes
- [ ] `package.json` has all required scripts: `dev`, `build`, `start`, `lint`, `test`, `test:e2e`, `prepare`

## Important Notes

- **Never** skip any verification step. The project is not ready until all checks pass.
- **Never** disable or bypass Husky hooks.
- Always use the **latest** Next.js version — check `npm view next version` before creating the project.
- Use `--no-import-alias` during scaffolding and set up path aliases manually if needed (the setup above configures `@/*` → `./src/*` in Vitest).
- All configuration files go in the **project root** unless otherwise specified.
- Test setup files go in `src/test/`.
