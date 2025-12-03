Ghost — AI Agent Quick Reference

Essential: Yarn v1 + Nx monorepo. Always run `yarn` then `yarn setup` for first-time work.

Quick commands
- `yarn dev` — host-only development
- `yarn dev:forward` — hybrid Docker (backend) + host frontends
- `yarn build` / `yarn build:clean`
- `yarn test:unit` / `yarn test:e2e` / `yarn lint`

High-level architecture (what matters)
- Backend: `ghost/core/core/server/` (models → services → API routes)
- Admin shell: `ghost/admin` (Ember) loads React micro-frontends via `ghost/admin/lib/asset-delivery`
- Apps: `apps/*` (React Vite apps). Admin apps use `admin-x-framework` and `shade`.
- Public UMD apps (portal, comments) are loaded via themes and data attributes.
- i18n: `ghost/i18n/locales/{locale}/{namespace}.json`.

Development patterns to follow
- Avoid modifying built artifacts; edit sources in `apps/*` and `ghost/*` packages.
- For admin apps, add UI in `apps/admin-x-*` and export via `apps/*/dist` → `ghost/admin` asset-delivery.
- Use `shade` for new components. Components: `apps/shade/src/components/*`, export via `src/index.ts` and add stories (`*.stories.tsx`).
- Use `admin-x-framework` hooks for API access & caching.

E2E & testing specific rules
- Tests: Playwright tests live under `e2e/` or `ghost/core` tests for core. Page objects: `helpers/pages/`.
- Follow AAA pattern (Arrange/Act/Assert) and factories (`e2e/data-factory`).
- Locators: prefer semantic (`getByRole`, `getByLabel`, `getByText`). `data-testid` is acceptable as fallback; never use CSS/XPath/`nth-child`.
- Run `yarn lint` and `yarn test:types` after changes; use `PRESERVE_ENV=true yarn test` for reproducible debugging.

Commit & PR expectations
- Commit format: summary (≤80 chars), blank line, `ref`/`fixes` with issue URL, then context.
- Include unit tests for business logic, update stories for UI changes, update i18n for user-visible strings.

What to reference in uncertain cases
- `AGENTS.md` (repo root) — setup & monorepo overview
- `e2e/AGENTS.md` and `e2e/CLAUDE.md` — Playwright test rules and examples
- `apps/shade/AGENTS.md` — UI component patterns and Storybook guidance
- `ghost/admin/lib/asset-delivery` — asset flow for React admin apps

Do: run lint & types; add focused tests; keep changes source-first.
Don't: commit `dist`/`built` artifacts, use `npm`, or use non-semantic Playwright selectors.

If unclear, ask: which area should I expand (architecture, CLI, tests, or UI)?
